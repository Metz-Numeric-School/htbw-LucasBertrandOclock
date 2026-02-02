# Procédure de Déploiement

Décrivez ci-dessous votre procédure de déploiement en détaillant chacune des étapes. De la préparation du VPS à la méthodologie de déploiement continu.

## Préparation du VPS

### Phase 1 : Mise a jour et téléchargement des ressources

Téléchargement de AAPanel avec la commande :

>URL=https://www.aapanel.com/script/install_7.0_en.sh && if [ -f /usr/bin/curl ];then curl -ksSO "$URL" ;else wget --no-check-certificate -O install_7.0_en.sh "$URL";fi;bash install_7.0_en.sh aapanel

Récupération des données de connexion :

- Lien d'accès : https://178.251.86.104:29695/d5478884
- Identifiant : yet4beqz
- Mot de passe : 42d1d642

Commande utilisée pour la récupération lorsque que AAPanel est installé


`bt`  
`14`

(14) View panel default info

### Phase 2 : Mise en place du dépot distant sur le vps

Créer le dépôt :

`
"sudo" apt install git` (on verifie que git est installé pour la suite)
`cd var/`  
`mkdir depot_git`  
`cd depot_git`  
`git init --bare` (crée un dépôt sans working tree)

## Préparation du dépôt Git local
### Phase 1 : Mise en place du versioning
On initialise git cliff :  
`git cliff --init`

On met ensuite en place le CHANGELOG.md  
Exemple :
```md
# Changelog
All notable changes to this project will be documented in this file.

## [Unreleased]
### Added
### Changed
### Fixed
### Removed

## [1.0.0] - 2026-02-02
### Added
- Recupération de la structure du projet pour déploiement
- Ajout des informations utiles à AAPanel dans PANEL.md
- Ajout des explications de déploiements dans DEPLOY.md
```
### Phase 2 : Connexion du dépôt git local vers le dépôt distant
On connecte les dépôt git en ajoutant le remote distant au local :  
`git remote add vps root@192.168.23.139:/var/depot_git`

### Phase 3 : Mise en place de l'envoie de version via git tag

On utilise git tag pour versionner notre code et envoyer au vps uniquement les versions voulu

`git tag 1.0.0`  
`git push -u vps 1.0.0`


## Méthode de déploiement

Todo...
