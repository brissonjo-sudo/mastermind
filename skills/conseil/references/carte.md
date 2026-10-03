# Schéma de CARTE — source canonique

Format d'échange interne entre personas, revue croisée et Chairman. Vit dans le skill
(et non dans `CLAUDE.md`) pour voyager avec le plugin : le `CLAUDE.md` du dépôt n'est
pas chargé chez un utilisateur qui installe le plugin.

```json
{
  "these": "1 phrase",
  "preuves": ["fait + source si dispo", "..."],
  "confiance": "élevée | moyenne | faible",
  "drapeaux": ["hypothèse non vérifiée", "point à confirmer"]
}
```

Les personas gardent dans leur fichier un exemple de CARTE adapté à leur rôle
(contenu différent, mêmes 4 champs). Modifier le schéma = modifier ce fichier
**et** vérifier les 4 personas.
