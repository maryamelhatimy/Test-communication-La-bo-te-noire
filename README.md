# Introduction
Dans les systèmes embarqués modernes, la collecte et l’analyse de données en temps réel jouent un rôle essentiel pour la sécurité et le suivi des équipements. Inspiré du principe des boîtes noires utilisées dans l’aviation, ce projet vise à développer un système embarqué capable d’enregistrer et de transmettre les données de mouvement (vitesse, orientation) à l’aide d’un capteur inertiel, avec visualisation en temps réel sur une station de contrôle.  
Ce projet met en œuvre des compétences en systèmes embarqués, transmission de données via I2C, conception de circuits imprimés, ainsi qu’en intégration matérielle complète.

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
  
# Schéma synoptique
  
