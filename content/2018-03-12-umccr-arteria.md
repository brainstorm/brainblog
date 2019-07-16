---
author: brainstorm
date: '2018-03-12T16:00:00.000000+00:00'
excerpt: Automating a sequencing center with open source event-driven automation
layout: post
modified: '2018-03-12T16:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2018/03/12/umccr-arteria
tags:
- umccr
- automation
- bioinformatics
title: Event driven automation for DNA sequencing centers with StackStorm and Arteria at UMCCR
---

TL;DR:

1. [Our team][umccr_recruit] can have full control over the Clinical DNA sequencing facility in a timely and effortless manner.
2. Backend engineer interested in challeging and large scale scientific compute? [Join us!][umccr_recruit]
3. Student interested in backend systems and large scale scientific workflows? [apply for this year's summer Google of Code to have CWL inside StackStorm][gsoc2018_stackstorm_cwl_idea], work with us!

## Automate the automation without ventricular fibrillation

Big data analysis nowadays is easy right? You just fire up a beefy VM somewhere, checkout some code and start working... **Wrong!**
We are working with sensitive clinical data and reliably need to produce results that may have a significant effect on the treatment of patients. We cannot afford to waste time on repetitive manual tasks and the undocumented, unrepeatable changes that come with it.

What we do instead is set up a robust and automated system where changes are automatically documented, tested and deployed. We are using [TravisCI][travisci] for testing and [Amazon CodeDeploy][aws_code_deploy] to pick up new deployment bundles once they have passed all tests. New applications then get deployed to an EC2 instance given an [application specification][aws_codedeploy_appspec].

<img src='https://blogs.nopcode.org/brainstorm/images/2018/03/arteria-deployment-tries.png' width=640 height=200>

We care about application "deployability" from the start, since we don't want to have a robust system that is as independent from underlaying infrastructure changes as possible.

Our current iteration is not a fail-safe [Blue-Green deployment setup][aws_bluegreen_deployments] yet, but we plan to [setup ELBs later on when moving into production][aws_elb_pricing]. Also we would like to avoid using CloudFormation and instead go for a [more generic Terraform deployment][terraform_blue_green] soon.

Right now, whenever we push a change, a [st2-docker][st2-docker] container is rebuilt with [our StackStorm packs][umccr_stackstorm_packs] and gets deployed to our EC2 instance. The whole service is built [on top of a docker-compose.yml file][st2-docker-compose], so spinning and tearing down the whole service is fairly straightforward.

If anything breaks, we just delete our whole EC2 instance and the predefined AWS autoscaling group re-provisions a fresh instance setting up everything automatically.

Oh, and those pesky SSL [certificate renewal tasks that can disrupt production systems when unattended][occulus_cert_renewal]? [Letsencrypt and docker does it all for us][letsencrypt_docker] because while [AWS Certificate manager][aws_cert_manager] is indeed free, **it can only be attached to not-so-free [ELB traffic pricing][aws_elb_pricing]** and it would mean a tighter coupling to AWS, [which we try to avoid][wikipedia_cloud_lockin].

I can hear *cloud first* folks asking now: "But you could implement all that in a few Lambda functions, API Gateways and load balancers, right?".

Yes, one could. But that would bind yourself to one vendor. What if, for practical or regulatory reason, you want to switch provider, use multiple providers or run part of your pipelines in-house? You would have to start from scratch or implement and run multiple setups on multiple platforms. As a example, currently the prices for [AWS Glacier storage are the cheapest among commercial cloud providers][aws_glacier_cheapest], but what if we wanted to implement a hybrid model with [minio backing up clinical cases on different premises][minio]? Or parts of the data cannot leave strictly controlled in-house environments?

## The heart of the integration problem

After working for sequencing centres for a while, operational (anti-)patterns emerge. Integration and automation are key in order to deal with the often repeated ordeal of data management in biosciences. My work as a research software engineer consists of easing the scale and reproducibility of scientific work.

What this means in practice is buying time for scientists so that they can aim higher with their goals. Technically it means using event-based tools such as StackStorm:

> StackStorm is a powerful open-source automation platform that wires together all of your apps, services and workflows. It’s extendable, flexible, and built with love for DevOps and ChatOps.

Thus we can free members of our team from tedious and error prone data mangling, leaving more free time to dedicate to more rewarding research.

## From arteriae...

Thankfully other smarter folks in Uppsala realised those integration needs earlier and [Johan Dahlberg][johan_dahlberg] started putting together middleware and packs for StackStorm, naming it ["the arteria project"][arteria_basic_context].

In this way, common operations could be **abstracted and written once**, to be reused by other teams. A small community emerged developing and sharing tool packs similar to the [StackStorm-exchange][stackstorm_exchange] but with a narrower and more focused aim: DNA sequencing facilities.

## ...to the capillary details of data

### Detecting new data

The arteria project provides a host of useful tools and routines to help with the common tasks of a sequencing centre. One of those tools is a [runfolder monitor][arteria_runfolder], which watches a defined directory for new sequencing data and keeps track of its state. 

Our central StackStorm orchestrator queries this service and raises an event every time new sequencing data is available. Based on this event, downstream data processing steps can be initiated for each new dataset. 

This can happen autonomously, efficiently, reliably and fully audited. 

There are no out-of-working-hours delays, no menial tasks for researchers to perform, **no inconsistencies due to slight differences in manual steps**.

### Pre-process and provision the data for analysis

The raw data from the sequencing machine first needs to be converted to a standard format that is recognised by the analysis tools in the downstream pipelines. In our case this is achieved with the [bcl2fastq][bcl2fastq] tool from Illumina. 

We have wrapped this tool in a [Docker container][bcl2fastq_docker] for portability. A simple bash script acts as an interface between the StackStorm controller and the container, thus allowing the seamless integration with other tasks of our data processing pipeline:

1. After a new dataset has been detected, a StackStorm rule calls the script to trigger the data conversion. 
2. The script calls back to the StackStorm instance allowing further steps to happen without human intervention. 
3. In case of a conversion success for instance the data is then automatically transferred to the compute cluster for analysis.

### Analysis pipeline(s)

There are several post processing steps to be taken to ensure quality, comparability and usefulness of the data to researchers and clinicians beyond our team. 

For instance, [UMCCRISE is filling file munging and reporting gaps with useful SnakeMake recipes][umccrise] that are executed after [the bcbio pipeline][bcbio] finishes its job.

### Data and results dissemination

The data lifecycle does not stop at the results though. Data and results need to be available in a way that clinicians, collaborators and other parties can easily access it and it needs to be safe and reliably stored for future reference. We plan to put data distribution modules in place that automate this process and make sure we comply with regulatory requirements as well as community demands.

For bigger scientific payloads and workflow, stay tuned for our ~upcoming~ [PCGR integration work within the StackStorm framework][umccr_pcgr_integration].

## Issues clogging our vessels and how you can help us

Here's a brief summary of our Trello, as a TODO list you could help us with [if you are willing to participate in this year's Google Summer of Code][gsoc2018_stackstorm_cwl_idea]? Take one of them and impress us with your draft proposal and/or implementation of:

1. CWL support on StackStorm: Writing workflows in one portable common language should be possible!
1. Join the StackStorm's community [efforts moving towards Kubernetes 1 process per container app testing][st2_kubernetes_1ppc].
1. <acronym title='Rule Based Access Control'>RBAC</acronym>: Using AAA/SSO service such as [KeyCloak][keycloak] for better access control.
1. Central application state handling, workflow state handling by [implementing GA4GH's WES events and endpoints that integrate with StackStorm][wes_service].
1. Make sure (re-)deployments are safe with respect to data persistence: database migrations, environment variables and assets.
1. Turn the arteria runfolder microservice into poll-less implementation by using [filesystem notifications with i.e fsmon][fsmon].

[letsencrypt_docker]: https://charliedrage.com/letsencrypt-on-docker
[umccr_stackstorm_packs]: https://github.com/umccr/stackstorm-umccr
[st2-docker]: https://github.com/umccr/st2-docker-umccr/
[st2-docker-compose]: https://github.com/umccr/st2-docker-umccr/blob/master/docker-compose.yml
[aws_glacier_cheapest]: https://richardburley.com/amazon-glacier-vs-google-nearline-vs-azure-storage/
[umccr_recruit]: https://umccr.github.io/
[arteria_basic_context]: https://arteria-project.github.io/
[rexray]: https://rexray.thecodeteam.com/
[st2_docker_vault]: https://github.com/StackStorm/st2-docker/pull/65
[st2_kubernetes_1ppc]: https://github.com/StackStorm/st2-docker/tree/master/runtime/kubernetes-1ppc
[minio]: https://minio.io/
[keycloak]: https://www.keycloak.org/
[travisci_codedeploy]: https://github.com/umccr/st2-docker-umccr/blob/master/.travis.yml
[aws_code_deploy]: https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html
[aws_codedeploy_appspec]: https://github.com/umccr/st2-docker-umccr/blob/master/appspec.yml
[aws_bluegreen_deployments]: https://d0.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf
[aws_elb_pricing]: https://aws.amazon.com/elasticloadbalancing/pricing/
[aws_cert_manager]: https://aws.amazon.com/certificate-manager/
[stackstorm_exchange]: https://exchange.stackstorm.org/
[gsoc2018_stackstorm_cwl_idea]: https://obf.github.io/GSoC/ideas/#integration-of-cwl-support-into-stackstorm-automation-framework
[travisci]: https://travis-ci.org/
[arteria_runfolder]: https://github.com/arteria-project/arteria-runfolder
[bcl2fastq]: https://sapac.support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software.html
[bcl2fastq_docker]: https://github.com/umccr/bcl2fastq-docker
[umccrise]: https://github.com/umccr/umccrise.git
[bcbio]: https://bcb.io/
[fsmon]: https://github.com/nowsecure/fsmon
[terraform_blue_green]: https://medium.com/@kemra102/blue-green-deployments-in-aws-with-terraform-2755942d4090
[wikipedia_cloud_lockin]: https://en.wikipedia.org/wiki/Vendor_lock-in
[johan_dahlberg]: https://uppsala-bioinformatics.se/
[occulus_cert_renewal]: https://techcrunch.com/2018/03/07/all-of-oculuss-rift-headsets-have-stopped-working-due-to-an-expired-certificate/
[wes_service]: https://pypi.python.org/pypi/wes-service/2.1
[umccr_pcgr_integration]: https://blogs.nopcode.org/brainstorm/2018-03-13-umccr-pcgr/
