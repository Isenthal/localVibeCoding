# Unlimited Local Vibe Coding on Linux
Dépendre d'un LLM payant et de ses quotas de tokens, c'est risquer d'être **interrompu** pendant une modification majeure en plein milieu ou ralentir un projet à délivrer.

Voici comment vibe coder librement sans limite, grâce à un modèle LLM local et l'agent IA **Aider**.

<em>Cette méthode est adaptable à Windows sans trop d'efforts (télécharger et installer manuellement les programmes dans les version citées)</em>

# TL;DR
Lancer un modèle gratuit au travers d'Ollama, avec Python 3.12. Installez **Aider** pour pouvoir travailler comme sur **Claude code**.

## Introduction
**Ollama** permet de lancer le modèle de notre choix. Nous verrons ici le modèle open weight (libre) "Qwen", proposé par Alibaba.

**Aider** est un outil de chat en ligne de commande qui vous permet de coder avec l'IA directement dans votre terminal. 

Vous pouvez choisir différents niveaux de performance, par exemple 7B pour un laptop et 14B ou 32B si vous avez une carte graphique puissante. Ici pour coder confortablement on peut choisir **qwen2.5-coder:7b**.

## Prérequis
Avant de commencer, assurez-vous d'avoir les outils de compilation nécessaires.

Python 3.12 est le standard de l'industrie pour faire de l'IA, au jour de la publication.

Cependant si vous avez déjà une version plus récente, (ce qui est le cas sur Fedora 43), il faut isoler un environnement Python 3.12 pour éviter de multiples incompatibilités.

```bash
sudo dnf install python3 python3-pip
python3 --version
sudo dnf groupinstall "Development Tools"
sudo dnf install python3.12 python3.12-devel gcc gcc-c++
```

## Installation d'Ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

## Installation d'Aider
Sur Fedora 43+, Python 3.14 peut causer des erreurs de compilation avec les anciennes versions de numpy. La méthode la plus fiable est d'isoler l'installation, grâce à pipx.

=> Ainsi, pas besoin de jongler avec de multiples environnements .venv
```bash
sudo dnf install pipx
pipx ensurepath
pipx install aider-chat --python /usr/bin/python3.12
```
C'est ici que la magie opère, on n'a pas besoin de configurer d'environnement, il a suffit de dire à pipx de cibler le binaire Python en version 3.12 et il se débrouillera ensuite pour tout faire de façon transparente.

## Repository Git
Si vous ne l'avez pas déjà fait, vous pouvez initialiser git dans votre répertoire de travail :
```bash
git init
git config --global init.defaultBranch main
git config user.name "votre nom"
git config user.email "toi@exemple.com"
```

Si vous ne le faites pas, **Aider** se débrouillera tout seul et initialisera un repo par défaut. Mais pour publier votre travail il faudra le faire à un moment ou à un autre.
Aider est un agent Git. S'il n'y a pas de repo, il perd 50% de sa puissance (pas de undo, pas de commits auto).

## Configuration d'Ollama
1 - Lancez votre modèle :
```bash
ollama run qwen2.5-coder:7b
```
2 - Configurez l'accès API :
Aider a besoin de l'URL de l'API Ollama pour communiquer. Ajoutez ceci à votre ~/.bashrc ou ~/.zshrc
```bash
export OLLAMA_API_BASE=http://localhost:11434
```
## Lancer Aider avec Qwen
Une fois configuré, dans un nouveau terminal, placez-vous dans votre projet Git et lancez :
```bash
aider --model ollama_chat/qwen2.5-coder:7b
```
# Evolution
Vous pouvez télécharger et lancer le modèle Codestral de Mistral AI pour davantage de performances (modèle 22B optimisé code) : 
```bash
ollama pull codestral && ollama run codestral 
aider --model ollama_chat/codestral
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
## Erreur "Python 3.12 not found"
`which python3.12` permet de confirmer que le chemin est bien `/usr/bin/python3.12`.

Si votre chemin est différent, il faut faire `pipx install aider-chat --python /chemin/de/python3.12 --force` 

## Erreur ImpImporter
Si vous rencontrez l'erreur suivante :
AttributeError: module 'pkgutil' has no attribute 'ImpImporter'

Root cause : Python 3.14 a supprimé des fonctions obsolètes utilisées par les vieux paquets.
Solution : forcez la mise à jour de numpy dans votre environnement avant d'installer Aider :

```bash
pipx inject aider-chat "numpy>=1.26.0"
```

# Licence
Ce projet est sous licence MIT - libre de réutilisation.

# Références
[Ollama](https://ollama.com/)

[Qwen2.5 coder](https://ollama.com/library/qwen2.5-coder)

[Mistral AI Codestral](https://mistral.ai/news/codestral)

[Aider](https://aider.chat/#getting-started)
