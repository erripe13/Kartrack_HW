# Kartrak – Hardware

> **Dépôts associés** :  
> - [Kartrack_SW](https://github.com/erripe13/Kartrack_SW) – Code embarqué STM32 (FreeRTOS, GPS, LoRa, SD...)  
> - [Kartrack_HW](https://github.com/erripe13/Kartrack_HW) – **Ce dépôt** – Schéma et PCB du système  
> - [Kartrack_APP](https://github.com/erripe13/Kartrack_APP) – Application d’analyse et visualisation des données

## 🎯 Présentation du projet Kartrak

Kartrak est un système embarqué de chronométrage et de télémétrie pour karting, conçu pour être fixé sur le volant. Il enregistre en temps réel les données de course (position GPS, accélérations, orientation) et les transmet en LoRa à l'appli desktop et les enregistre sur carte microSD. L’objectif est de proposer un outil complet pour les pilotes amateurs ou en compétition : analyse post-course, visualisation en temps réel par l'équipe, appel aux stands, comparaisons de trajectoires.

---

Le projet est basé sur la plateforme **STM32F746G-DISCO**, exploitant son STM32 puissant et ses périphériques avancés (écran TFT, SDMMC, i2c, SPI, UART) pour accélérer le développement hardware, éliminant l'étape d'implémentation écran/uC.

Ce dépôt contient les fichiers **KiCad** du schéma électronique et du circuit imprimé (PCB) conçu pour le système embarqué Kartrack. Il s’agit d’une carte d’interface et d’alimentation destinée à supporter la carte **STM32F746G-DISCO** tout en intégrant les composants spécifiques au projet.

Le système est alimenté à partir de **deux batteries 18650** montées en **parallèle**. Un **convertisseur boost (step-up)** élève la tension à 5 V pour alimenter la carte STM32F7 Discovery via ses broches VIN (arduino) et GND. Ces 5V sont réutilisés et abaissés à 3.3V avec un LDO. Un autre LDO permet la recharge des batteries.

Le PCB a été conçu dans une logique de prototypage rapide :
- **Réduction du temps de conception** en réutilisant une carte STM32 existante (la DISCO) plutôt qu’un microcontrôleur nu.
- Embarque les modules suivants :
  - LoRa : RA-01 (SPI). Le Lora nous permet de couvrir un grand espace de communication (Piste de Karting) sans passer par le réseau GSM et a faible conso.
  - GPS : Quectel L76 (UART). La grande contrainte du GPS est sa fréquence élevée d'aquisition, ici 5Hz. La norme des GPS est de 1Hz, mais pour un kart à 60km sur une piste de 1000m la norme n'est pas suffisante.
  - Gyroscope : ICM-42605 (I²C). Le gyroscope nous permet d'ajouter une donnée en plus à notre pilote. Ici le gyro est réglé sur la mesure la plus rapide, l'accélaration la plus élevée (16G). En cas d'accident cette donnée peut être des plus intéréssante.  
     
---

## À venir

- Intégration directe d’un STM32 nu sur une version 1 de ce projet, considérant celle-ci comme une bêta ou un POC.
- Création d'un boîtier en conséquence plus fin et plus petit, potentiellement étanche.
- Amélioration de la partie RF pour une meilleure fiabilité du GPS et une plus grande portée du LoRa.
