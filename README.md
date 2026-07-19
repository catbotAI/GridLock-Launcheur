[README.md](https://github.com/user-attachments/files/30166942/README.md)
# GridLock Launcher

Launcher Windows multi-jeux de **GridLock Studio**, construit avec Electron dans un style neon inspire de Matrix.

![Apercu de GridLock Launcher](smoke-launcher.png)

## Fonctionnalites

- Bibliotheque centralisee pour tous les jeux GridLock
- Installation, reparation et lancement depuis une seule interface
- Detection automatique des jeux absents ou incomplets
- Verification des nouvelles versions toutes les 60 secondes
- Telechargements decoupes en plusieurs parties pour les fichiers volumineux
- Verification SHA-256 de chaque partie et de l'archive finale
- Mises a jour independantes du launcher et de chaque jeu
- Stockage des jeux dans le dossier utilisateur du launcher
- Lancement direct par `GridLock-Launcher.exe`, sans fichier `.bat`

## Jeux

| Jeu | Version | Description |
| --- | --- | --- |
| **GridLock Zero** | 3.3.0 | Jeu de survie arcade avec les modes Impossible Game, Hide and Seek et Laser Game. |
| **GridLock One** | 1.1.0 | Platformer de precision compose de 10 niveaux de plus en plus difficiles. |
| **GridLock Random** | 1.0.0 | Platformer procedural de 20 secteurs avec 26 types de plateformes, pieges et bonus. |

## Installation utilisateur

1. Telecharge l'archive Windows du launcher.
2. Extrais completement le dossier `GridLock-Launcher-win32-x64`.
3. Lance `GridLock-Launcher.exe`.
4. Choisis un jeu dans la barre laterale puis clique sur **Installer**.

Le dossier complet doit rester ensemble : l'executable Electron utilise les fichiers presents dans `resources`, `locales` et les autres fichiers du paquet.

> L'application n'est actuellement pas signee. Windows SmartScreen peut donc afficher **Editeur inconnu**. Ne telecharge le launcher que depuis la page officielle de GridLock Studio.

## Commandes

Pre-requis : Windows 10 ou 11 x64, Node.js et npm.

```powershell
cd work\gridlock-launcher
npm.cmd install
npm.cmd start
```

### Tests

```powershell
npm.cmd run smoke
npm.cmd run smoke:remote
```

Le premier test controle l'interface Electron. Le second verifie le manifeste distant et le debut d'une archive de jeu.

### Compilation Windows

```powershell
npm.cmd run package:win
```

L'executable compile est genere ici :

```text
release/GridLock-Launcher-win32-x64/GridLock-Launcher.exe
```

## Mises a jour

Le launcher recupere un manifeste JSON distant qui contient :

- la version actuelle du launcher ;
- la version disponible de chaque jeu ;
- les liens de telechargement ;
- les empreintes SHA-256 attendues.

Lorsqu'une version plus recente est disponible, le bouton devient **Mettre a jour**. Le nouveau paquet est telecharge et controle avant de remplacer l'ancienne installation.

Les archives volumineuses peuvent etre preparees avec :

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\split-update.ps1 `
  -Source updates\GridLock-Launcher-win32-x64.zip `
  -OutputDirectory updates\drive-parts\launcher
```

## Structure

```text
electron/       Processus principal Electron et gestion des jeux
renderer/       Interface du launcher et couvertures
scripts/        Tests, icone, extraction et preparation des mises a jour
updates/        Manifeste local et fichiers de version
release/        Builds Windows generes
```

## Securite

- Les sommes SHA-256 detectent les fichiers incomplets ou modifies.
- Le renderer Electron n'obtient pas un acces Node.js direct.
- Les jeux sont lances uniquement depuis leur dossier d'installation attendu.
- Aucun antivirus ni avertissement Windows n'est contourne par le launcher.

## Statut

GridLock Launcher est en developpement actif. Les versions actuelles sont destinees aux tests et peuvent encore contenir des bugs.

---

**GridLock Studio**
