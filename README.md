# Unlimited Local Vibe Coding
Comment vibe coder librement (illimité, modèle open weight), grâce à l'agent IA **Aider** 
Dépendre d'un LLM payant et de ses quotas de tokens, c'est risquer d'être interrompu pendant une modification majeure en plein milieu ou ralentir un projet à délivrer.

# Utiliser Ollama en Vibe Coding sur Linux
Cible : Fedora 43+ et le modèle QWEN
Le modèle codestral (12Gb) est particulièrement bon pour coder avec Aider, mais il fait 22B.

Python 3.12 est le standard de l'industrie à ce jour (2026-04-01) pour faire de l'IA, car toutes les bibliothèques lourdes (Numpy, PyTorch) y sont déjà optimisées et pré-compilées.

Cependant si vous avez une version plus récente, ou que vous restez sur la version de Fedora 43, il faut travailler dans Python 3.12 pour éviter de multiples incompatibilités.

## Introduction
Ollama permet de lancer localement un modèle. Nous verrons ici le modèle open weight Qwen, proposé par Alibaba.

Aider est un outil de chat en ligne de commande qui vous permet de coder avec l'IA directement dans votre terminal. 
En le couplant à Ollama, vous gardez vos données en local.
Vous pouvez choisir différents niveaux de performance, par exemple 3B ou 7B pour un laptop et 14B ou 32B si vous avez une carte graphique puissante.

## Prérequis
Avant de commencer, assurez-vous d'avoir les outils de compilation nécessaires (Fedora n'inclut pas tout par défaut) :
```bash
sudo dnf install python3 python3-pip
python3 --version
sudo dnf groupinstall "Development Tools"
sudo dnf install python3-devel gcc gcc-c++
```

## Installation d'Ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Vérifiez qu'Ollama tourne :
```bash
curl http://localhost:11434
```

Sinon, vous pouvez lancer manuellement Ollama :
```bash
sudo systemctl start ollama
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

```bash
pip install "numpy>=1.26.0"
```

# Licence
Ce projet est sous licence MIT - libre de réutilisation.

# Références
[Ollama](https://ollama.com/)


[Codestral](https://mistral.ai/news/codestral)


[Aider](https://aider.chat/#getting-started)
