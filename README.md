# POKECOLE - Administration des Exercices

## Architecture

Chaque fichier de questions est géré indépendamment avec sa propre version :
- `mathematiques_CP.json`, `mathematiques_CE1.json`, etc.
- Un fichier `index.json` liste toutes les versions

## Configuration GitHub

### 1. Créer le repository

1. Créez un repository GitHub : `pokecole-questions`
2. Rendez-le **public**
3. Créez un dossier `questions/` à la racine

### 2. Créer un Personal Access Token

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token
3. Cochez `repo` (accès complet aux repositories)
4. Copiez le token généré

### 3. Activer GitHub Pages

1. Settings → Pages
2. Source : Deploy from a branch
3. Branch : main, folder: / (root)
4. Save

URL : `https://VOTRE_USERNAME.github.io/pokecole-questions/`

### 4. Configurer l'application Godot

Modifiez `scripts/questions_sync.gd` :
```gdscript
const QUESTIONS_BASE_URL = "https://VOTRE_USERNAME.github.io/pokecole-questions/questions/"
```

## Utilisation de l'interface admin

### Ouvrir l'interface

Ouvrez `admin/index.html` dans un navigateur.

### Configurer GitHub

1. Entrez votre repository : `username/pokecole-questions`
2. Entrez votre token GitHub
3. Cliquez "Sauvegarder"
4. Testez la connexion

### Gérer les questions

1. Sélectionnez une **matière** et un **niveau**
2. Cliquez "📥 Charger" pour récupérer les questions depuis GitHub
3. Ajoutez/modifiez/supprimez des questions
4. Cliquez "🚀 Publier" pour envoyer sur GitHub

### Paramètre time_override

Chaque question peut avoir un temps personnalisé :
```json
{
    "id": 1,
    "type": "qcm",
    "question": "...",
    "time_override": 45
}
```

## Structure des fichiers sur GitHub

```
pokecole-questions/
├── questions/
│   ├── index.json          # Liste des versions
│   ├── mathematiques_CP.json
│   ├── mathematiques_CE1.json
│   ├── francais_CP.json
│   └── ...
└── index.html              # (optionnel) Interface admin
```

### Format index.json

```json
{
    "files": {
        "mathematiques_CP": {
            "version": "2024.01.18.1523",
            "count": 10,
            "updated_at": "2024-01-18T15:23:00Z"
        }
    },
    "updated_at": "2024-01-18T15:23:00Z"
}
```

### Format matiere_niveau.json

```json
{
    "matiere": "mathematiques",
    "niveau": "CP",
    "version": "2024.01.18.1523",
    "updated_at": "2024-01-18T15:23:00Z",
    "questions": [
        {
            "id": 1,
            "type": "qcm",
            "question": "Combien font 1 + 1 ?",
            "reponses": ["1", "2", "3", "4"],
            "reponse_correcte": 1,
            "time_override": 20
        }
    ]
}
```

## Synchronisation dans Godot

L'application vérifie automatiquement les mises à jour :

```gdscript
# Dans votre script de démarrage
QuestionsSync.sync_all()

# Écouter les événements
QuestionsSync.sync_completed.connect(func(success, message):
    print("Sync: ", message)
)
```

## Mode hors-ligne

L'application fonctionne hors-ligne :
1. Questions synchronisées : `user://questions/`
2. Questions par défaut : `res://data/questions/`
