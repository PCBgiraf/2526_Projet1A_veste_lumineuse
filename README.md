# 2526\_Projet1A\_veste\_lumineuse


## Memo GitHub

```bash
git clone <url ssh du code> # pour créer le sous-dossier sur le PC 

cd 2526_Projet1A_veste_lumineuse  # pour entrer dans le dossier et pouvoir faire des git pull et git status

ls # pour dire dans quel dossier on se trouve

cat README.md # pour voir ce que Git lit actuellement

explorer .  # savoir quelle README.md le git lit si on crée 2 readme.md (probablement causé par un git clone)


git pull # récupérer les dernières modifications

# On travaille

git status # vérifier les modifications

git add . # ajouter les fichiers au prochain commit

git commit -m "Message"

git push


```
Conseil : utiliser Visual studio Code pour la partie software sur readme

## Sommaire
I  ) Introduction

II ) Hardware

     a) Sélection des composants

     b) Logiciel KiCad
 
    c) Soudure du PCB

III) Software

     a) Récupération de valeurs par le microphone

     b) Allumage des LED NeoPixel

     c) Dégradé de couleurs (optionnel)

IV ) Démonstration du produit final (lien vidéo)

V  ) Difficultés rencontrés dans le projet

## Introduction 

Dans le cadre d'un projet d'électronique de 1ère année, il nous est demandé de concevoir une veste lumineuse dont les lumières réagissent aux bruits environnants. Par manque de temps, nous avons interprété et restreint la consigne par : créer une bande de LED qui allume de plus en plus de LED simultanément à mesure que le bruit ambiant s'intensifie.


## Hardware : a) Sélection des Composants

liste des composants principaux qui composent le PCB (en excluant résistances et condensateurs)

On travaille sur une carte PCB 40mm-40mm (taille suffisante pour placer tous nos composants) avec 4 couches : 5V,3.3V,3.3VA,GND .


Guide des Composants : interrupteur, kappa de découplage, résistance, diode chenillard, connecteur INPUT/OUTPUT (alim), connecteur SWD

On place des diodes chenillard implémenté sur le PCB afin de vérifier le bon fonctionnement sur la partie firmware 
càd on isole le problème : si les LED finaux ne fonctionnent pas, est-ce parce qu'ils sont mal soudés ou alors est-ce à cause du code; les chenillard ici nous donne la solution.

------ Fonctionnement du connecteur SWD ------

Le connecteur SWD permet d'injecter le code compilé (le binaire) dans la mémoire flash de la puce. Et il permet le Débogage en temps réel, càd "mettre en pause (breakpoints)" , "lire et modifier" la valeur des variables ou des registres internes pendant que le code tourne et "avancer pas à pas" dans les lignes de code pour comprendre pourquoi un bug survient.
structure :

VCC (VTarget) : Pour que la sonde de débogage connaisse la tension logique de la carte (ici 3.3V).

SWDIO : Le flux de données.

SWCLK : Le signal d'horloge pour synchroniser les données.

GND : La masse commune.

RESET (Optionnel) : Pour forcer le redémarrage de la puce.

----------------------------------------------

## Hardware : b) Logiciel KiCad

schéma Kicad et PIN def de la STM32

![PIN definitions](image/stm32_pin_def.png)

![Éditeur schéma Kicad](image/Kicad_schema.png)

![cappa de découplage](image/cappa_decouplage.png)

## KiCad version final 
![visuPCB](image/visuPCB.png)

![visu3D](image/visu3D.png)


## Hardware : c) Soudure du PCB

/// mettre photo zoomé sur les LED chenillard du PCB pour informer qu'il y a un sens (oui car toutes LED sont des diodes) donné au dos avec un T qui l'indique. je la pose ce soir :!!!!!///

/// photo des PCB à vide + du Stencil : et dire que la méthode avec la pâte à braser est bcp plus rapide et plus précise qu'à la main ///

Au Fab Lab nous avons réalisé la soudure du microphone avec un connecteur tripolaire pour pouvoir le relier au connecteur de notre PCB.
Mise en garde : Bien mettre la gaine (ici en jaune) pour éviter tout court-circuit et conserver une bonne robustesse de l'ensemble déjà soudé.

![Soudure du microphone adapté avec le PCB](image/microphone.jpg)


## Software : a) Récupération de valeurs par le microphone

/// Photo TIMER6 et parler de l'avantage d'utiliser ça et non les fonctions HAL que propose C ///
// Bash : Code de la fonction qui donne la valeur max : dire que les valeurs vont de 0 à 4096 avec pour valeur 2000 avec le bruit ambiant, comme il est plus naturel de calculer en prenant comme repère 0 traduisant le silence -> 40 dB bruit ambiant , nous avons alors translater les valeurs pour avoir entre 0 et 2000 au lieu de 2000 et 4096. Ceci ce traduit par un return de la fonction par max - min (c'est-à-dire la fonction relève l'amplitude). //
// Code d'une liste qu'on prend où on écoute : et avec TimeCallElapse qui est la période des interruptions et qui restitue une valeur prise par le microphone //

## Software : b) Allumage des LED NeoPixel

Durant les premières séances, nous avons utilisé notre carte STM32 pour réaliser les premiers essais, car le PCB était encore en cours de fabrication. Nous avons d’abord tenté de souder les LED une par une, mais le montage ne fonctionnait pas correctement. Ce dysfonctionnement pouvait venir d’une détérioration des LED pendant la soudure, à cause d’un échauffement trop important, ou bien d’un défaut des LED ou des connexions. Nous avons donc décidé d’utiliser un ruban de LED RGB adressables, plus fiable et plus simple à mettre en œuvre.

![représentation LED en série](image/led_serie.png)
 
Cependant, les LED adressables ne se commandent pas comme des LED classiques. Elles suivent un principe particulier :

![Principe](image/RGB.jpeg)

### Rôle du timer et du signal PWM
Pour commander les LED RGB adressables, la STM32 envoie une suite de bits sur le fil DATA. Les LED différencient un bit 0 d’un bit 1 grâce à la durée de l’impulsion à l’état haut.

Un signal PWM alterne entre un état bas et un état haut. En modifiant le temps passé à l’état haut, on peut créer deux types d’impulsions :
- une impulsion courte pour envoyer un bit 0 ;
- une impulsion plus longue pour envoyer un bit 1.

Le timer permet donc d'envoyer un signal PWM stable pour que les LED comprennent correctement les données.

## STM32G4

Une fois le PCB prêt, nous avons dû adapter notre programme à une autre carte : la STM32G4. Le code précédent n’était pas directement compatible, car les fichiers de configuration, les bibliothèques HAL et certains périphériques internes peuvent changer d’une famille de STM32 à une autre.

Nous avons donc importé les dossiers et fichiers nécessaires correspondant à cette nouvelle STM32G4. Cela comprenait notamment les fichiers liés au microcontrôleur, à la configuration des périphériques, aux timers, aux GPIO et aux bibliothèques HAL.

![Configuration](image/configuration.png)

Une fois que la bande de LED adressables s'allument sur notre nouvelle carte STM32G4, nous avons pu passer à l’assemblage des deux parties principales du programme :

le code de commande des LED ;
le code d’acquisition du microphone.

L’objectif était de faire fonctionner les deux programmes ensemble. Le microphone permet de mesurer l’intensité sonore ambiante, puis cette valeur est utilisée pour modifier l’affichage des LED. Par exemple, lorsque le son détecté est faible, peu de LED s’allument. Lorsque le son devient plus fort, un plus grand nombre de LED peut s’allumer ou la couleur peut changer.

![amplitude](image/amplitude.png)
![amplitudeprincipe](image/amplitude.JPG)

## Software : c) Dégradé de couleurs (optionnel)


Nous cherchons à créer un pattern avec le chenillard pour l'esthétisme des LED : avec pour objectif des LED qui changent de couleurs avec ces dernières qui déplacent les couleurs le long de la chaîne, voici le code utilisé pour réaliser cela :

![couleur](image/couleur.png)

Cette partie du code sert à afficher un dégradé de couleurs sur le ruban de LED. Le programme parcourt toutes les LED une par une. Si la LED fait partie du nombre de LED à allumer, une couleur est calculée avec la fonction `hsl_to_rgb()`. La couleur dépend de la position de la LED dans le ruban, ce qui permet d’obtenir un dégradé.

Si la LED ne doit pas être allumée, elle est mise en noir avec `led_set_RGB(i, 0, 0, 0)`, ce qui revient à l’éteindre.

Une fois que toutes les couleurs sont définies, la fonction `led_render()` envoie les données au ruban. La variable `angle` est ensuite augmentée pour faire évoluer les couleurs au cours du temps, ce qui crée un effet animé. 



## Démonstration du produit final (lien vidéo)

Cliquer sur l'un des hyperliens obtenir un aperçu du projet final.
www.linkedin.com/in/antoine-loysel
www.youtube_vidéo_veste_lumineuse
www.linkedin_margaux_maurent
www.linkedin_LouJane_Hartmann


## Difficultés rencontrées au cours du projet 

// parler des LED à souder une par une : elle chauffe trop, c'est pas normal 
// Screen des valeurs du microphone avec les fonctions HAL qui était autour de 700 : ce qui est absurde. Mais désormais avec l'usage de Timer tout est bon //
// N'avoir qu'un seul PCB rend difficile la répartition des tâches car on doit attendre que l'un ait fini avec le PCB avant de faire ses mesures ce qui retarde la chaîne de travail : une des solutions aurait été de faire plusieurs PCB (solution réalisable car le PCB et les composants était rapide à souder). // 


## Conclusion