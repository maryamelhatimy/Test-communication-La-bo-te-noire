# Introduction
Dans les systèmes embarqués modernes, la collecte et l’analyse de données en temps réel sont essentielles pour le suivi et la sécurité des équipements, notamment dans des domaines critiques comme l’automobile, l’aviation ou le ferroviaire. S’inspirant du fonctionnement des boîtes noires utilisées dans ces industries, ce projet a pour objectif de concevoir un système embarqué capable d’enregistrer et de transmettre en temps réel des données de mouvement (vitesse et orientation) à l’aide d’un capteur inertiel. Les informations sont ensuite visualisées sur une station de contrôle via un écran LCD.
Ce projet s’inscrit dans le cadre du Tekbot Robotics Challenge et fait appel à plusieurs compétences majeures : programmation directe des microcontrôleurs ATmega328P sans utiliser de carte Arduino, communication via le protocole I2C, conception de circuits imprimés avec KiCAD, réalisation d’un boîtier cubique de 7 cm, intégration matérielle sur veroboard ou PCB, ainsi que la conception d’une alimentation spécifique.
# Sommaire

- [cahier des charges ](#Cahier-des-charges)
- [Schéma synoptique](#Schéma-synoptique)
  
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

[voir le schema synoptique](Images.md#-image--schema-synoptique)

