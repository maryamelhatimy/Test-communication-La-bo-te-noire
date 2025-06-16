# Introduction

Dans le cadre du Tekbot Robotics Challenge, ce projet consiste à concevoir une boîte noire inspirée de celles utilisées dans l’aéronautique.  
Cette boîte embarquée doit être capable de :
- Collecter des données de vitesse et de position via un capteur MPU6050 (gyroscope + accéléromètre),
- Transmettre ces données en temps réel à une station de contrôle via le bus I2C,
- Visualiser les données sur un écran LCD (connecté à la station).

Le projet vise à :
- Mettre en œuvre un microcontrôleur ATmega328P de manière autonome,
- Réaliser une carte électronique sur KiCad (avec alimentation externe),
- Créer une interface de visualisation en temps réel.
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
- ATmega328P x2
- MPU6050
- LCD 16x2 (mode 4 bits)
- Alimentation
  
## Schéma synoptique
  
