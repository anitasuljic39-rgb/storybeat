# StoryBeat — Mémo projet

> Document de reprise. À donner à Claude Code (ou à toute nouvelle conversation) pour repartir avec tout le contexte.
> Dernière mise à jour : juin 2026.

---

## 1. Le projet

**StoryBeat** vend des **chansons personnalisées sur-mesure**, livrées par mail.
Le client commande, remplit un questionnaire émotionnel, et reçoit sa chanson.

- **Différenciation** : 3 registres au-delà de l'émotionnel pur des concurrents → **Émotion / Humour / Clash**.
- **Délai** : **moins de 24h** (la plupart des concurrents sont à 72h).
- **Production** : via Suno (IA), retouchée par Anita. **Ne jamais mettre en avant le « comment ».** On vend une chanson personnalisée et l'émotion. Vocabulaire OK : « créée sur mesure », « composée pour vous », « unique ». À éviter : « nos artistes en studio » (faux).
- **Business distinct de Destiny Impact** → repo GitHub dédié : `anitasuljic39-rgb/storybeat` (déjà créé). Travailler sur **main** uniquement.

## 2. Modèle de vente

- **Tunnel** (funnel), pas site vitrine ni appli. Référence analysée : **MeCantara** (pub IG → page `/commander` → questionnaire → paiement → livraison email).
- **Outil retenu : Systeme.io** (tunnel + formulaire + paiement Stripe + emails auto, no-code).

### Étapes du tunnel
1. Page de vente (promesse + extrait audio + réassurance + 1 bouton)
2. Questionnaire (le cœur)
3. Paiement Systeme.io / Stripe — on y collecte **voix (H/F), email, prénom de l'acheteur**
4. Confirmation + email auto
5. Automatisation qui notifie Anita avec toutes les réponses

## 3. Questionnaire — principes validés

- **QCM d'abord** (le client réfléchit le moins possible) + micro-champs texte pour l'irremplaçable (prénom, souvenir, détail) → évite l'effet « template ».
- **Pas d'auto-avance** : le client clique, puis valide avec « Continuer ».
- **Champ libre « Ajouter / préciser » sur CHAQUE écran** (toujours présent, jamais bloquant).
- **Tout est facultatif.** **Tutoiement.**
- **Choix multiple** là où les réponses se cumulent (traits, émotions, contextes…). **Choix unique** là où une seule réponse a du sens.
- **Champs conditionnels** possibles (ex. fratrie : le champ « prénoms » n'apparaît que si « Oui »).
- La page **scrolle** : pas de limite de caractères, tout reste visible quel que soit le nombre de réponses.

### Écran d'entrée du tunnel (UNE seule fois)
Message d'accueil universel + choix des 7 catégories. Texte validé :
> **Crée sa chanson sur-mesure.**
> Quelques questions, deux minutes. Un secret : plus tes réponses sont précises — un prénom, un souvenir, une petite manie — **plus la chanson lui plaira.**
> *Une histoire chantée pour ne jamais s'effacer.* ✨
> *On raconte quelle histoire ?* → [7 catégories]

⚠️ Ce message n'apparaît QU'UNE FOIS, à l'entrée. Dans les pages test isolées il se répète : c'est normal, ça disparaît à l'assemblage. Chaque catégorie garde juste sa petite phrase d'ambiance en sous-titre de sa 1re question.

- **Phrase signature de marque** : « Une histoire chantée pour ne jamais s'effacer. »
- Branchement : la 1re question (« Quelle histoire ? ») aiguille vers les questions de cette seule catégorie.

### Le « moule » en 6 ingrédients (1 écran = 1 ingrédient minimum, on peut en ajouter)
1. **Le lien** — qui / pour qui / depuis quand
2. **Le moment** — UN souvenir concret
3. **Le détail signature** — phrase, manie, surnom (multi traits)
4. **L'émotion à provoquer** — **multi**
5. **Les messages** — **multi, max 3 (un par couplet)**
6. **L'univers musical** (dernier) — artiste/chanson **du destinataire** + QCM de styles en secours (styles larges : pop, variété, rap/slam, jazz/soul, classique, folk, R&B, etc.)

### Règles transverses à appliquer à TOUTES les catégories
- **Sexe / « il-elle »** : ajouter un choix Garçon/Fille (ou Homme/Femme) quand le prénom peut être mixte → nécessaire pour les accords de la chanson.
- **Rôle + prénom de l'offrant séparés** : ne pas mélanger « de la part de qui » (rôle : maman, oncle, parrain…) et le prénom de l'offrant → 2 champs distincts.

## 4. Design

Charte néon dérivée du logo StoryBeat : fond noir `#0b0a10`, rose `#ff3aa8`, violet `#9b4dff`, cyan `#1fd6ff`, dégradé rose→violet→cyan, motif onde sonore. Pastilles sélectionnées = remplies en dégradé + ✓ (haute lisibilité). Mobile-first, scroll vertical.
⚠️ NE PAS utiliser la charte dorée de Destiny Impact (univers différent).
Fichiers test = modèles de code à répliquer. Moteur JS commun : intro / choice (unique) / multi (avec `max` optionnel) / text / free (champ libre) / showIf (conditionnel) / récap final.

## 5. Avancement des catégories

| Catégorie | Statut | Écrans |
|---|---|---|
| Amour | ✅ Validé (moule de référence) | 6 |
| Hommage | ✅ Validé | 6 |
| Naissance | ✅ Validé (le plus abouti) | 8 |
| Anniversaire | ⏳ À faire | — |
| Humour | ⏳ À faire | — |
| Clash | ⏳ À faire | — |
| Mariage | ⏳ À faire | — |

### Détail NAISSANCE (8 écrans — référence la plus complète)
1. **Le bébé** : prénom · sexe (Garçon/Fille) · rôle de l'offrant (maman/papa/2 parents/grand-parent/oncle-tante/parrain-marraine/frère-sœur/famille) · prénom de l'offrant · libre
2. **Le jour J** : date+heure (texte) · contextes (multi : matin de neige, été, nuit d'orage, jour de fête, même jour qu'un proche, pile à terme, avant terme, etc.) · libre
3. **Ce que l'arrivée représente** : texte ouvert · multi (longtemps attendu, premier enfant, miracle après l'épreuve, arc-en-ciel après deuil, surprise, petit dernier, rêve réalisé) · libre
4. **La fratrie** : choix Oui/Non · si Oui → texte « prénoms + qui attendait le plus » · libre
5. **Ce qui rend ce bébé unique** : multi caractère (sacré caractère, tout doux, rayon de soleil, portrait de maman/papa, gourmand, dormeur, coquin) · « à qui ressemble-t-il » · libre
6. **L'émotion** (multi) : émerveillement, tendresse, fierté, joie, envie de protéger, belles larmes · libre
7. **Les vœux** (multi max 3) : amour inconditionnel, on sera là, vie de bonheur, sois libre, plus belle histoire, grandis en paix, le monde t'attend · « dis-lui avec tes mots »
8. **Univers musical** : artiste/chanson aimée (ex. un artiste que vous adorez) · styles (berceuse, pop tendre, variété, rap/slam, jazz/soul, classique, folk, comptine) · libre

### Détail AMOUR (6 écrans)
1. Lien : prénom + depuis quand · « Vous en êtes où ? » (unique) · libre
2. Moment : raconte (texte) · « pioche un type de moment » (unique, secours) · libre
3. Détail signature : traits (multi) · manie/surnom/phrase (texte) · libre
4. Émotion (**multi**) · libre
5. Messages (**multi max 3**) · « dis-le avec tes mots »
6. Univers musical : artiste/chanson du destinataire · styles (unique) · libre

### Détail HOMMAGE (6 écrans)
1. Lien : prénom + ton lien · « De son vivant / En sa mémoire » (unique) · libre
2. Souvenir : raconte · « pioche un type de souvenir » (unique) · libre
3. Ce qui la rendait unique : traits (multi) · phrase/surnom/geste (texte) · libre
4. Émotion (**multi**) : gratitude, fierté, belles larmes, tendresse, force/réconfort, sourire dans le manque · libre
5. Messages (**multi max 3**) : merci, tu me manques, fier de toi, tu vis en moi, tu m'as fait qui je suis, on ne t'oublie pas, je t'aime · « dis-le avec tes mots »
6. Univers musical : artiste/chanson aimée · styles · libre

## 6. Reste à faire

- [ ] Valider le contenu de : **Anniversaire, Humour, Clash, Mariage** (même process : Anita valide les questions ici avant code).
- [ ] Répercuter les règles transverses (sexe « il/elle », rôle+prénom offrant séparés) sur Amour et Hommage.
- [ ] **Assembler** le questionnaire complet (écran d'entrée 7 catégories + branchement) dans Claude Code sur le repo `storybeat`.
- [ ] **Page de vente** (titre, promesse, réassurance, bouton) angle Émotion/Humour/Clash + « moins de 24h ».
- [ ] Intégrer dans **Systeme.io** (ou héberger la page et brancher paiement). Collecte voix/email/prénom acheteur au paiement.

## 7. Fichiers du repo

- `STORYBEAT.md` — ce mémo (source de vérité).
- `storybeat-amour-test.html` — moule Amour validé.
- `storybeat-hommage-test.html` — Hommage validé.
- `storybeat-naissance-test.html` — Naissance validé (référence de code la plus complète : multi, max, showIf conditionnel, sexe, rôle+prénom).
