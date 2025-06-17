# Introduction
Dans les systèmes embarqués modernes, la collecte et l’analyse de données en temps réel sont essentielles pour le suivi et la sécurité des équipements, notamment dans des domaines critiques comme l’automobile, l’aviation ou le ferroviaire. S’inspirant du fonctionnement des boîtes noires utilisées dans ces industries, ce projet a pour objectif de concevoir un système embarqué capable d’enregistrer et de transmettre en temps réel des données de mouvement (vitesse et orientation) à l’aide d’un capteur inertiel. Les informations sont ensuite visualisées sur une station de contrôle via un écran LCD.
Ce projet s’inscrit dans le cadre du Tekbot Robotics Challenge et fait appel à plusieurs compétences majeures : programmation directe des microcontrôleurs ATmega328P sans utiliser de carte Arduino, communication via le protocole I2C, conception de circuits imprimés avec KiCAD, réalisation d’un boîtier cubique de 7 cm, intégration matérielle sur veroboard ou PCB, ainsi que la conception d’une alimentation spécifique.
# Sommaire

- [cahier des charges ](#Cahier-des-charges)
- [Schéma synoptique](#Schéma-synoptique)
- [Réalisation du PCB](#Réalisation-du-PCB)
- [Description fonctionnelle des différents blocs du système](#Description-fonctionnelle-des-différents-blocs-du-système)
  
# Cahier des charges

## Objectifs fonctionnels
- Lire les données de vitesse et d’orientation à l’aide du MPU6050
- Utiliser le microcontrôleur ATmega328P sans carte Arduino
- Concevoir un circuit imprimé (PCB) et une alimentation autonome
- Transmettre les données à une station de contrôle via I2C
- Afficher les données sur un écran LCD en mode 4 bits

## Contraintes techniques
- Le cube doit mesurer 7x7x7 cm, avec une face ouverte pour voir le circuit
- Le microcontrôleur du cube agit en **maître I2C**
- Le microcontrôleur de la station agit en **esclave I2C**
- Alimentation externe obligatoire, hors du cube
- Schéma et PCB réalisés avec **KiCad**

## Matériel utilisé

- [ATmega328P](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf) ×2  
- [MPU6050](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf)  
- [LCD 16x2 (mode 4 bits)](https://www.gotronic.fr/pj2-sbc-lcd16x2-fr-1441.pdf?srsltid=AfmBOopmg8VyH8PQXxRcqE7GEvoyRwGRHeKVU9ZsKwGmKu13oZXhPhaJ)  
- Alimentation

  
# Schéma synoptique
Ce système embarqué est composé de deux unités : une boîte noire et une station de contrôle, connectées via le bus I2C. La boîte noire comprend un capteur MPU6050 qui détecte les mouvements de la main et envoie les données à un microcontrôleur ATmega328P configuré en maître. Celui-ci traite les données et les transmet à la station de contrôle, où un autre ATmega328P, configuré en esclave, les reçoit. Les informations sont ensuite affichées sur un écran LCD. Chaque unité est alimentée séparément par une source de 5V.

![schéma synoptique](https://github.com/user-attachments/assets/8b703337-a753-49e2-947b-a988aeb19c89)

# Description fonctionnelle des différents blocs du système
## Bloc d’alimentation
## 1. Fonction principale
Ce bloc a pour objectif de fournir une tension continue et stable de 5V nécessaire au fonctionnement des différents composants électroniques du système, notamment le capteur MPU6050, les microcontrôleurs ATmega328P et l'écran LCD.
## 2. Fonctionnement
On utilise trois batteries de Li-Ion 3,7V rechargeables montées en série pour obtenir environ 12V.  
Le régulateur L7805 reçoit ce 12V en entrée (Vin) et fournit du 5V en sortie (Vout).  
Deux condensateurs (330nF et 100nF) sont utilisés pour stabiliser la tension et filtrer les parasites.
- 330nF à l’entrée
- 100nF à la sortie

📌 Le schéma suivant illustre ce montage :
![image](https://github.com/user-attachments/assets/690d4b70-85dd-4cbb-bff8-a1c31e7033fd)

## Bloc d’entrée(dans la boite noire)

Nous avons choisi le capteur MPU6050 parce qu’il intègre un accéléromètre et un gyroscope dans un seul composant. Cela permet de mesurer les mouvements et les rotations de la main avec précision. Il est facile à utiliser grâce au protocole I2C, et il fonctionne bien avec le microcontrôleur ATmega328P. En plus, il est peu coûteux et largement utilisé dans les projets embarqués. D’autres capteurs peuvent faire un travail similaire, comme le MPU9250 (qui ajoute un magnétomètre), le LSM6DS3 (plus récent et plus économe), ou le ADXL34

![image](https://github.com/user-attachments/assets/2c989400-0c29-46dc-907c-dcd7208a431b)
## Bloc de commande(dans la boite noire)
Avant d’intégrer le microcontrôleur ATmega328P dans la boîte noire et de concevoir le circuit imprimé, nous avons d’abord réalisé plusieurs essais pour nous familiariser avec ce composant, car nous avions l’habitude d’utiliser des cartes Arduino.
Nous avons donc simulé des circuits simples sur Proteus, notamment un montage avec l’ATmega328P, un bouton poussoir, un relais et une lampe 12V. Le principe était le suivant : lorsqu’on appuie sur le bouton, le relais active l’alimentation de la lampe, qui s’allume. Cette étape nous a permis de comprendre le fonctionnement du microcontrôleur et son pilotage direct sans carte Arduino.

# La communication I2C
Ce document constitue une présentation détaillée et approfondie du protocole **I2C (Inter-Integrated Circuit)**, qui est un standard de communication série synchrone très répandu dans l’électronique embarquée. Ce protocole facilite l’échange d’informations entre un ou plusieurs maîtres et plusieurs périphériques esclaves en utilisant seulement deux fils, simplifiant ainsi les connexions matérielles tout en assurant une communication fiable et efficace.  


# Sommaire

- [📘 Introduction](#-introduction)
- [⚙️ Principe de fonctionnement](#-principe-de-fonctionnement)  
  - [🔹 Prise de contrôle du bus](#-prise-de-contrôle-du-bus)  
  - [🔹 Transmission d'un octet](#-transmission-dun-octet)  
  - [🔹 Transmission d'une adresse](#-transmission-dune-adresse)  
  - [🔹 Écriture d'une donnée](#-écriture-dune-donnée)  
  - [🔹 Arbitration](#-arbitration)
  - [🔹 Clock Stretching](#-clock-stretching)
- [📡 Communication I2C entre MPU6050 et ATmega328P](#-communication-i2c-entre-mpu6050-et-atmega328p)  
  - [🔹 Fonctionnement de la liaison I2C](#-fonctionnement-de-la-liaison-i2c)
  - [🔹 Envoi des commandes et réception des données](#-envoi-des-commandes-et-reception-des-donnees)


---

## 📘 Introduction
Dans les systèmes électroniques modernes, les microcontrôleurs, capteurs, mémoires, convertisseurs de données et autres circuits intégrés doivent souvent échanger des informations entre eux.
Pour assurer une transmission fiable, rapide et structurée des données, on utilise des protocoles de communication (c'est un ensemble de règles et de conventions définissant la manière dont les données sont échangées entre les différents composants d’un système).
On distingue deux types principaux :  
**-Parallèle** : rapide, mais nécessite plusieurs fils.  
**-Série** : plus simple, utilise moins de fils, adaptée aux systèmes embarqués.  
Parmi les protocoles série les plus connus : **UART,SPI,I2C,CAN,LIN...**    
Parmi les protocoles série les plus utilisés, **le protocole I2C** se distingue par sa simplicité, son faible coût de mise en œuvre, et sa capacité à connecter de nombreux périphériques avec seulement deux lignes. Voyons maintenant de plus près son fonctionnement.  
👉 Voyons maintenant de plus près son fonctionnement.


---

## ⚙️ Principe de fonctionnement
[Protocole I2C](https://fr.wikipedia.org/wiki/I2C) (Inter-Integrated Circuit), développé par Philips (aujourd’hui NXP) dans les années 1980, est un standard mondial pour la communication série entre circuits intégrés, surtout dans les systèmes embarqués. Il utilise un bus bidirectionnel à deux fils : SDA pour les données et SCL pour l’horloge, permettant à plusieurs périphériques de partager le même canal tout en gérant précisément l’accès.  
Contrairement à des protocoles comme SPI, I2C minimise le nombre de connexions nécessaires, ce qui simplifie le routage sur circuit imprimé et réduit les coûts.   Ce protocole est largement utilisé dans des domaines variés : automobile, domotique, informatique, etc.  
I2C fonctionne selon un modèle.[maître-esclave](https://www.ionos.fr/digitalguide/serveur/know-how/le-principe-master/slave): un ou plusieurs maîtres contrôlent la communication, et les esclaves répondent aux requêtes. Chaque communication commence par une condition **Start**, suivie de **l’adresse de l’esclave** et d’un bit de direction (lecture/écriture). Les données sont ensuite échangées octet par octet, chaque octet étant confirmé par **un bit d’acquittement**(ACK). La communication se termine par une condition **Stop**, qui libère le bus.  
Techniquement, I2C utilise des lignes ouvertes (open-drain) : les dispositifs ne peuvent que tirer les lignes vers le bas, tandis que des résistances pull-up maintiennent le niveau haut par défaut. Cela évite les conflits, notamment en mode multi-maîtres.  
Enfin, I2C offre une grande flexibilité en termes de vitesse, du mode standard (100 kHz) au mode rapide (jusqu’à 3,4 MHz) et au-delà dans certaines variantes propriétaires.  
![communication-I2C](https://github.com/user-attachments/assets/42f1202c-0ec3-4e69-a15c-abace28aff09) 


---

### 🔹 Prise de contrôle du bus

La prise de contrôle du bus par un maître débute par une **condition Start (S)**, qui est un événement distinctif sur le bus. Cette condition correspond à une transition sur la ligne SDA de l’état haut à l’état bas, alors que la ligne SCL est maintenue à l’état haut. Cette séquence particulière est détectée par tous les périphériques connectés au bus, qui entrent alors en mode écoute, prêts à recevoir des données.  

La condition Start joue un rôle fondamental : elle marque l’exclusivité du maître sur le bus, ce qui évite les collisions ou l’interférence avec d’autres maîtres éventuels. Elle sert aussi de synchronisation initiale à la transmission de données, en assurant que tous les appareils sont synchronisés sur le début de la communication.  

Les résistances pull-up sur les lignes SDA et SCL maintiennent ces lignes à un état logique haut par défaut, garantissant ainsi que le bus est en repos quand aucune communication n’a lieu.

## 🖼️ Image : BUS I2C 
![Bus-i2c](https://github.com/user-attachments/assets/56dc1d36-e7e6-44f3-b15a-36ae52d9d87d)  
## 🖼️ Image : condition start
![start-condition](https://github.com/user-attachments/assets/c1bdf777-61d8-4f95-a602-330b61cba147) 

---

### 🔹 Transmission d'un octet

La transmission des données sur le bus I2C s’effectue par octets (8 bits). Chaque bit est transmis séquentiellement, en commençant par le bit le plus significatif (MSB).  

Le protocole impose que chaque bit soit placé sur la ligne SDA pendant que la ligne SCL est à l’état bas. Ensuite, la ligne SCL passe à l’état haut, moment où le récepteur lit la valeur présente sur la ligne SDA. Lorsque la ligne SCL redescend à l’état bas, l’émetteur peut placer le bit suivant sur SDA, et ainsi de suite.  

Après l’envoi des 8 bits d’un octet, la ligne SDA est libérée pendant le 9e cycle d’horloge. C’est alors au récepteur de signaler par un bit d’acquittement (ACK) s’il a correctement reçu l’octet, en tirant la ligne SDA à l’état bas. S’il ne tire pas SDA à zéro, un bit de non-acquittement (NACK) est détecté, ce qui indique que la communication doit être interrompue ou qu’une erreur s’est produite.  

Ce mécanisme d’**ACK/NACK** est crucial car il assure la fiabilité des transmissions, permettant au maître de savoir si l’esclave est disponible et prêt à recevoir ou envoyer des données.

---

### 🔹 Transmission d'une adresse

Après la condition Start, le maître envoie un octet d’adresse pour identifier l’esclave avec lequel il souhaite communiquer. L’adresse est généralement codée sur 7 bits, suivis d’un bit R/W indiquant si la transaction sera une lecture ou une écriture.  

Le protocole prévoit aussi une extension 10 bits pour les réseaux comportant un grand nombre de périphériques, mais cette extension est moins fréquemment utilisée.  

Tous les périphériques esclaves surveillent le bus et comparent l’adresse reçue avec leur propre adresse. Celui qui reconnaît son adresse répond alors par un bit ACK en tirant la ligne SDA à l’état bas pendant le 9e bit. Les autres esclaves restent silencieux jusqu’à la prochaine séquence.  

Cette étape est essentielle car elle garantit que seules les communications destinées à un périphérique spécifique sont traitées, évitant ainsi toute interférence entre plusieurs périphériques sur le même bus.
## 🖼️ Image : transmission d'une adresse  
![transmission_d'une_adresse](https://github.com/user-attachments/assets/426c9eaf-1dc3-4bfc-bb9d-1b4b468bced7)

---

### 🔹 Écriture d'une donnée

Une fois l’adresse reconnue par l’esclave, la phase de transfert des données peut commencer. Le maître transmet alors les octets de données à l’esclave, chaque octet étant suivi d’un bit ACK envoyé par l’esclave pour confirmer la bonne réception.  

Le protocole permet d’envoyer autant d’octets que nécessaire dans une même communication, ce qui permet des transferts efficaces et continus.  

Pour terminer la communication, le maître génère une condition **Stop (P)**, qui correspond à une transition de la ligne SDA de l’état bas à l’état haut alors que la ligne SCL est haute. Cette séquence indique à tous les périphériques que la transmission est terminée et que le bus est libéré pour une autre communication.  

Il existe également une condition **Restart**, qui est une condition Start générée sans condition Stop préalable, permettant de chaîner plusieurs opérations sur le même bus sans interruption.  
## 🖼️ Image : transmission d'une donnee
![transmission-d'une-donnee](https://github.com/user-attachments/assets/51e00c84-0cf6-4005-928a-f2d0413f72a8)  
## 🖼️ Image : condition stop
![stop-condition](https://github.com/user-attachments/assets/a3bcc978-09d5-408d-a788-68827986eedc) 

---

### 🔹 Arbitration

Le protocole I2C est conçu pour supporter un mode multi-maîtres, où plusieurs maîtres peuvent tenter d’accéder au bus simultanément. Pour éviter les conflits, un mécanisme d’arbitrage est mis en place.  

Lorsqu’un maître commence à transmettre, il surveille la ligne SDA et la compare avec ce qu’il souhaite envoyer. Si un maître détecte que la ligne SDA est forcée à l’état bas par un autre maître alors qu’il tente de la maintenir haute, il comprend qu’il a perdu l’arbitrage et abandonne immédiatement la transmission, laissant le bus libre au maître dominant.  

Ce mécanisme garantit qu’aucune collision électrique ne se produit sur le bus et que seule une source transmet à un instant donné. C’est une des forces du protocole I2C, qui permet une coexistence harmonieuse de plusieurs maîtres sur un même bus.
## 🖼️ Image : arbitrage
![arbitrage](https://github.com/user-attachments/assets/2b7e7f8a-e0f6-46b2-9b76-5f87cdfb8dcb)



### 🔹 Clock Stretching

Le clock stretching est une fonctionnalité du protocole I2C qui permet à un esclave de ralentir temporairement la communication lorsqu’il n’est pas prêt à envoyer ou recevoir des données. Cela se fait en gardant la ligne SCL à l’état bas (LOW), empêchant ainsi le maître de continuer à envoyer des impulsions d’horloge. Une fois que l’esclave est prêt, il libère la ligne SCL, permettant au maître de reprendre la transmission. Cette technique est utile, par exemple, lorsque le capteur a besoin de plus de temps pour traiter ou préparer les données. Le maître doit respecter cet étirement d’horloge pour éviter des erreurs de communication.  

## 🖼️ Image : clock stretching
![clock streatching](https://github.com/user-attachments/assets/1149606a-e454-484b-9eb4-7030ac5e0d8f)  
---
## 📡 Communication I2C entre MPU6050 et ATmega328P
Dans notre projet, le microcontrôleur **ATmega328P** communique avec le capteur **MPU6050** à l’aide du protocole **I2C**. Ce protocole permet de transmettre les données d’accélération et de rotation via deux fils (`SDA` et `SCL`). Le **MPU6050** agit comme **esclave**, et l’**ATmega328P** comme **maître**.

---

### 🔹 Fonctionnement de la liaison I2C

La connexion matérielle entre le MPU6050 et l’ATmega328P s’effectue via le protocole I2C (Inter-Integrated Circuit), qui utilise deux lignes de communication :   
- SDA (Serial Data Line) : ligne bidirectionnelle pour l’échange des données, connectée à la broche PC4 de l’ATmega328P.    
- SCL (Serial Clock Line) : ligne d’horloge générée par le maître, connectée à la broche PC5 de l’ATmega328P.

Sur le bus I2C, le dispositif qui initie la communication est appelé maître, tandis que celui qui répond s’appelle esclave. Dans notre cas :    
L’ATmega328P joue le rôle de maître, c’est lui qui contrôle le bus, génère l’horloge, et initie les échanges.    
Le MPU6050 est l’esclave, il attend que le maître lui demande des données spécifiques.    
Le maître démarre la communication en envoyant une adresse unique correspondant à l’esclave (ici l’adresse I2C du MPU6050, généralement 0x68), suivie d’une commande indiquant quel registre ou donnée il souhaite lire.  
                          
| ATmega328P | MPU6050 |
|------------|---------|
| PC4 (SDA)    | SDA     |
| PC5 (SCL)    | SCL     |
| VCC        | 5V  |
| GND        | GND     |
| 🔧 Pull-up | Recommandé : 4.7kΩ sur SDA et SCL |  

## 🖼️ Image : liaison-i2c-entre-mpu6050-et-atmega328p
![liaison-entre-mpu6050-atmega328p](https://github.com/user-attachments/assets/f277cd9e-e71a-4ea2-91e9-050bcb0050a0)
 
---

### 🔹 Envoi des commandes et réception des données

La communication suit ce processus :  
- Le maître (ATmega328P) commence par envoyer une commande au MPU6050, qui consiste à spécifier l’adresse du registre interne dont il souhaite lire la valeur. Par exemple, pour lire l’accélération sur l’axe X, il envoie l’adresse du registre ACCEL_XOUT_H.  
- Cette commande est envoyée via le bus I2C sous forme d’une trame contenant l’adresse de l’esclave, suivie de l’adresse du registre ciblé.  
- Une fois la commande reçue, le MPU6050 prépare la donnée correspondante et la transmet dès que le maître la demande.  
- Le maître récupère alors la ou les valeurs envoyées par le capteur, généralement sur plusieurs octets, qu’il traite ensuite pour en extraire l’information de mouvement (accélération, rotation, température).
  
[voir le datasheet du MPU6050](https://www.alldatasheet.com/datasheet-pdf/download/1132807/TDK/MPU-6050.html)    

 # Réalisation du PCB

## Création du schéma électronique
- Conception du schéma intégrant le microcontrôleur ATmega328P, le capteur MPU-6050 et les connecteurs.
[voir le schéma électronique](Images.md#Création-du-schéma-électronique)

## Affectation des empreintes (footprints)
- Attribution des empreintes physiques correspondant aux composants.
[voir Affectation des empreintes](Images.md#Affectation-des-empreintes)

## Création de la carte PCB (implantation)
- Placement des empreintes des composants sur le circuit imprimé.

  ![Création de la carte PCB](https://github.com/user-attachments/assets/ea794759-0ee5-4ad1-9845-91bff85d9f4c)

## Réorganisation des composants
- Ajustement de la disposition pour optimiser l’espace et faciliter le routage.

![Réorganisation des composants](https://github.com/user-attachments/assets/159148ee-3c07-492a-8ed8-c2e2e21f3099)

## Routage des pistes
- Traçage des pistes électriques reliant les composants.
  
![ Routage des pistes](https://github.com/user-attachments/assets/14889db7-c03d-41f7-9225-8cdfcd50c843)

## Définition des contours du PCB (Edge Cuts)
- Délimitation de la forme finale et des dimensions de la carte.
  
![ Définition des contours du PCB](https://github.com/user-attachments/assets/3df37061-8004-4fdc-8a90-1eaaaba42349)

## Visualisation 3D du PCB
- Contrôle visuel du circuit imprimé en trois dimensions avant fabrication.
  
![ Visualisation 3D du PCB](https://github.com/user-attachments/assets/29374317-7dc3-43e4-95ee-a7a15d3b6c9a)


