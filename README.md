<img width="165" height="150" alt="schema kicad" src="https://github.com/user-attachments/assets/c3db3d41-2836-4fae-8048-15bc634e74b1" /># 💡 Lampe Intelligente à Détection de Mouvement et de Lumière

**Auteur :** Gouma Loubele Rollsy  
**Groupe :** 1221FA  
**Plateforme :** Arduino Uno  
**Langage :** C/C++  
**Université :** Politehnica Bucarest — FILS — AM (Français)

---

## Description

Ce projet implémente une lampe intelligente autonome basée sur un microcontrôleur
**Arduino Uno**. Le système combine deux capteurs environnementaux pour décider
automatiquement d'allumer ou d'éteindre une LED :

- Un **capteur PIR (HC-SR501)** pour détecter la présence humaine via le
  rayonnement infrarouge passif.
- Une **photorésistance (LDR)** pour mesurer le niveau de luminosité ambiante
  via le convertisseur ADC 10 bits de l'Arduino (broche A0).

La lampe ne s'allume que lorsque **les deux conditions sont simultanément vraies** :
un mouvement est détecté **ET** la luminosité ambiante est insuffisante.
Un délai de temporisation (~30 secondes) maintient la lampe allumée après la
disparition du mouvement pour éviter les coupures brusques.

---

## Motivation

L'éclairage artificiel représente une part importante de la consommation électrique
résidentielle. Une grande partie de cette énergie est gaspillée lorsque les lumières
restent allumées dans des pièces vides ou en présence d'une luminosité naturelle
suffisante.

Ce projet propose une solution **économique, simple et autonome** :

- ⚡ **Économie d'énergie** : la lampe reste éteinte si la lumière du jour suffit,
  même en cas de présence détectée.
- 🤖 **Automatisation complète** : aucune intervention manuelle requise.
- 🔧 **Implémentation bas niveau** : protocoles gérés manuellement en C/C++,
  sans bibliothèques de haut niveau, conformément aux exigences du cours.
- 🔌 **Extensibilité** : le système peut être enrichi (module Wi-Fi, relais pour
  vraie ampoule, horloge RTC, interface web).

---

## Architecture

### Schéma fonctionnel

<!-- TODO: Insérer ici le schéma bloc du système (ex: image KiCad ou TinkerCad) -->
<!-- Exemple : ![Schéma fonctionnel](./docs/schema_fonctionnel.png) -->

### Schéma de câblage (Breadboard)

<!-- TODO: Insérer ici le schéma de câblage complet -->
<!-- Exemple : ![Schéma câblage](./docs/schema_cablage.png) -->

### Schéma électrique (KiCad)

<!-- TODO: Insérer ici le schéma électrique KiCad -->
<!-- Exemple : ![Schéma KiCad](./docs/schema_kicad.png) --!<img width="680" height="620" alt="schema_kicad" src="https://github.com/user-attachments/assets/893dfb36-6cf6-4410-8863-9182f1bbf9fa" />




### Logique de décision

const int pinPIR = 2;
const int pinLDR = A0;
const int pinLampe = 13;
const int seuilObscurite = 400;

void setup() {
  pinMode(pinPIR, INPUT);
  pinMode(pinLampe, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // 1. Lecture PIR
  int etatPIR = digitalRead(pinPIR);

  // 2. Lecture LDR
  int valeurLDR = analogRead(pinLDR);

  // 3. Affichage moniteur série
  Serial.print("Mouvement (PIR): ");
  Serial.print(etatPIR);
  Serial.print(" | Luminosite (LDR): ");
  Serial.println(valeurLDR);

  // 4. Logique conditionnelle
  if (etatPIR == HIGH && valeurLDR < seuilObscurite) {
    digitalWrite(pinLampe, HIGH); // Allumer
  } else {
    digitalWrite(pinLampe, LOW);  // Éteindre
  }

  delay(200);
}
