# StoryBeat — Mémo projet

> Document de reprise. À donner à Claude Code (ou à toute nouvelle conversation) pour repartir avec tout le contexte.
> Dernière mise à jour : juin 2026. **Les 7 catégories du questionnaire sont validées. Prochaine étape : assemblage.**

---

## 1. Le projet

**StoryBeat** vend des **chansons personnalisées sur-mesure**, livrées par mail.
Le client commande, remplit un questionnaire émotionnel, et reçoit sa chanson.

- **Différenciation** : 3 registres au-delà de l'émotionnel pur des concurrents → **Émotion / Humour / Clash**.
- **Délai** : **moins de 24h** (la plupart des concurrents sont à 72h).
- **Production** : via **Suno** (IA), retouchée par Anita. **Ne jamais mettre en avant le « comment ».** On vend une chanson personnalisée et l'émotion. OK : « créée sur mesure », « composée pour vous », « unique ». À éviter : « nos artistes en studio » (faux).
- ⚠️ **Suno = musique 100% originale.** Pas de reprise/parodie de tubes existants (droits d'auteur). On demande un artiste/style juste pour **s'en inspirer**.
- **Business distinct de Destiny Impact** → repo GitHub dédié : `anitasuljic39-rgb/storybeat`. Travailler sur **main** uniquement.

## 2. Modèle de vente

- **Tunnel** (funnel), pas site vitrine ni appli. Référence : **MeCantara** (pub IG → page `/commander` → questionnaire → paiement → livraison email).
- **Outil retenu : Systeme.io** (tunnel + formulaire + paiement Stripe + emails auto, no-code).
- Étapes : page de vente → questionnaire → paiement (collecte **voix H/F, email, prénom acheteur**) → confirmation + email auto → notification à Anita avec les réponses.

## 3. Questionnaire — principes validés

- **QCM d'abord** + micro-champs texte pour l'irremplaçable (prénom, souvenir, détail) → évite l'effet « template ».
- **Pas d'auto-avance** : le client clique, puis valide avec « Continuer ».
- **Champ libre « Ajouter / préciser » sur CHAQUE écran.**
- **Tout est facultatif. Tutoiement.**
- **Choix multiple** quand les réponses se cumulent ; **choix unique** quand une seule a du sens.
- **Champs conditionnels** (`showIf`). **Re-render** d'écran pour les choix pilotes (`controls`). Punchlines longues = chips pleine largeur (`long`).
- La page **scrolle** : aucune limite de caractères.

### Écran d'entrée du tunnel (UNE seule fois) — message universel + choix des 7 catégories
> **Crée sa chanson sur-mesure.**
> Quelques questions, deux minutes. Un secret : plus tes réponses sont précises — un prénom, un souvenir, une petite manie — **plus la chanson lui plaira.**
> *Une histoire chantée pour ne jamais s'effacer.* ✨
> *On raconte quelle histoire ?* → [7 catégories]

⚠️ Ce message n'apparaît QU'UNE FOIS, à l'entrée. Dans les pages test isolées il se répète : normal, ça disparaît à l'assemblage.
- **Phrase signature de marque** : « Une histoire chantée pour ne jamais s'effacer. »

### Le moule (1 écran = 1 ingrédient ; on peut ajouter des écrans)
Lien → moment → détail signature → émotion (**multi**) → messages (**multi max 3, un par couplet**) → univers musical (artiste du destinataire + QCM styles larges en secours).

### Règles transverses (TOUTES catégories)
- **Sexe / accords** : choix Homme/Femme/Garçon/Fille quand le prénom peut être mixte.
- **Rôle + prénom de l'offrant séparés** (2 champs : le rôle, puis le prénom).

## 4. Design

Charte néon du logo : fond `#0b0a10`, rose `#ff3aa8`, violet `#9b4dff`, cyan `#1fd6ff`, dégradé rose→violet→cyan, onde sonore. Pastilles sélectionnées = remplies en dégradé + ✓. Mobile-first, scroll vertical.
⚠️ NE PAS utiliser la charte dorée de Destiny Impact.
Moteur JS commun (voir fichiers test) : intro / choice (unique) / multi (`max`, `long`) / text / area / free / showIf / controls / récap final.

## 5. Avancement des catégories — ✅ LES 7 SONT VALIDÉES

| Catégorie | Statut | Écrans |
|---|---|---|
| Amour | ✅ Validé (moule de référence) | 6 |
| Hommage | ✅ Validé | 6 |
| Naissance | ✅ Validé | 8 |
| Anniversaire | ✅ Validé | 7 |
| Humour | ✅ Validé | 6 |
| Clash | ✅ Validé | 7 |
| Mariage | ✅ Validé | 7 |

Chaque catégorie a son fichier `storybeat-<cat>-test.html` = source de vérité du contenu ET du code. Pour assembler, repartir de ces fichiers (ne pas réinventer les libellés).

### Détail MARIAGE (7 écrans)
1. Pour quel mariage : prénoms des mariés · « Tu chantes… » (à ma moitié / aux mariés / discours témoin / surprise) · ton rôle · ton prénom · libre
2. Leur histoire : comment ça a commencé (texte) · « Ils en sont où ? » (jeunes mariés / années / longue date officialisée / retrouvés) · libre
3. Ce qui les définit : multi (complices, inséparables, drôles, solides, aventuriers, tendres, pétillants, yin-yang, fusionnels, piliers) · leur private joke/surnom · libre
4. Émotion (multi) : émouvante, festive, tendre, drôle, romantique, gratitude · libre
5. Messages (multi max 3) : faits l'un pour l'autre, vie de bonheur, merci de nous réunir, amour éternel, fiers de vous, mille aventures · « dis-le avec tes mots »
6. Le grand jour : détail du mariage (texte) · « la chanson c'est pour… » (cérémonie / première danse / soirée / à offrir) · libre
7. Univers musical : artiste/chanson du couple · styles (pop romantique, variété, soul/R&B, ballade, festif, classique) · libre

## 6. Reste à faire

- [ ] **Assembler** le questionnaire complet dans Claude Code : écran d'entrée (message universel + choix des 7 catégories) + branchement vers la bonne catégorie + récap final commun. Réutiliser le moteur JS des fichiers test.
- [x] Répercuter les règles transverses (sexe « il/elle », rôle+prénom offrant) sur **Amour** et **Hommage** (validées avant ces règles).
- [ ] **Page de vente** (titre, promesse, réassurance, bouton) angle Émotion/Humour/Clash + « moins de 24h ».
- [ ] Intégrer dans **Systeme.io** (ou héberger + brancher paiement). Collecte voix/email/prénom acheteur au paiement.

## 7. Fichiers du repo

- `STORYBEAT.md` — ce mémo (source de vérité).
- `storybeat-amour-test.html` — Amour (moule de référence).
- `storybeat-hommage-test.html` — Hommage.
- `storybeat-naissance-test.html` — Naissance (showIf conditionnel, sexe, rôle+prénom).
- `storybeat-anniversaire-test.html` — Anniversaire (écran âge/cap).
- `storybeat-humour-test.html` — Humour (garde-fou, phrasing Suno).
- `storybeat-clash-test.html` — Clash (niveau de chauffe + garde-fou discret, punchlines `long`).
- `storybeat-mariage-test.html` — Mariage (« tu chantes… », le grand jour).
