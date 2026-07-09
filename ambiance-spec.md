# StoryBeat — Ajouter une question « ambiance » sur l'écran musical

**Fichier : `index.html` (branche main).**

**But :** sur l'écran « univers musical » (celui qui contient la question du STYLE), ajouter une question d'**ambiance** juste à côté du style. La personne choisit ainsi son style musical ET l'ambiance voulue au même endroit.

## Où l'ajouter (et où NE PAS l'ajouter)

Ajouter UNIQUEMENT dans ces parcours, sur l'écran qui contient le bloc `key:"style"` :

- Catégorie **amour** → dans SES 6 liens : couple, enfant, parent, fratrie, gp, ami  
- Catégorie **naissance**  
- Catégorie **anniversaire**  
- Catégorie **mariage**

NE PAS l'ajouter à : **hommage**, **humour**, **clash** (ton déjà défini).

## Ce qu'il faut faire, pour CHACUN des parcours listés ci-dessus

1. Sur l'écran « univers musical » (celui qui contient `{type:"choice", key:"style", …}`), ajouter **juste après le bloc `style`** ce nouveau bloc :

{type:"choice", key:"ambiance", label:"Quelle ambiance doit transmettre la chanson ?", o:\["Romantique","Joyeuse","Nostalgique","Énergique","Calme","Drôle"\]},

2. Dans le `recap` de ce même parcours, ajouter une entrée pour l'ambiance, juste à côté de l'entrée du style (`["style","Style"]`) — par exemple placer `["ambiance","Ambiance"]` juste avant `["style","Style"]` :

\["ambiance","Ambiance"\],\["style","Style"\]

## Important

- Ne PAS toucher à la question existante « En écoutant, tu veux qu'il/elle ressente quoi ? » (key:"emotion") : elle reste telle quelle. On AJOUTE seulement l'ambiance.  
- Ne rien changer aux catégories hommage, humour, clash.  
- Le bloc `ambiance` est un `choice` (choix unique), optionnel comme les autres.

## Tests

1. Dans Amour (chaque lien), Naissance, Anniversaire, Mariage : l'écran musical affiche bien le style ET la nouvelle question « Quelle ambiance… » avec les 6 options.  
2. Hommage, Humour, Clash : PAS de question d'ambiance (inchangés).  
3. Le choix d'ambiance apparaît bien dans le récap final et dans l'e-mail EmailJS.  
4. Le reste du questionnaire fonctionne normalement.

Puis commit et push sur main.  
