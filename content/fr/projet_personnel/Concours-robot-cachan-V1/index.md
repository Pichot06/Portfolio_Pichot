---
title: "Concours de Robotique Cachan première année"
date: 2023-06-08
url: "/fr/projects/Concours-robot-cachan-V1/"
category: projects
summary:
description:
cover:
  image: 3.jpg
  alt:
  caption:
  relative: true
showtoc: true
draft: false
layout: single
tocopen: true
hidemeta: false
tags: ["projet", "GEII"]
keywords: ["projet", "GEII"]
---

# Présentation du concours

Dans le cadre de ma première année de BUT GEII, j’ai eu l’opportunité de participer à la Rencontre Robotique des IUT GEII, un concours national organisé à Cachan. L’objectif du projet était de concevoir un robot autonome capable de suivre une ligne noire sur fond blanc, tout en accomplissant divers challenges sur un parcours imposé.

Le robot devait être capable de s’adapter à des courbes, éviter des obstacles, reconnaître des marques spécifiques au sol et atteindre des zones d’arrivée précises. Le concours s’est déroulé sur deux jours, avec 13 épreuves dévoilées le vendredi matin, 13 l’après-midi et 10 supplémentaires le samedi, ce qui exigeait une grande réactivité et une forte capacité d’adaptation.

{{< figure src="9.jpg" align="center" width="600px">}}
_Terrain_

# Avant le départ 

Le projet portait sur la conception d’un robot suiveur de ligne. Nous avions déjà travaillé sur ce type de robot à travers deux projets SAÉ réalisés au semestre 1 et 2, ce qui nous a permis de réutiliser certaines bases. Pour le concours, la structure du robot (châssis, roues, carte de commande et carte batterie) nous a été fournie.

Nous avons ensuite conçu plusieurs cartes électroniques spécifiques, notamment une carte capteurs, une carte mezzanine et une carte hacheur. L’intégralité du code de suivi de ligne a été développé par notre équipe, en utilisant un algorithme PID pour ajuster la trajectoire du robot avec précision. Bien que nous ayons eu peu de notions sur le PID au départ, nos recherches et nos essais nous ont permis d’obtenir rapidement un fonctionnement fluide et efficace.

Le code réalisé formait une base stable, sur laquelle nous avons pu nous appuyer pour adapter le comportement du robot en fonction des différents challenges rencontrés lors du concours.

```cpp
#include "mbed.h"
#include <cstdio>
#include <string.h>


/////////////////////////////////////


#define periode 100
#define VG 0.7
#define VD 0.68
#define VMOY 65 // 60 //62 //65 //68 //70 //75
#define KP 72   // 60
#define KD 2.5  // 0.7 //1.5 //2 //2.5 //3

//_______________________________________________
//-----LED RGB MBED
BusOut leds(LED1, LED2, LED3);
//-----FIN DE COURSE ET JACK
DigitalIn FDC(PTE21), Jack(PTE20);
//-----TELEMETRE
//SRF05 Droite(A5, D4);    // trig echo Telemetre droite
//SRF05 Devant(PTE30, D5); // trig echo Telemetre devant

//-----BOUTONS
//DigitalIn bp1(D12);
//DigitalIn bp2(D13);
//-----LCD
// Grove_LCD_RGB_Backlight rgbLCD(D14, D15); // SCL SDA LCD
//-----CAPTEURS
AnalogIn C1X(A1), C2X(A2), C3X(A3), C4X(A4);
//-----MOTEURS
PwmOut motD(D6);
PwmOut motG(D8);
//-----SENS MOTEURS
DigitalOut sensD(D7);
DigitalOut sensG(D9);
//-----ENCODEURS MOTEURS
// 2 signaux moteurD
InterruptIn MotD_A(D2);
InterruptIn MotD_B(D3);
// 2 signaux moteurG
InterruptIn MotG_A(D11);
InterruptIn MotG_B(D10);
//-----BATTERIE
AnalogIn Bat(A0);

//____________________________________
//----VARIABLES GLOBALES
int etat = -1;
int vg, vd;
int Jackread, FDCread, bp1read, bp2read;
float Vbat, coeffbat, valbat;
float trmin;
float Devantread, Droiteread;
float C1, C2, C3, C4, P, I, D;
float Error = 0.0, Integrale = 0.0, Last_error = 0.0;
float Derivative = 0.0, Correction = 0.0;
//----VARIABLES GLOBALES
int etat = -1;
int vg, vd;
float Vbat, coeffbat, valbat;
float trmin;
int compteur = 0;
int compteur2 = 0;
float consigne = 60;
int Jackread, FDCread;
float C1, C2, C3, C4, P, I, D;
float Error = 0.0, Integrale = 0.0, Last_error = 0.0;
float Derivative = 0.0, Correction = 0.0;
char tab[10];
//----FONCTIONS
void init();
void vitesse(int, int);
void PID(void);
void PIDLent(void);

//-----TIMER/TICKER
Timer rac;
Timer timer1;
Timer Time1;
Timer TourneG;
int main() {
  init();
  while (1) {
    // rgbLCD.clear();
    // rgbLCD.locate(0, 0);
    // sprintf(tab, "etat = %d ", etat);
    // rgbLCD.print(tab);
    C1 = C1X.read();
    C2 = C2X.read();
    C3 = C3X.read();
    C4 = C4X.read();
    Jackread = Jack.read();
    FDCread = FDC.read();
    //   printf("%f et %f\n",C1,C2);
    switch (etat) { // partie transitions
    case -1:
      if (Jackread == 1)
        etat = 0;
      break;
    case 0:
      if (FDCread == 0)
        etat = 1000;
      if (C1 > 0.7 && C2 > 0.7)
        etat = 500;
      break;
    case 500:
      if (C1 > 0.7 && C2 > 0.7&& rac.read()>0.3)
      rac.stop();
      rac.reset();
        etat = 510;
      break;
    case 510:
    if (C3 > 0.7&& rac.read()>0.3)
      etat = 0; // 520
      break;
    case 520:
      if (C1 < 0.7 && C2 < 0.7)
        etat = 530;
      break;
    case 530:
      etat = 0;
      break;
    case 1000:
      if (Jackread == 0)
        etat = -1;
      break;
    }
    switch (etat) { // partie état
    case -1:
      vitesse(0, 0);
      break;
    case 0:
      PID();
      break;
    case 500:
    rac.start();
      PIDLent();
      break;
    case 510:
    tourneG
      vitesse(-40,40);
      break;
    case 520:
      PID();
      break;
    case 530:
      vitesse(30,-40);
      wait(1.5);
    case 1000:
      vitesse(0, 0);
      break;
    }
    // printf("etat : %d\n", etat);
  }
}
void init() {
  printf("commence !\n\r");
  rgbLCD.setRGB(255, 139, 0); // set the color
  motD.period_us(periode);
  motG.period_us(periode);
  vitesse(0, 0);
  timer1.start();
}

void vitesse(int G, int D) {
  valbat = Bat.read();
  Vbat = (valbat * 3.3 * ((1200.0 + 4700.0) / 1200.0) + 0.77); // calcul
  coeffbat = (12 / Vbat);
  if (D > 100)
    D = 100;
  if (D < -100)
    D = -100;

  if (D > 0) {
    sensD.write(1); // marche
    motD.pulsewidth_us((100 - D) * coeffbat);
  } else {
    sensD.write(0);
    motD.pulsewidth_us((abs(D)) * coeffbat);
  }
  if (G > 0) {
    sensG.write(0);
    motG.pulsewidth_us(G * coeffbat);
  } else {
    sensG.write(1);
    motG.pulsewidth_us((100 + G) * coeffbat);
  }
}

void PID(void) {
  Time1.start();
  C1 = C1X.read();
  C2 = C2X.read();
  C3 = C3X.read();
  C4 = C4X.read();
  // Proportionnel
  Error = C3 - C2; // soustration de c3 c2 pour voir l'erreur
  P = Error * KP; // erreur  (moteur Gauche 70 droite 68) 70 + Error*0.3 et 68 -
  // Intégrale
  Integrale = Integrale + Error;
  I = Integrale * 0.1;
  // Dérivatif
  Time1.stop();
  Derivative = (Error - Last_error) / Time1.read();
  Last_error = Error;
  D = Derivative * KD;
  Time1.reset();
  // Correction
  Correction = (P + D);
  // printf("%f\n\r",Error);
  // printf("P=%.2f   I=%.2f   D=%.2f\n\r",P,I,D);
  // printf("ET %.2f\n\r",Correction);
  vg = VMOY + (int)Correction;
  vd = VMOY - (int)Correction;
  // printf("vitesse %d,%d\n\r",vd,vg);
  vitesse(vg, vd);
}

void PIDLent() {
  Time1.start();
  C1 = C1X.read();
  C2 = C2X.read();
  C3 = C3X.read();
  C4 = C4X.read();
  // Proportionnel
  Error = C3 - C2; // soustration de c3 c2 pour voir l'erreur
  P = Error * KP; // erreur  (moteur Gauche 70 droite 68) 70 + Error*0.3 et 68 -
  // Intégrale
  Integrale = Integrale + Error;
  I = Integrale * 0.1;
  // Dérivatif
  Time1.stop();
  Derivative = (Error - Last_error) / Time1.read();
  Last_error = Error;
  D = Derivative * KD;
  Time1.reset();
  // Correction
  Correction = (P + D);
  // printf("%f\n\r",Error);
  // printf("P=%.2f   I=%.2f   D=%.2f\n\r",P,I,D);
  // printf("ET %.2f\n\r",Correction);
  vg = VMOYLENT + (int)Correction;
  vd = VMOYLENT - (int)Correction;
  // printf("vitesse %d,%d\n\r",vd,vg);
  vitesse(vg, vd);
}

```

# Vendredi et Samedi

Nous sommes arrivés à Paris le jeudi, la veille du début des épreuves. L'hébergement se faisait dans un gymnase, sur des lits de camp.

{{< figure src="2.jpg" align="center" width="600px">}}
_Arrivé au concours_

Le vendredi matin, dès 9h, nous avons commencé par l’homologation des robots. Notre équipe de Nice avait apporté quatre robots, tous homologués avec succès. Une fois cette étape validée, nous avons pu accéder aux premières épreuves.Les 13 premiers challenges nous ont été remis, avec un niveau de difficulté assez simple. 

{{< pdf_embed file="Challenge du vendredi matin.pdf" >}}

Nous avions ensuite trois heures pour en réussir un maximum. Chaque validation se faisait en conditions officielles : il fallait se présenter sur l'une des quatre pistes, annoncer le challenge à réaliser, puis le réussir sous le regard attentif des arbitres.

L'après-midi, de nouveaux challenges nous ont été proposés, et l’objectif restait le même : en réussir le plus possible avant le samedi matin. À partir de 19h, les épreuves étaient suspendues, les arbitres ayant terminé leur journée. Nous avons alors tous été dîner, puis nous sommes revenus travailler sur les nouveaux challenges du lendemain.Avec Thomas Gomes, nous avons passé une nuit blanche à préparer huit nouveaux challenges. Tous ont été validés dès l’arrivée du jury le samedi matin. 

{{< figure src="6.jpg" align="center" width="600px">}}
_4h48 du matin avec thomas gomes_

Ensuite, une nouvelle série de 10 challenges, plus complexes, a été lancée, ainsi que deux épreuves de rapidité. Mon robot a terminé premier sur la première course, et deuxième sur la seconde.

Au final, l’équipe de Nice a réussi à valider tous les challenges du vendredi matin. En additionnant les points de toutes nos équipes, nous avons décroché la deuxième place du classement général.

# Conclusion : un rapport humain

Au-delà des aspects techniques, la Coupe de Robotique de Cachan a été une véritable aventure humaine.
Cette expérience nous a permis de développer notre esprit d’équipe dans un environnement aussi stimulant qu’exigeant. Nous avons appris à communiquer de manière claire, à faire face aux imprévus, et à avancer ensemble dans un esprit d’entraide. Elle a renforcé notre cohésion de groupe et favorisé la création de liens solides avec les étudiants de première année. L’ambiance bienveillante, ainsi que le partage des compétences entre promotions, ont joué un rôle essentiel dans la réussite du projet, illustrant à quel point les relations humaines sont au cœur du travail collectif.


