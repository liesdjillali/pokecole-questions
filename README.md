# POKECOLE - Administration des Exercices

## Configuration GitHub Pages

### 1. Créer un repository GitHub

1. Allez sur [github.com](https://github.com) et créez un nouveau repository nommé `pokecole-questions`
2. Rendez-le **public** (requis pour GitHub Pages gratuit)

### 2. Uploader les fichiers

Uploadez ces fichiers dans le repository :
- `index.html` - Interface d'administration
- `version.json` - Fichier de version
- `questions.json` - Fichier des questions

### 3. Activer GitHub Pages

1. Allez dans **Settings** > **Pages**
2. Sous "Source", sélectionnez **Deploy from a branch**
3. Choisissez la branche `main` et le dossier `/ (root)`
4. Cliquez sur **Save**

Votre URL sera : `https://VOTRE_USERNAME.github.io/pokecole-questions/`

### 4. Configurer l'application Godot

Modifiez le fichier `scripts/questions_sync.gd` :

```gdscript
const QUESTIONS_BASE_URL = "https://VOTRE_USERNAME.github.io/pokecole-questions/"
```

## Utilisation de l'interface d'administration

### Accéder à l'interface

Ouvrez `https://VOTRE_USERNAME.github.io/pokecole-questions/` dans votre navigateur.

### Ajouter des questions

1. Sélectionnez une **matière** et un **niveau**
2. Cliquez sur **➕ Ajouter une question**
3. Remplissez le formulaire :
   - **Type** : QCM, Vrai/Faux, ou Texte
   - **Question** : Le texte de la question
   - **Temps imparti** (optionnel) : Override le temps par défaut
   - **Réponses** : Selon le type de question

### Exporter les questions

1. Cliquez sur **📦 Exporter tout** pour télécharger le fichier JSON complet
2. Uploadez ce fichier (`questions.json`) sur GitHub
3. Mettez à jour `version.json` avec une nouvelle version

### Structure du fichier questions.json

```json
{
    "version": "2024.01.18.1",
    "updated_at": "2024-01-18T12:00:00Z",
    "questions": {
        "francais_CP": [
            {
                "id": 1,
                "type": "qcm",
                "question": "Quelle lettre...",
                "reponses": ["A", "B", "C", "D"],
                "reponse_correcte": 1,
                "time_override": 45
            }
        ],
        "mathematiques_CE1": [...]
    }
}
```

### Paramètre time_override

Chaque question peut avoir un paramètre `time_override` optionnel qui définit un temps personnalisé en secondes pour cette question spécifique.

- Si `time_override` est présent et > 0, il sera utilisé
- Sinon, le temps par défaut de la matière/niveau sera utilisé

## Synchronisation automatique

L'application Godot vérifie automatiquement les mises à jour au démarrage :

1. Télécharge `version.json`
2. Compare avec la version locale
3. Si différente, télécharge `questions.json`
4. Met en cache les questions localement

### Mode hors-ligne

L'application fonctionne hors-ligne grâce au cache local :
- Les questions synchronisées sont stockées dans `user://questions/`
- Les questions par défaut sont dans `res://data/questions/`

## Import des questions existantes

Dans l'interface admin, vous pouvez importer vos fichiers JSON existants :
1. Ouvrez la console du navigateur (F12)
2. Exécutez : `importExistingQuestions()`
3. Sélectionnez vos fichiers JSON

## Support

Les types de questions supportés :
- **qcm** : Choix multiple (4 réponses)
- **vrai_faux** : Vrai ou Faux
- **texte** : Réponse libre

Les matières :
- Mathématiques
- Français
- Géographie
- Anglais

Les niveaux :
- CP, CE1, CE2, CM1, CM2
