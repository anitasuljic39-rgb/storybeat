# StoryBeat — Mémo projet

> Document de reprise. À donner à Claude Code (ou à toute nouvelle conversation) pour repartir avec tout le contexte.
> Dernière mise à jour : juin 2026.

---

## 1. Le projet

**StoryBeat** vend des **chansons personnalisées sur-mesure**, livrées par mail.
Le client commande, remplit un questionnaire émotionnel, et reçoit sa chanson.

- **Différenciation clé** : 3 registres au lieu du seul registre émotionnel des concurrents → **Émotion / Humour / Clash**.
- **Délai de livraison** : **moins de 24h** (argument fort : la plupart des concurrents sont à 72h).
- **Production** : faite via Suno (IA), retouchée par Anita. **Ne jamais mettre en avant le « comment ».** On vend une chanson personnalisée et l'émotion, pas l'outil. Vocabulaire OK : « créée sur mesure », « composée pour vous », « unique ». À éviter : « nos artistes en studio » (faux).
- **Business distinct de Destiny Impact** → repo GitHub séparé et dédié (`storybeat`).

## 2. Modèle de vente

- **Tunnel** (funnel), pas site vitrine ni application. Une page → un objectif : commander.
- Référence concurrente analysée : **MeCantara** (pub Instagram → page `/commander` avec questionnaire → paiement → livraison email). C'est le modèle à répliquer.
- **Outil retenu : Systeme.io** (tunnel, formulaire, paiement Stripe, emails automatiques, démarrage gratuit, no-code).

### Étapes du tunnel
1. Page de vente (promesse + extrait audio + réassurance + 1 bouton)
2. Questionnaire (le cœur — qualifie la commande ET engage le client)
3. Paiement (Systeme.io / Stripe) — c'est ici qu'on collecte **voix (H/F), email, prénom de l'acheteur**
4. Page de confirmation + email automatique
5. Automatisation qui notifie Anita avec toutes les réponses

## 3. Le questionnaire — principes validés

- **QCM d'abord** pour que le client réfléchisse le moins possible, MAIS on garde des micro-champs texte pour l'irremplaçable (prénom, souvenir, détail) → c'est ce qui rend la chanson vraiment personnalisée et évite l'effet « template ».
- **Pas d'auto-avance** : le client clique ses réponses, et c'est LUI qui valide avec « Continuer ».
- **Champ libre « Ajouter / préciser » sur chaque écran** (toujours présent, jamais bloquant).
- **Tout est facultatif** (tout est sautable).
- **Tutoiement.**
- **Écran d'accueil** au début : rappeler que plus les réponses sont précises, plus la chanson est personnalisée (« Les détails font toute la magie »).
- **Branchement** : tout part de la 1re question « Quelle histoire ? » → on ne pose ensuite QUE les questions de cette histoire.

### Le « moule » en 6 ingrédients (1 écran = 1 ingrédient)
1. **Le lien** — qui / pour qui / depuis quand (le décor)
2. **Le moment** — UN souvenir concret (le cœur émotionnel)
3. **Le détail signature** — phrase, manie, surnom (l'empreinte unique)
4. **L'émotion à provoquer** — couleur de la chanson → **choix multiple**
5. **Les messages** — ce qu'on veut dire → **choix multiple, max 3 (un par couplet)**
6. **L'univers musical** (dernier écran) — artiste/chanson **du destinataire** + QCM de styles en secours

## 4. Moule AMOUR — VALIDÉ (référence pour les autres)

Ordre : Accueil → Lien → Moment → Détail signature → Émotion (multi) → Messages (multi max 3) → Univers musical.

- **Lien** : `prénom + depuis quand` (texte ex.) · `Vous en êtes où ?` (choix unique) · champ libre
- **Moment** : `raconte-le` (texte ex.) · `pioche un type de moment` (choix unique, secours) · champ libre
- **Détail signature** : `traits` (multi) · `sa manie / surnom / phrase culte` (texte ex.) · champ libre
- **Émotion** : `l'émotion à provoquer` (**multi**, sans limite) · champ libre
- **Messages** : `jusqu'à 3 messages` (**multi, max 3 — un par couplet**) · `dis-le avec tes mots` (texte)
- **Univers musical** : `artiste/chanson du destinataire` (texte ex.) · `style` (choix unique, secours) · champ libre

Design : charte néon StoryBeat dérivée du logo (fond noir #0b0a10, rose #ff3aa8, violet #9b4dff, cyan #1fd6ff ; dégradé rose→violet→cyan ; motif onde sonore). Pastilles sélectionnées = remplies en dégradé + ✓ (haute lisibilité). Mobile-first.
⚠️ Ne PAS utiliser la charte dorée de Destiny Impact — univers différent.

## 5. Reste à faire

- [ ] Décliner les **6 autres histoires** sur le moule Amour : Hommage, Naissance, Anniversaire, Humour, Clash, Mariage (adapter libellés/réponses ; ex. pour Clash, « messages » → « piques », « émotion » → « niveau de chauffe »).
- [ ] **Page de vente** (texte : titre, promesse, réassurance, bouton) avec angle Émotion/Humour/Clash + « moins de 24h ».
- [ ] Intégrer le questionnaire dans **Systeme.io** (ou héberger la page HTML et brancher le paiement).
- [ ] Collecte voix/email/prénom acheteur à l'étape paiement.

## 6. Fichiers

- `storybeat-amour-test.html` — page test du moule Amour (mécanique validée).
- `STORYBEAT.md` — ce mémo.
