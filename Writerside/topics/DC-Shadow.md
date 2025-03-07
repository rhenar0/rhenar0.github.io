# DC Shadow

## Définitions

**DCShadow** : Une technique permettant à un attaquant d’introduire un faux controleur de domaine pour modifier des objets Active Directory sans être détecté.

## Introduction

Première apparition publique de l’attaque :

Présenté à la Bluehat IL 2018 conference par Vincent LE TOUX (Inventeur de PingCastle) et Benjamin Delpy (Inventeur de mimikatz)

_Le titre de la conférence :_
**Active Directory: What can make your million dollar SIEM go blind?**

Ils présentent qu'avant leurs travaux, la persistence était déjà présente mais avec une façon complexe de la mettre en place et pas forcément la plus adapté car détectale.

<img src="old_way.png" alt="L'ancienne façon de faire." width="450"/>

## La création

### Objectifs

Introduire un faux contrôleur de domaine pour modifier des objets ou effacer des traces.

### Pourquoi ?
<li>
    <li>Utilise les protocoles natifs de réplication d’AD</li>
    <li>Permet de mettre en place des persistances intéressante</li>
    <li>Aucun logging</li>
</li>

<note>
    <p> 
        <b>Fun Fact</b> 
    </p>
    Cette exploitation n'est pas une vulnérabilité ! Les options utilisés sont documentés par Microsoft eux-même.
</note>

## Pré-requis

Être enregistré en tant que DC dans le domaine :
CN=<host_dc>,CN=Servers,CN=<CN_Fisrt>,CN=Sites,CN=Configuration,DC=<domain_dc>
objectClass : server
dNSHostName : <host_dc>.<domain_dc>
serverReference : CN=<host_dc>,CN=Computers,DC=<domain_dc>
Être capable de demander et/ou de répondre à des appels RPC spécifiques : DRSBind, DRSUnbind, DRSCrackNames, DRSAddEntry, DRSReplicaAdd, DRSReplicaDel, DRSGetNCChanges.


