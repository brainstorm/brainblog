---
author: brainstorm
date: '2018-03-12T20:00:00.000000+00:00'
excerpt: How UMCCR automated cancer report generation by depositing VCF files in S3 buckets and a couple of lambdas
layout: post
modified: '2018-03-12T20:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2018/03/13/productionalizing-cancer-reporting
tags:
- umccr
- automation
- bioinformatics
title: Productionalising cancer reporting
---

[In a previous blogpost][umccr_arteria] I showed how [StackStorm][stackstorm] can fill some gaps in scientific application integration. Here I will describe how one of those applications looks like from the inside.

Sometimes [scientific software cannot be decomposed in a reasonable time][vms_containers_or_serverless] for more general and scale-proof consumption. The amount of **engineering required to re-architecture some scientific tools is often underestimated**.

Instead, wrapping a minimum viable "macro"-service monolith with lean conventions and APIs might be a more sensible alternative in a fast paced research environment like genomics. In our case the [Personal Cancer Gene Reporter][pcgr] involves several gigabytes of annotation data, docker containers and a flurry of R, perl and python dependencies. Brace for impact.

### Scaffolding PCGR for deployment

PCGR is a cancer functional annotation and reporting package that demands non-trivial amount of processing to install, provision data (multiple gigabytes downloaded from Google Drive urls changing on each release) and make sure that required configurations and dependencies are in place.

To streamline this process, we [use Ansible][ansible] to provision new machines, see UMCCR's [pcgr-deploy][pcgr_deploy] repository.

Since a one-size fits all solution doesn't always work, the pcgr-deploy Ansible repository also allows for deployments via: [AWS][pcgr_deploy_aws], [AWSCLI][pcgr_deploy_awscli], [OpenStack][pcgr_deploy_openstack] and [VirtualBox][pcgr_deploy_vagrant].

However, `pcgr-deploy` is just one step towards an end-user friendly solution and a quite time consuming one. With Ansible it still takes a good **40 minutes** before we can run cancer samples. To speed up the process we create snapshots of provisioned PCGR instances, which can be reused in subsequent analyses. This reduces the waiting time to **a few seconds** while the fresh virtual machine is starting.

So we can quickly fire up a machine with our PCGR image and start working, i.e. upload our data, run analysis, retrieve the results and shut down the instance again.

Alternatively, we can automate this manual process further and potentially encourage other researchers and RSEs to do the same or better:

{{<tweet 969742149334921217>}}

### Generating a cancer patient report

Ideally, all the user should have to do is make his input data available. The rest should happen automatically. We achieve that with the help of StackStorm and AWS lambda functions.

#### Serverless code

First the user supplies the data in a pre-defined S3 bucket following naming conventions:

```bash
{sample_batch}-{6 digit uuid}-somatic-tar.gz
{sample_batch}-{6 digit uuid}-normal.tar.gz
```

Each of these files contains the `.vcf.gz` variant files and PCGR [`.toml` configuration][toml_format] necessary to carry out the PCGR analysis. So at the end of a [cancer variant calling pipeline like bcbio][bcbio_cancer_variantcall], a tarball with the aforementioned files can be generated.

On the [serverless][serverless] side, a [lambda function][aws_umccr_lambda] registers the arrival of the new files via the [S3 `ObjectCreated:*`][aws_object_created] and filters to those containing `-somatic.tar.gz` and `-normal.tar.gz` prefix. An event is then generated which triggers the function below, calling back to our StackStorm instance and queuing a message for processing:

```python
import os
import json
import http.client
from urllib import parse, request
import boto3

s3 = boto3.client('s3')
sqs = boto3.resource('sqs', region_name='ap-southeast-2')

st2_host = os.environ.get("ST2_HOST")
st2_url = os.environ.get("ST2_API_URL")
st2_api_token = os.environ.get("ST2_API_KEY")
queue_name = os.environ.get("QUEUE_NAME")

headers = {
    'St2-Api-Key': st2_api_token,
    'Content-Type': 'application/json',
}

def st2_callback(fname):
    """ Just a HTTP POST query to StackStorm server with minimal payload
    """
    
    connection = http.client.HTTPSConnection(st2_host)

    params = json.dumps({"trigger": "pcgr.up", "payload": {"status": "done", 
                                                           "task": "instantiate", 
                                                           "fname": fname}})
    connection.request("POST", "/api/v1/webhooks/st2", params, headers)
    
    # We do actually have to care about the response
    response = connection.getresponse()
    data = response.read()
    print(data)
    connection.close()

def queue_sample(fname):
    try:
        queue = sqs.get_queue_by_name(QueueName=queue_name)
    except:
        queue = sqs.create_queue(QueueName=queue_name, Attributes={'DelaySeconds': '5'})

    response = queue.send_message(MessageBody=fname)
    print(response)

def lambda_handler(event, context):
    print("Received event: " + json.dumps(event, indent=2))

    # Get the object from the event and show its content type
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = parse.unquote_plus(event['Records'][0]['s3']['object']['key'], encoding='utf-8')
    try:
        response = s3.get_object(Bucket=bucket, Key=key)
        
        # Queue samples to be processed in AWS SQS Queue
        queue_sample(key)

        # Tell StackStorm about the new file in the bucket 
        # so that it can start the corresponding workflow
        st2_callback(key)
        
        return key


    except Exception as e:
        print(e)
        print('Error getting object {} from bucket {}. Make sure they exist and your bucket is in the same region as this function.'.format(key, bucket))
        raise e
```

The [UMCCR lambda function][aws_umccr_lambda] simply retrieves an API token for our StackStorm server from the lambda environment variables and HTTP POST the StackStorm instance that a new file waits to be processed. Those tokens and other sensitive information are of course [held encrypted at rest][aws_lambda_encrypted] and never burned into any image for obvious security reasons.

As mentioned, our lambda function also adds a message to a queue (in this case, Amazon SQS, for now) so that [the consumer script in the instance][pcgr_consumer_script] can start processing samples as soon as it is up and running. To make sure that the consumer script is running after boot, a [systemd service definition is provisioned by ansible][pcgr_systemd_service] on AMI creation time, [enabled on boot but disabled for provisioning][ansible_systemd_detail].

Furthermore, since the devil is in the details and **we are working with patient cancer data**, no instance image is put into production (and potentially published) if test runs and variant files have been processed by that instance before. In other words, re-provisioning the AMI easily and in a repeatable way is paramount to ensure that our [data is safe from image forensic analysis][image_forensic_analysis].

If you are a busy person in need for some automation, you can probably stop reading here and point the lambda above to some [CloudFormation][aws_cloudformation] or [launch configuration][aws_launch_configuration] if you are working on Amazon.

#### Stackstorm

On the other hand, if you want to keep cloud vendor lock-in at bay, stay with StackStorm. On the StackStorm side of the equation, the event from the lambda is received and our integrations from the [stackstorm-pcgr pack][pcgr_stackstorm] react and provision PCGR on an AWS instance:

```yaml
---
chain:
  - name: "up"
    ref: ansible.playbook
    parameters:
      playbook: "/opt/stackstorm/packs/pcgr/ansible/ansible/aws.yml"
      extra_vars:
           - "ansible_python_interpreter=/opt/stackstorm/virtualenvs/ansible/bin/python"
           - "inventory_file=/opt/stackstorm/packs/pcgr/ansible/ansible/inventory/aws/ec2.py"
    notify:
      on-success:
        routes:
          - umccr_slack
        message: "PCGR AWS instance up and running"
      on-failure:
        routes:
          - umccr_slack
        message: "Something went wrong instantiating PCGR on AWS and/or processing your sample"
```

A similar trigger and a lambda are used with `-output.tar.gz` suffixes created on the same bucket to notify the user that the processing has been completed successfully and where and how they can download such results.

We even save [some bucks in the process with Spot instances][aws_netflix_spot_instances], if configured accordingly in Ansible after [querying the spot market with a small python script][aws_spot_duration_script]:

```yaml
#Amazon
aws:
    instance_type: m4.xlarge
    instance_profile_name: pcgr
    spot_price: 0.6
    image_id: ami-5921d73b # PCGR v0.5.3
    security_group: default
    region: ap-southeast-2
    zone: ap-southeast-2a
    volume_device: /dev/xvdb
    volume_size: 50
```

When the job is finished and no more samples are left to process on the S3 bucket, [the instance shuts down by itself][aws_boto3_autoshutdown].


### Future work

To close the loop of productionalising a scientific tool like PCGR that is under active development, we would need to **automate the automation** so that each [new release of PCGR][pcgr_releases] gets automatically built into a ready-to-use VM image and does not eat away our precious time that is better spent on other research topics.

It is my hope that as time goes by, some of the practices described here become so obvious to anybody involved that it becomes standard practice.

[umccr_arteria]: https://blogs.nopcode.org/brainstorm/2018/03/12/umccr-arteria
[pcgr]: http://github.com/sigven/pcgr
[pcgr_breast_tumor_sample]: http://folk.uio.no/sigven/tumor_sample.BRCA.0.5.3.pcgr.html
[pcgr_deploy]: https://github.com/umccr/pcgr-deploy
[pcgr_deploy_aws]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/aws.yml
[pcgr_deploy_openstack]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/openstack.yml
[pcgr_deploy_vagrant]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/Vagrantfile
[pcgr_deploy_awscli]: https://github.com/umccr/pcgr-deploy/tree/master/aws/cli
[aws_umccr_lambda]: https://github.com/umccr/pcgr-deploy/blob/master/aws/lambda/trigger_pcgr.py
[aws_lambda_encrypted]: https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html#env_encrypt
[aws_object_created]: https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html#notification-how-to-event-types-and-destinations
[aws_spot_duration_script]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/scripts/get_spot_duration.py
[aws_netflix_spot_instances]: http://highscalability.com/blog/2017/12/4/the-eternal-cost-savings-of-netflixs-internal-spot-market.html
[pcgr_releases]: https://github.com/sigven/pcgr/releases
[serverless]: https://serverless.com/
[pcgr_stackstorm]: https://github.com/brainstorm/stackstorm-pcgr
[vms_containers_or_serverless]: http://rishidot.com/krishnan/app-development/vms-containers-or-serverless/
[pcgr_consumer_script]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/files/pcgr_consumer.py
[pcgr_systemd_service]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/files/pcgr.service.j2
[image_forensic_analysis]: http://www.forensicswiki.org/wiki/Main_Page
[aws_cloudformation]: https://aws.amazon.com/cloudformation/
[aws_launch_configuration]: https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchConfiguration.html
[stackstorm]: https://stackstorm.com/
[ansible]: https://www.ansible.com
[toml_format]: https://en.wikipedia.org/wiki/TOML
[bcbio_cancer_variantcall]: https://bcbio-nextgen.readthedocs.io/en/latest/contents/pipelines.html#cancer-variant-calling
[ansible_systemd_detail]: https://github.com/umccr/pcgr-deploy/commit/50f6150a995dd3f7395b1622abe6e1559c7947b5#diff-1ce6d209088f4d133c282b1df1cb0ed7R191
[aws_boto3_autoshutdown]: https://github.com/umccr/pcgr-deploy/blob/master/ansible/files/pcgr_consumer.py#L147
