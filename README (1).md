# 🎮 Tower Jumper — Clone Icy Tower (Unity)

Un **endless vertical climber 2D** inspiré d'**Icy Tower** avec un style visuel **Block Blast**.
Jeu mobile portrait (9:16) pour **Android** et **iOS**.

![Unity](https://img.shields.io/badge/Unity-2022.3_LTS+-black?logo=unity)
![C#](https://img.shields.io/badge/C%23-Scripts-239120?logo=csharp)
![Platform](https://img.shields.io/badge/Platform-Android_%7C_iOS-blue)

---

## 📁 Structure du projet

```
TowerJumper/
├── Assets/
│   ├── Scripts/
│   │   ├── Core/
│   │   │   ├── GameConfig.cs          ← Toutes les constantes du jeu (ScriptableObject)
│   │   │   ├── GameManager.cs         ← Singleton : score, combo, rush, état du jeu
│   │   │   ├── SoundManager.cs        ← Sons synthétisés (pas de fichiers audio)
│   │   │   └── GameSceneSetup.cs      ← ⭐ Outil auto-setup de la scène (Editor)
│   │   ├── Player/
│   │   │   ├── PlayerController.cs    ← Physique manuelle, saut speed-based, wall bounce
│   │   │   ├── PlayerAnimator.cs      ← Apparence du personnage, glow rush
│   │   │   └── CameraFollow.cs        ← Caméra qui suit vers le haut + rising floor
│   │   ├── World/
│   │   │   ├── WorldGenerator.cs      ← Génération procédurale seeded + difficulté
│   │   │   ├── PoolManager.cs         ← Object pooling (plateformes recyclées)
│   │   │   ├── BiomeManager.cs        ← 5 biomes avec transitions par hauteur
│   │   │   ├── WallRenderer.cs        ← Murs en briques (style Block Blast)
│   │   │   └── RisingFloor.cs         ← Zone de danger rouge qui monte
│   │   ├── Platforms/
│   │   │   ├── PlatformBase.cs        ← Classe de base (normale + milestone)
│   │   │   ├── FragilePlatform.cs     ← Se brise après 0.3s
│   │   │   └── MobilePlatform.cs      ← Oscille horizontalement
│   │   ├── Ghost/
│   │   │   └── GhostRecorder.cs       ← Enregistre et rejoue la trajectoire
│   │   ├── UI/
│   │   │   ├── HUDController.cs       ← Score, combo, rush bar, floors, danger
│   │   │   ├── GameOverUI.cs          ← Écran game over + leaderboard
│   │   │   ├── MenuController.cs      ← Menu principal (mode, seed, perso)
│   │   │   └── MobileControls.cs      ← Joystick virtuel + bouton saut
│   │   └── Data/
│   │       └── LeaderboardManager.cs  ← Top 10 scores en JSON/PlayerPrefs
│   ├── Scenes/                        ← À créer dans Unity (MenuScene, GameScene)
│   ├── Prefabs/                       ← Générés automatiquement par le PoolManager
│   └── Resources/
├── ProjectSettings/
│   ├── ProjectSettings.asset          ← Config portrait, Android API 24+, iOS 14+
│   └── Physics2DSettings.asset        ← Gravité Y = 0 (gravité manuelle dans le code)
├── .gitignore
├── push_to_github.sh                  ← Script pour pusher vers GitHub
└── README.md                          ← Ce fichier
```

---

## 🚀 Guide de démarrage rapide

### Prérequis

| Outil | Version minimum | Lien |
|-------|----------------|------|
| **Unity Hub** | Dernière version | [unity.com/download](https://unity.com/download) |
| **Unity Editor** | **2022.3 LTS** ou plus récent | Via Unity Hub |
| **Git** | 2.30+ | [git-scm.com](https://git-scm.com/) |
| **Android SDK** (pour Android) | API 24+ | Via Unity Hub > Installs > Add Modules |
| **Xcode** (pour iOS, macOS uniquement) | 14+ | Mac App Store |

### Étape 1 — Cloner le repo

```bash
git clone https://github.com/sraitogaming/sraitogaming.git
cd sraitogaming
```

### Étape 2 — Ouvrir dans Unity

1. Ouvre **Unity Hub**
2. Clique sur **"Open" > "Add project from disk"**
3. Sélectionne le dossier `TowerJumper/` (celui qui contient le dossier `Assets/`)
4. Unity va importer le projet (1-3 minutes la première fois)

> ⚠️ Si Unity demande la version, choisis **2022.3 LTS** ou supérieur. Si tu n'as pas cette version, Unity Hub te proposera de l'installer.

### Étape 3 — Créer le GameConfig asset

1. Dans Unity, va dans `Assets/Scripts/Core/`
2. **Clic droit** dans le panneau Project > **Create > TowerJumper > GameConfig**
3. Nomme-le `GameConfig`
4. Il apparaît dans `Assets/` avec toutes les valeurs par défaut

### Étape 4 — Créer les Scènes

#### Scène Menu (MenuScene)

1. **File > New Scene** (choisir "Basic 2D")
2. **File > Save As** → `Assets/Scenes/MenuScene.unity`
3. C'est la scène du menu principal — tu peux la construire avec le Canvas UI de Unity

#### Scène de Jeu (GameScene) — ⭐ Setup automatique

1. **File > New Scene** (choisir "Basic 2D")
2. **File > Save As** → `Assets/Scenes/GameScene.unity`
3. Crée un **GameObject vide** : **GameObject > Create Empty** → nomme-le `SceneSetup`
4. Ajoute le composant `GameSceneSetup` au SceneSetup
5. **Assigne le GameConfig** que tu as créé à l'étape 3 dans le champ "Config"
6. **Clic droit sur le composant > "Setup Game Scene"**

> 🎉 **Tout se crée automatiquement** : joueur, caméra, murs, plateformes, HUD, contrôles mobiles, son, fantôme... Tous les liens sont câblés !

### Étape 5 — Configurer les scènes de build

1. **File > Build Settings**
2. Clique **"Add Open Scenes"** pour chaque scène :
   - `MenuScene` (index 0)
   - `GameScene` (index 1)
3. Assure-toi que **MenuScene est en premier** (index 0)

### Étape 6 — Tester dans l'éditeur

1. Ouvre `GameScene`
2. Clique **▶ Play**
3. Contrôles clavier :
   - **← → ou A D** : se déplacer
   - **Espace ou ↑ ou W** : sauter
4. En mode Game, tu peux aussi cliquer à gauche (joystick) et à droite (saut)

---

## 📱 Compiler pour mobile

### Android (APK / AAB)

1. **File > Build Settings**
2. Sélectionne **Android** → clique **"Switch Platform"**
3. **Player Settings** (vérifier) :
   - Company Name : `SraitoGaming`
   - Product Name : `Tower Jumper`
   - Package Name : `com.sraitogaming.towerjumper`
   - Minimum API Level : **Android 7.0 (API 24)**
   - Scripting Backend : **IL2CPP**
   - Target Architectures : ✅ **ARM64**
   - Default Orientation : **Portrait**
4. Clique **"Build"** → choisis un dossier → tu obtiens un `.apk`
5. Pour Google Play : clique **"Build App Bundle (Google Play)"** → tu obtiens un `.aab`

**Installer sur ton téléphone :**
```bash
# Via USB + adb
adb install TowerJumper.apk

# Ou transfère le .apk et installe-le directement
```

### iOS (Xcode)

> ⚠️ Nécessite un Mac avec Xcode installé

1. **File > Build Settings**
2. Sélectionne **iOS** → **"Switch Platform"**
3. **Player Settings** :
   - Target minimum iOS Version : **14.0**
   - Bundle Identifier : `com.sraitogaming.towerjumper`
   - Signing Team ID : ton Apple Developer ID
4. Clique **"Build"** → choisis un dossier
5. Ouvre le `.xcodeproj` généré dans **Xcode**
6. Sélectionne ton appareil iOS connecté
7. **Product > Run** (ou Cmd+R)

---

## 🔧 Guide des modifications

### Modifier les paramètres du jeu

Toutes les valeurs du gameplay sont dans le **GameConfig** (ScriptableObject). Tu peux les modifier **sans toucher au code** :

| Ce que tu veux changer | Où dans GameConfig |
|------------------------|-------------------|
| Vitesse du joueur | `normalMaxSpeed` / `hardcoreMaxSpeed` |
| Hauteur du saut | `normalJumpForceBase` + `normalJumpForceSpeedBonus` |
| Glissance au sol | `normalGroundFriction` (plus bas = plus glissant) |
| Taille des plateformes | `normalPlatformMinW` / `normalPlatformMaxW` |
| Écart entre plateformes | `normalGapMin` / `normalGapMax` |
| Vitesse du sol qui monte | `normalFloorBaseSpeed` / `normalFloorAccel` |
| Durée du combo timer | `normalComboTimer` |
| Durée du Rush | `normalRushDuration` |
| Remplissage Rush par combo | `rushFillPerCombo` |
| Changement de biome | `biomeChangeHeight` (en unités de hauteur) |
| Fréquence milestones | `milestoneInterval` (en floors) |

> 💡 **Astuce** : Tu peux modifier ces valeurs **pendant que le jeu tourne** dans l'éditeur pour tester en temps réel !

### Ajouter un nouveau biome

1. Ouvre `Assets/Scripts/World/BiomeManager.cs`
2. Dans la méthode `InitializeDefaultBiomes()`, ajoute un nouveau `BiomeData` au tableau :

```csharp
new BiomeData {
    name = "MON BIOME",
    bgTop = HexColor("#112233"),
    bgBottom = HexColor("#001122"),
    platformColor = HexColor("#ff00ff"),
    brickColor = HexColor("#330033"),
    accentColor = HexColor("#00ffff")
}
```

### Ajouter un nouveau personnage

1. Ouvre `Assets/Scripts/Core/GameSceneSetup.cs`
2. Dans `SetupScene()`, ajoute un 5ème élément au tableau `pa.characters` :

```csharp
new PlayerAnimator.CharacterData {
    name = "MonPerso",
    bodyColor = Color.cyan,
    headColor = Color.white,
    eyeColor = Color.red,
    accessoryColor = Color.magenta
}
```

3. Mets à jour `MenuController.cs` pour supporter le 5ème bouton

### Modifier un type de plateforme

Les 3 types sont dans `Assets/Scripts/Platforms/` :

- **PlatformBase.cs** → plateformes normales
- **FragilePlatform.cs** → modifie `breakDelay` pour changer le temps avant rupture
- **MobilePlatform.cs** → modifie `amplitude`, `speed` pour changer l'oscillation

### Ajouter un nouveau type de plateforme

1. Crée un nouveau script qui hérite de `PlatformBase` :

```csharp
public class BouncyPlatform : PlatformBase
{
    public float bounceMultiplier = 1.5f;

    public override void OnPlayerLand()
    {
        // Le joueur rebondit plus haut
        // (il faudra modifier PlayerController pour gérer ce cas)
    }
}
```

2. Ajoute le type dans `PoolManager.cs` et `WorldGenerator.cs`

### Modifier les sons

Tous les sons sont synthétisés dans `SoundManager.cs`. Pour modifier un son, change les paramètres de fréquence et durée dans la méthode correspondante. Par exemple pour le saut :

```csharp
// Saut plus aigu et plus long
public void PlayJump()
{
    var clip = GenerateClip("jump", 0.2f, (t, dur) =>  // 0.2s au lieu de 0.12s
    {
        float freq = Mathf.Lerp(500, 1000, t / dur);   // 500→1000 au lieu de 300→600
        // ...
    });
}
```

### Modifier le HUD

Le HUD est créé automatiquement par `GameSceneSetup.cs`. Pour modifier :

- **Position** : Change les `Vector2` dans `CreateUIText()`
- **Taille** : Change le `fontSize` parameter
- **Couleur** : Modifie dans `HUDController.cs` les couleurs des textes

---

## 🏗️ Architecture technique

### Principes clés

1. **Gravité manuelle** — `Rigidbody2D.gravityScale = 0`, on applique `vy -= gravity * dt` dans FixedUpdate
2. **Saut speed-based** — Plus tu vas vite horizontalement, plus tu sautes haut
3. **Friction différenciée** — Glace au sol (0.96), quasi rien en l'air (0.998)
4. **Wall bounce** — Miroir de la vitesse + boost vertical proportionnel
5. **Sweep test** — Détection de collision plateforme par interpolation entre frames
6. **Seeded Random** — `System.Random(seed)` pour générer des mondes reproductibles
7. **Object pooling** — Plateformes recyclées (pas de `Instantiate`/`Destroy` en jeu)

### Flux du jeu

```
MenuScene                    GameScene
┌─────────┐                 ┌──────────────────────────┐
│  PLAY   │ ──────────────→ │  GameManager.StartGame() │
│  Mode   │                 │  WorldGenerator.Start()   │
│  Seed   │                 │  PlayerController         │
│  Perso  │                 │  CameraFollow + Rising    │
│  Ghost  │                 │  HUD + MobileControls     │
└─────────┘                 └──────────┬───────────────┘
     ↑                                 │ Game Over
     │                      ┌──────────▼───────────────┐
     └───────── MENU ────── │  GameOverUI               │
                            │  Score + Leaderboard      │
               RETRY ──────→│  Retry / Menu             │
                            └──────────────────────────┘
```

---

## 🐛 Debugging

### Problèmes fréquents

| Problème | Solution |
|----------|---------|
| Le joueur traverse les plateformes | Vérifie que `Physics2D.gravity.y = 0` dans ProjectSettings |
| Pas de saut | Vérifie que le joueur est `grounded` (il faut atterrir d'abord) |
| Plateformes invisibles | Vérifie que `BiomeManager` est présent dans la scène |
| Pas de son | Vérifie que `SoundManager` est dans la scène (DontDestroyOnLoad) |
| Touch ne marche pas | Vérifie le multi-touch : `Input.multiTouchEnabled = true` |
| Le jeu lag | Active le **Profiler** (Window > Analysis > Profiler) |

### Tester sur mobile depuis l'éditeur

1. **Window > Analysis > Input Debugger** — vérifier les inputs touch
2. **Game view** : change la résolution en **1080x1920 (Portrait)** pour simuler un mobile
3. Utilise **Unity Remote** (app gratuite) pour tester le touch depuis ton téléphone connecté en USB

---

## 📦 Pusher vers GitHub

Si tu as cloné le repo et fait des modifications :

```bash
cd TowerJumper/
git add -A
git commit -m "Ma modification"
git push origin main
```

Pour le premier push (repo vide) :

```bash
cd TowerJumper/
chmod +x push_to_github.sh
./push_to_github.sh
```

---

## 📄 Licence

Projet personnel — SraitoGaming © 2025-2026

---

## 🤝 Crédits

- Gameplay inspiré de **Icy Tower** par FreeVerse/Pyramid Games
- Style visuel inspiré de **Block Blast**
- Développé avec **Unity 2022.3 LTS**
