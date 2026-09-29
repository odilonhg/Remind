# 📸 Remind — Application de Souvenirs Locaux & Partage P2P

**Remind** est une application Android moderne, 100 % locale et respectueuse de la vie privée, conçue pour capturer des moments du quotidien (format double photo / Dual-Camera) et les échanger automatiquement en réseau local (P2P Wi-Fi) avec les personnes situées à proximité.

---

## ✨ Fonctionnalités Principales

- **📸 Capture Réaction & Dual-Camera (CameraX)** :
  - Prise de vue simultanée ou séquentielle de la caméra arrière et de la caméra frontale.
  - Sauvegarde haute résolution locale avec position géographique EXIF.

- **📡 Partage Peer-to-Peer (P2P) 100% Local (Ktor + NSD)** :
  - Découverte automatique des appareils à proximité sur le même réseau Wi-Fi grâce à **NSD (Network Service Discovery / mDNS)**.
  - Échange direct de photos entre appareils via un serveur HTTP embarqué **Ktor (CIO)** sans passer par un serveur cloud ou Internet.

- **🗺️ Carte Interactive des Souvenirs (osmdroid)** :
  - Visualisation des Reminds capturés et reçus directement sur une carte OpenStreetMap interactive.
  - Marqueurs géolocalisés avec prévisualisation des photos.

- **🖼️ Galerie & Visualiseur de Photos** :
  - Grille personnalisable (1, 2, 3 ou 4 colonnes).
  - Mode plein écran avec affichage de la caméra secondaire en incrustation.
  - Options de suppression et de partage.

- **📦 Sauvegarde & Restauration ZIP** :
  - Exportation complète de la galerie et des métadonnées au format d'archive ZIP.
  - Importation avec dédoublonnage automatique.

- **🚀 Mises à jour In-App Automatiques (GitHub Releases)** :
  - Vérification automatique et téléchargement in-app de la dernière version publiée sur GitHub Releases.
  - Installation directe via `FileProvider` et `PackageInstaller`.

- **📲 Widgets Écran d'Accueil (Jetpack Glance)** :
  - **Remind Reçu** : Affiche la dernière photo reçue de vos proches à proximité.
  - **Remind Souvenir** : Affiche un souvenir aléatoire de votre galerie.

- **🔒 Confidentialité Totale** :
  - Aucune inscription, aucun compte, aucun serveur distant, aucun traqueur.
  - Vos photos et données restent exclusivement sur vos appareils.

---

## 🛠️ Stack Technique & Architecture

- **Langage** : Kotlin 2.2
- **UI Framework** : [Jetpack Compose](https://developer.android.com/jetpack/compose) (Material 3, Edge-to-Edge)
- **Gestion d'État & Concurrence** : Kotlin Coroutines, StateFlow, LiveData
- **Caméra** : [CameraX](https://developer.android.com/training/camerax) (core, camera2, view)
- **Base de Données** : [Room Database](https://developer.android.com/training/data-storage/room) avec KSP
- **Stockage des Préférences** : [DataStore Preferences](https://developer.android.com/topic/libraries/architecture/datastore)
- **Réseau Local & P2P** :
  - **NSD (mDNS)** pour la découverte de services
  - **Ktor Server & Client (CIO)** pour les transferts HTTP P2P
  - **Kotlinx Serialization** pour l'échange de JSON
- **Cartographie** : [osmdroid](https://github.com/osmdroid/osmdroid) (OpenStreetMap)
- **Widgets** : [Jetpack Glance](https://developer.android.com/jetpack/compose/glance)
- **Tâches en arrière-plan** : [WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager)
- **Chargement d'Images** : [Coil Compose](https://coil-kt.github.io/coil/)

---

## 📐 Architecture du Projet

```text
com.odilonhg.remind
├── data
│   ├── local
│   │   ├── db           # Database Room (PhotoEntity, PhotoDao, AppDatabase)
│   │   ├── preferences  # UserPreferencesRepository (DataStore)
│   │   └── storage      # Gestionnaire d'images physiques sur le stockage local
│   ├── location         # Geolocation helper (GPS / Coarse location)
│   ├── p2p              # Moteur P2P (NSD Discovery, Ktor Server & Client)
│   └── update           # Manager de mises à jour GitHub (GitHub Releases API)
├── service              # Service d'arrière-plan P2P (Foreground Service)
├── ui
│   ├── navigation       # NavGraph & MainContainer avec animations
│   ├── screens          # Écrans Compose (Home, Galerie, Carte, Profil, Appairage)
│   └── theme            # Material3 Theme & Typography
├── widget               # Receveurs & layouts des Widgets Glance
└── worker               # DailyReminderWorker (Rappels quotidiens)
```

---

## 🚀 Installation & Compilation

### Prérequis
- Android Studio Ladybug (2024.2.1) ou plus récent
- JDK 17
- Android SDK 35 (minSdk 26 — Android 8.0+)

### Cloner et Compiler le projet
```bash
# Cloner le dépôt
git clone https://github.com/odilonhg/Remind.git
cd Remind

# Compiler l'application (APK Debug)
./gradlew assembleDebug
```

---

## 🔄 Système de Mise à jour Automatique In-App

Remind intègre son propre système d'auto-update basé sur les **Releases GitHub** :
1. Au lancement (ou manuellement depuis la page **Profil**), l'application interroge l'API GitHub (`/releases/latest`).
2. Si une version plus récente est disponible (comparaison de semver `vX.Y`), l'application télécharge l'APK officiel depuis GitHub.
3. L'installation est déclenchée directement in-app via `FileProvider`.

---

## 🔒 Sécurité & Vie Privée

- **100 % Décentralisé** : Les photos ne transitent sur aucun serveur central.
- **Réseau Local Uniquement** : L'échange P2P se fait exclusivement entre appareils connectés au même réseau Wi-Fi local.
- **Transparence** : Aucune donnée personnelle ou télémétrique n'est collectée.

---

## 📜 Licence

Ce projet est sous licence MIT. Libre d'utilisation, de modification et de distribution.

Créé avec ❤️ par **odilonhg**.
