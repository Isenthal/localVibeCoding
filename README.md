# localVibeCoding
Comment coder librement sans dépendre d'un LLM payant.

# Utiliser Aider avec Ollama (modèle open weight Qwen2.5) sur Fedora 43+

Ce tutoriel explique comment installer l'agent IA **Aider** et le connecter à un modèle local avec **Ollama**
Python 3.12 est la bonne version à utiliser à ce jour (2026-04-01)

Cependant si vous avez une version plus récente, nous verrons aussi comment résoudre les problèmes de compatibilité (Python 3.14+).

## Introduction
Aider est un outil de chat en ligne de commande qui vous permet de coder avec l'IA directement dans votre terminal. 
En le couplant à Ollama, vous gardez vos données en local.
Vous pouvez choisir différents niveaux de performance, par exemple 3B ou 7B pour un laptop et 14B ou 32B si vous avez une carte graphique puissante.

## Prérequis
Avant de commencer, assurez-vous d'avoir les outils de compilation nécessaires (Fedora n'inclut pas tout par défaut) :

```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install python3-devel gcc gcc-c++
```

## Installation d'Aider
Sur Fedora 43+, Python 3.14 peut causer des erreurs de compilation avec les anciennes versions de numpy. La méthode la plus fiable est d'isoler l'installation.
```bash
sudo dnf install pipx
pipx ensurepath
# Installation forcée avec une version de numpy compatible
pipx install aider-chat
```

## Configuration d'Ollama
1 - Lancez votre modèle :
```bash
ollama run qwen2.5:7b
```
2 - Configurez l'accès API :
Aider a besoin de l'URL de l'API Ollama pour communiquer. Ajoutez ceci à votre .bashrc ou .zshrc
```bash
export OLLAMA_API_BASE=http://localhost:11434
```
## Lancer Aider avec Qwen
Une fois configuré, placez-vous dans votre projet Git et lancez :
```bash
aider --model ollama/qwen2.5:7b
```
# Dépannage (Troubleshooting)
Si vous rencontrez l'erreur suivante :
AttributeError: module 'pkgutil' has no attribute 'ImpImporter'

Cause : Python 3.14 a supprimé des fonctions obsolètes utilisées par les vieux paquets.
Solution : Forcez la mise à jour de numpy dans votre environnement avant d'installer Aider :
Bash

pip install "numpy>=1.26.0"

# Licence
Ce projet est sous licence MIT - libre de réutilisation.
