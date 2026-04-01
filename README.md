# Unlimited Local Vibe Coding on Linux
Dépendre d'un LLM payant et de ses quotas de tokens, c'est risquer d'être **interrompu** pendant une modification majeure en plein milieu ou ralentir un projet à délivrer.

Voici comment vibe coder librement sans limite, grâce à un modèle LLM local et l'agent IA **Aider**.

<em>Cette méthode est adaptable à Windows sans trop d'efforts (télécharger et installer manuellement les programmes dans les version citées)</em>

## Introduction
Ollama permet de lancer le modèle de notre choix. Nous verrons ici le modèle open weight (libre) "Qwen", proposé par Alibaba.

Aider est un outil de chat en ligne de commande qui vous permet de coder avec l'IA directement dans votre terminal. 

Vous pouvez choisir différents niveaux de performance, par exemple 3B ou 7B pour un laptop et 14B ou 32B si vous avez une carte graphique puissante. Ici pour coder confortablement sur un laptop on peut choisir **qwen2.5:7b**

## Prérequis
Avant de commencer, assurez-vous d'avoir les outils de compilation nécessaires.

Python 3.12 est le standard de l'industrie pour faire de l'IA, au jour de la publication.

Cependant si vous avez déjà une version plus récente, (ce qui est le cas sur Fedora 43), il faut isoler un environnement Python 3.12 pour éviter de multiples incompatibilités.

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

## Installation d'Aider
Sur Fedora 43+, Python 3.14 peut causer des erreurs de compilation avec les anciennes versions de numpy. La méthode la plus fiable est d'isoler l'installation, grâce à pipx. 
=> Ainsi, pas besoin de jongler avec des environnements avec .venv
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
Aider a besoin de l'URL de l'API Ollama pour communiquer. Ajoutez ceci à votre ~/.bashrc ou ~/.zshrc
```bash
export OLLAMA_API_BASE=http://localhost:11434
```
## Lancer Aider avec Qwen
Une fois configuré, placez-vous dans votre projet Git et lancez :
```bash
aider --model ollama/qwen2.5:7b
```
# Evolution
Vous pouvez télécharger et lancer le modèle Codestral de Mistral AI pour davantage de performances (modèle 22B optimisé code) : 
```bash
ollama pull codestral & ollama run codestral 
aider --model ollama/codestral
```

# Dépannage (Troubleshooting)
## Le modèle n'a pas l'air de se lancer
Vérifiez qu'Ollama tourne :
```bash
curl http://localhost:11434
```
Sinon, vous pouvez lancer manuellement Ollama :
```bash
sudo systemctl start ollama
```
## Erreur ImpImporter
Si vous rencontrez l'erreur suivante :
AttributeError: module 'pkgutil' has no attribute 'ImpImporter'

Root cause : Python 3.14 a supprimé des fonctions obsolètes utilisées par les vieux paquets.
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
