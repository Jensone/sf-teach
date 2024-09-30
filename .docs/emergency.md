---
description: Comment faire si je suis en galère ?
---

## Problèmes avec la BDD

Procédure de résolution avec la BDD :

Supprimer la BDD défectueuse
  
```bash
symfony console d:d:d --force
```

Création de la base de données vide

```bash
symfony console d:d:c
```

Suppression de tous les fichiers de migration

```bash
symfony console make:migration
```

Execution de toutes les migrations

```bash
symfony console d:m:m -n
```

Lancer les fixtures

```bash
symfony console d:f:l -n
```

---

## Problème avec l'installation d'un package

Procédure de résolution :

