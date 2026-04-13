# 2526\_Projet1A\_veste\_lumineuse

Veste lumineuse



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
utiliser Visual studio Code pour la partie software sur readme

Cahier des charges : carte PCB 40mm-40mm


Guide des Composants : interrupteur, kappa de découplage, résistance, diode chenillard, connecteur INPUT/OUTPUT (alim), connecteur SWD


## Fonctionnement du connecteur SWD

Le connecteur SWD permet d'injecter le code compilé (le binaire) dans la mémoire flash de la puce. Et il permet le Débogage en temps réel, càd "mettre en pause (breakpoints)" , "lire et modifier" la valeur des variables ou des registres internes pendant que le code tourne et "avancer pas à pas" dans les lignes de code pour comprendre pourquoi un bug survient.
structure :

VCC (VTarget) : Pour que la sonde de débogage connaisse la tension logique de la carte (souvent 3.3V).

SWDIO : Le flux de données.

SWCLK : Le signal d'horloge pour synchroniser les données.

GND : La masse commune.

RESET (Optionnel) : Pour forcer le redémarrage de la puce.


## Diode chenillard

On place des diodes chenillard sur KiCad afin de vérifier le bon fonctionnement sur la partie firmware 
càd on isole le problème : si les LED finaux ne fonctionnent pas, est-ce parce qu'ils sont mal soudés ou alors est-ce à cause du code; les chenillard ici nous donne la solution.

## schéma Kicad et PIN def de la STM32

![PIN definitions](image/stm32_pin_def.png)

![Éditeur schéma Kicad](image/Kicad_schema.png)

![cappa de découplage](image/cappa_decouplage.png)

## Mise en oeuvre des LED (mise en série + code fonctionnel)

![repésentation LED en série](image/led_serie.png)

![Fonctionnement LED avec la STM32](image/led_stm32.png)

## KiCad version final 
![visuPCB](image/visuPCB.png)

![visu3D](image/visu3D.png)

## Vérification du bon fonctionnement du PCB
On réussit à faire clignoter les LED avec le code suivant : 
![code allumage des LED_test](image/code_LED_test.png)

## Sommaire 

## étude du capteur audio (micro)

Au FAB Lab nous avons réalisé les branchements pour pouvoir relier le microphone au connecteur de notre PCB :
![Branchement du microphone adapté avec le PCB](image/microphone.jpg)


## Au cours des séances dernières, le PCB étant inutilisable car il nous fallait finir de souder les connecteurs "MIC IN" et "LED OUT" nous avons réparti entre nous les tâches suivantes : 
Antoine -> au Fab lab pour souder les connecteurs au PCB

Margaux -> écriture du code des LED chenillardes avec la nucléo donnée par l'établissement car le PCB étant en finition (cependant, cette stm32 diffère avec celui qu'on utilise dans notre PCB, ce qui exige de changer quelques lignes de code) 

Lou-Jane -> écriture du code des LED chenillardes et utilisation de l'oscilloscope pour vérifier le bon fonctionnement de celles-ci.

## détail du code sur STM32_CubeIDE

Dans un premier temps, nous cherchons à faire clignoter la chaîne de LED pour vérifier le bon fonctionnement de nos composants, voici le code utilisé : 
```bash
------à remplir------
```
Dans un deuxième temps, nous cherchons à créer un patern avec le chenillard pour l'esthétisme des LED : avec pour objectif des couleurs qui change et des LED qui s'allument et s'éteignent à la chaîne, voici le code utilisé pour réaliser cela :
```bash
------à remplir------
```
Dans un troisième temps, nous récupérerons les valeurs obtenues par le microphone, d'une part en observant à l'oscilloscope puis en regardant directement sur l'ordinateur :
```bash
------à remplir------
```
Dans un dernier temps, nous allons relier le fonctionnement du microphone avec celui de la chaîne de LED afin de donner de la cohérence entre l'allumage des LED avec les Pics d'intensité sonore captés par le microphone :
```bash
------à remplir------
```
## étude de la batterie (power bank)

------à remplir------

## Conception de la veste muni de LEDs

------à remplir------

## Difficultés rencontrées au cours du projet 

------à remplir------



