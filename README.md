# protocole-vtp
Découverte du protocole VTP

### Qu'est-ce que le protocole VTP?
```
VTP est un protocole de couche 2 qui permet d'ajouter, renommer ou supprimer un ou plusieurs VLAN sur un seul
commutateur (le serveur) qui propagera cette nouvelle configuration à l'ensemble des autres commutateurs du
réseau (clients). VTP permet ainsi d'éviter toute incohérence de configuration des VLAN sur l'ensemble
d'un réseau local.
```
*Note: Un switch ne peut appartenir qu’à un seul domaine VTP.*

## Principe de fonctionnement

![VLAN_Trunking_Protocol](https://user-images.githubusercontent.com/83721477/166197807-1caf86d7-b446-4690-b1ce-432ce881f5de.gif)

```
Les administrateurs peuvent changer les informations de VLAN sur les commutateurs fonctionnant en mode serveur
uniquement. Une fois que les modifications sont appliquées, elles sont distribuées à tout le domaine VTP au
travers des liens « trunk » (Cisco ISL ou IEEE 802.1Q).
```
<hr>

#### VTP fonctionne sur les commutateurs Cisco dans un de ces 3 modes :

* Client: Les switches en mode client appliquent automatiquement les changements reçus du domaine VTP.
* Serveur: Se charge de transmettre les messages VTP
* Transparent: Le switch reçoit les mises à jour et les transmet à ses voisins sans les prendre en compte. Il peut créer, modifier ou supprimer ses propres VLAN mais ne les transmet pas 
<hr>

### Fonctionnement
* Les messages VTP diffuse des annonces de création, de suppression ou de modification de VLAN. Cette diffusion s’effectue à travers tous les switchs grâce à une trame niveau 2 avec une adresse de destination MAC `multicast` bien particulière qui est `01-00-0C-CC-CC-CC`. 
* Ils sont envoyés toutes les 5 minutes ou bien, à chaque fois qu'il y a une modification sur les VLAN.
* La version VTP par défaut activée sur un switch est la 1. Cependant, il existe trois versions VTP différentes.
* Il est possible de modifier le switch pour qu’il exécute la version VTP 2 ou 3. Les différentes versions ne sont pas compatibles entre eux, il faut obligatoirement la même version dans le même domaine VTP

### VTP Version 2:
* Création du mode Transparent : ce switch transmet sans regarder la version VTP
* Les versions 1 et 2 supportent uniquement les VLAN 1 à 1005. Elles peuvent travailler ensemble
excepter au niveau du mode Transparent.

### VTP Version 3:
* Supporte les VLAN 1 au VLAN 4095
* Supporte le PVLAN et le SPAN
* Création du VTP Mode FF
* Synchronisation des database du protocole MST
* Possibilité de mettre un switch en mode "secondary"

<hr>

### Configuration

Pour configurer le VTP, voici les étapes:

1. Obligatoire: configurer un domaine VTP
```
Switch(config)#vtp domain TEST
```
2. Obligatoire: configurer le mode de votre switch (client, transparent ou server)
```
Switch(config)#vtp mode server
Device mode already VTP SERVER.
```
3. Optionnel: activer la fonction pruning
```
Switch(config)#vtp pruning
Pruning switched on
```
4. Optionnel: configurer un mot de passe pour sécuriser les messages VTP
```
Switch(config)#vtp password cisco123
Setting device VLAN database password to cisco123
```
5. Optionnel: activer la version 2 ou 3 de VTP (version 1 active par défaut)
```
Switch(config)#vtp version 2
```
