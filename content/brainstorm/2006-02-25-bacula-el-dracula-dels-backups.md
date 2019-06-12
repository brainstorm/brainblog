---
author: brainstorm
date: '2006-02-24T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889296
excerpt: null
layout: post
modified: '2006-02-24T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/02/25/bacula-el-dracula-dels-backups/
tags:
- howto
- software
- unix
title: "Bacula, el dr\xE0cula dels backups :-["
---

![bacula logo][1]

Primer de tot, no podia deixar d'incloure aquesta frase ("eslógan" oficial de [bacula][2]):

> It comes by night and sucks the vital essence from your computers.

Després d'aquest incís, anem a veure com fer còpies de seguretat dels nostres sistemes amb aquest software de backups tant complet (feu un cop d'ull a la gran quantitat de [documentació][3] i els [dispositius][4] soportats).

Bacula té una estructura [modular][5], en poques paraules hi ha tres daemons:

*   Storage Daemon: S'encarrega de gestionar les particularitats del(s) dispositi(s) físics 
*   File Daemon: Té accés als conjunts de fitxers dels quals hem de fer els backups 
*   Director: Gestiona tots els recursos de forma centralitzada: Amb quina periodicitat s'han de fer backups, sobre quins dispositius, etc... 

Per els impacients, hi ha un [petit tutorial][6] que explica molt ràpidament com muntar bacula. Com que la documentació de bacula és força bona i **molt** extensa, comentaré una mica com m'ho he muntat jo...

<!--more-->

Mentre aneu definint les polítiques del vostre sistema de backups, veureu que els fitxers de configuració es fan força llargs/repetitius, per tant us recomano que feu servir intensivament els "includes" que ofereix bacula per separar les features, més endavant ho comento.

<pre># ls /etc/bacula/
bacula-dir.conf  bacula-fd.conf  bacula-sd.conf  bconsole.conf
</pre>

Mirem primer el bacula-dir.conf per parts:

<pre>Director {                            # define myself
  Name = nopcode-dir
  DIRport = 9101                # where we listen for UA connections
  QueryFile = "/usr/libexec/bacula/query.sql"
  WorkingDirectory = "/var/lib/bacula"
  PidDirectory = "/var/run"
  Maximum Concurrent Jobs = 1
  Password = "&lt;un hash ben llarg&gt;"         # Console password
  Messages = Daemon
}
@/etc/bacula/conf/jobs.conf
@/etc/bacula/conf/filesets.conf
@/etc/bacula/conf/schedules.conf
@/etc/bacula/conf/clients.conf
@/etc/bacula/conf/storage.conf
(...)
@/etc/bacula/conf/pools.conf
</pre>

Aquí podeu veure que bacula usa un port registrat per l'IANA, que usa CRAM-MD5 per autenticar entre components (també podeu usar [TLS][7]) i per últim els includes que he comentat abans començant per "@". Per fer-vos una idea general de com estan interrelacionats aquests fitxers de configuració i les funcionalitats de bacula, us comento una mica la funció de cada fitxer.

# bacula-dir.conf i @'s

Un **job** és una tasca (backup, verificació o restore) que s'executa sobre un conjunt de fitxers **fileset** (/home, p.ex) amb una periodicitat definida per un **schedule** (cada setmana incremental i a principis de mes full, p.ex) sobre un dispositiu d'emmagatzemament **storage** (partició, cinta ...).

Al fitxer jobs.conf defineixo una plantilla que ens servirà per definir jobs similars i no estar repetint la mateixa informació per cadascun d'ells, podent redefinir els paràmetres per cada job concret:

<pre># jobs.conf

JobDefs {
  Name = "DefaultJobTemplate"
  Type = Backup
  Level = Incremental
  Client = nopcode-fd
  FileSet = "sistema-fset"
  Schedule = "WeeklyCycle"
  Storage = File
  Messages = Standard
  Pool = Default
  Write Bootstrap = "/backup/bacula/nopcode.bsr"
  Priority = 10
}

# sistema
# Aquí podriem redefinir variables del DefaultJobTemplate

Job {
  Name = "sistema-job"
  JobDefs = "DefaultJobTemplate"
  Pool = "sistema"
}
</pre>

El **fileset** són els fitxers que volem incloure/excloure en el nostre backup amb opcions addicionals de compressió i/o de generació de hashos MD5 o SHA1 (realment molt útil per fer [auditories de seguretat automatitzades][8] a la tripwire):

<pre># filesets.conf

FileSet {
  Name = "sistema-fset"
  Include {
    Options {
      signature = MD5
      compression = GZIP
    }
    File =  &lt;/etc/bacula/conf/filesets/sistema-incl
  }

  Exclude {
    File = &lt;/etc/bacula/conf/filesets/sistema-excl
  }
</pre>

Fixeu-vos en el modificador "<" darrere del paràmetre File. Indica que la llista de paths per fer backup està en aquest fitxer (una ruta per línea). Un altre modificador interessant és "\<", que permet indicar a bacula que la llista de paths està en el client. Això ens permet que un usuari defineixi les rutes de les quals vol fer backup sense intervenció de l'administrador. Aquesta feature pot ser un inconvenient si l'usuari decideix fer backup de tota la seva col·lecció d'mp3&#8242;s... si es dóna el cas, aneu preparant cintes/hdd's addicionals :-/.

L'schedule especifica *quan* executem els jobs amb una sintaxi força intuïtiva/stándar:

\# schedules.conf

Schedule {  
Name = "WeeklyCycle"  
Run = Full 1st sun at 1:05  
Run = Differential 2nd-5th sun at 1:05  
Run = Incremental mon-sat at 1:05  
}

Les **pool**s són simplement conjunts de **volums**. Habitualment els volums d'una pool són d'un mateix "tipus": sistema, mail, web ... Però també podem definir altres estratègies de backup com per exemple fer una pool per dia (tot el backup d'un dia estarà en una pool). A la documentació de bacula hi ha una [comparativa][9] entre polítiques. Una pool d'exemple podria ser:

<pre>#pools.conf

Pool {
  Name = mail
  Pool Type = Backup
  Recycle = yes
  AutoPrune = yes
  Volume Retention = 90 days    # ~3 months
  Accept Any Volume = yes
  Label Format = "mail-"
  Maximum Volume Bytes = 100m
}
</pre>

Aquesta especificació generarà al disc USB extern que tinc com a unitat de backups, fitxers (volums) de l'estil mail-0001 de 100MB cada un. Si no s'especifica el paràmetre "Maximum Volume Bytes" generarà fitxers tant grans com el backup que realitzem (comprimit si és el cas).

# bacula-sd.conf

L'única secció on heu de tenir especial atenció és la de *Device*, que defineix *com* és el vostre dispositiu de backups (en el meu cas un disc dur extern USB de 250GB):

<pre>Device {
  Name = USB_HDD
  Media Type = File
  Archive Device = /backup/volumes
  Label Media = yes                   # lets Bacula label unlabeled media
  Random Access = Yes
  AlwaysOpen = no
  Automatic Mount = yes
  Removable Media = no
}
</pre>

Compte amb el paràmetre "Automatic Mount", si no el poseu a "yes", preguntarà cada vegada que munteu un nou volum (us enviarà un mail), haureu de loguejar a la bconsole (bacula console) i dir-li mount... poc automatitzat/pràctic, no creieu ?

# bacula-fd.conf

Aquest fitxer el podeu conservar tal com ve de série, modificant el camp Password si cal. És el que instal·lareu als vostres clients i que es comunicarà amb el director.

<pre>Director {
  Name = nopcode-dir
  Password = "&lt; hash llarg &gt;"
}

FileDaemon {                          # this is me
  Name = nopcode-fd
  FDport = 9102                  # where we listen for the director
  WorkingDirectory = /var/lib/bacula
  Pid Directory = /var/run
  Maximum Concurrent Jobs = 20
}
</pre>

S'accepten crítiques/preguntes/idees ! <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_wink.gif" alt=";-)" class="wp-smiley" />

 [1]: http://blogs.nopcode.org/brainstorm/wp-content/images/bacu_logored.jpg
 [2]: http://www.bacula.org/
 [3]: http://bacula.org/?page=documentation
 [4]: http://bacula.org/dev-manual/Supported_Tape_Drives.html
 [5]: http://bacula.org/dev-manual/Customizin_Configurat_Files.html
 [6]: http://bacula.org/dev-manual/Brief_Tutorial.html
 [7]: http://www.bacula.org/rel-manual/Bacula_TLS.html
 [8]: http://www.bacula.org/rel-manual/Using_Bacula_Improv_Comput.html
 [9]: http://bacula.org/rel-manual/Backup_Strategies.html