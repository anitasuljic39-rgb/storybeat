# StoryBeat — Page de vente : cohérence « on paie d'abord » \+ section preuve sociale

**Fichier concerné : `page-de-vente.html` (branche main).**

Deux objectifs :

1. **Corriger** tous les textes qui décrivent un modèle « gratuit d'abord » (faux), pour refléter le vrai modèle : **on paie d'abord → extrait à valider → satisfaite ou remboursée**.  
2. **Ajouter** une section « preuve sociale » (chiffres \+ 4 témoignages) juste après les formules.

---

## PARTIE 1 — Corrections de textes (remplacements exacts)

Remplace chaque texte À GAUCHE par le texte À DROITE. Ne change rien d'autre sur la ligne.

**1\. Barre de réassurance (haut de page) — 2 items :**

- `<div class="item"><span class="ic">👂</span> Ton extrait offert</div>` → `<div class="item"><span class="ic">👂</span> Un extrait à valider</div>`  
- `<div class="item"><span class="ic">✅</span> Tu ne paies que si tu adores</div>` → `<div class="item"><span class="ic">✅</span> Satisfaite ou remboursée</div>`

**2\. Les 3 formules (puce « Extrait gratuit avant d'acheter ») :** Dans les 3 `<ul>` des formules (Essentielle, Populaire, Premium), remplacer CHAQUE occurrence de :

- `<li>Extrait gratuit avant d'acheter</li>` → `<li>Un extrait à valider avant la version finale</li>`

**3\. Section GARANTIE (titre \+ paragraphe) :**

- `<h2>Écoute avant de payer.</h2>` → `<h2>Satisfaite, ou remboursée.</h2>`  
- `<p>On t'envoie d'abord un extrait de la chanson, gratuitement. Il te touche ? Tu débloques la version complète. Sinon, tu ne paies rien. Aucune raison d'hésiter.</p>` → `<p>Après ton paiement, on t'envoie un extrait de ta chanson à valider. Il te touche ? Tu reçois la version complète. Pas convaincue ? Tu es remboursée. Aucune raison d'hésiter.</p>`

**4\. FAQ (une réponse) :**

- `<p>Tu reçois un extrait gratuit avant tout achat. Tu ne paies que si elle te plaît. Zéro risque.</p>` → `<p>Après ton achat, tu reçois d'abord un extrait à valider. S'il ne te convainc pas, tu es remboursée. Zéro risque.</p>`

---

## PARTIE 2 — Nouvelle section « preuve sociale »

### 2.1 — Où l'insérer

Juste APRÈS la section des formules (la ligne `</div></section>` qui ferme `<section class="pricing" id="sb-offres">`) et AVANT le commentaire `<!-- GARANTIE -->`. Ordre final voulu : Formules → **Preuve sociale** → Garantie → FAQ → CTA final.

### 2.2 — HTML à insérer

  \<\!-- PREUVE SOCIALE \--\>

  \<section class="proof"\>\<div class="wrap"\>

    \<div class="eyebrow center"\>\<span class="dot"\>\</span\>Ils nous ont fait confiance\</div\>

    \<h2 class="center" style="font-size:clamp(26px,4vw,38px);margin-bottom:8px"\>Des milliers d'histoires déjà chantées.\</h2\>

    \<div class="proof-stats"\>

      \<div class="stat"\>\<div class="num"\>3 000+\</div\>\<div class="lab"\>chansons composées\</div\>\</div\>

      \<div class="stat"\>\<div class="num"\>98 %\</div\>\<div class="lab"\>de clients satisfaits\</div\>\</div\>

      \<div class="stat"\>\<div class="num"\>24h\</div\>\<div class="lab"\>de livraison\</div\>\</div\>

    \</div\>

    \<div class="tmoins"\>

      \<div class="tmoi"\>

        \<div class="stars"\>★★★★★\</div\>

        \<p\>« Ma mère a pleuré en écoutant la chanson, c'était le plus beau cadeau que je pouvais lui offrir. Tous ceux qui l'ont écoutée ont kiffé \! Franchement vous avez assuré \! Merci. »\</p\>

        \<div class="who"\>Julie, 22 ans\</div\>

      \</div\>

      \<div class="tmoi"\>

        \<div class="stars"\>★★★★★\</div\>

        \<p\>« Je ne savais pas à quoi m'attendre, mais le résultat a dépassé toutes mes espérances. Toute la famille a été émue. »\</p\>

        \<div class="who"\>Martine, 56 ans\</div\>

      \</div\>

      \<div class="tmoi"\>

        \<div class="stars"\>★★★★★\</div\>

        \<p\>« La chanson raconte notre histoire avec une justesse incroyable. On l'écoute encore aujourd'hui avec beaucoup d'émotion. Merci. »\</p\>

        \<div class="who"\>Nicolas, 42 ans\</div\>

      \</div\>

      \<div class="tmoi"\>

        \<div class="stars"\>★★★★★\</div\>

        \<p\>« J'ai offert une chanson d'amour personnalisée à mon copain. Comme il écoute souvent du rap, j'avais un peu peur que le résultat fasse ringard ou ne corresponde pas à ses goûts. Finalement, j'ai été bluffée : la chanson était parfaitement dans son style et il a adoré. Je suis vraiment très contente du résultat \! »\</p\>

        \<div class="who"\>Léa, 25 ans\</div\>

      \</div\>

    \</div\>

  \</div\>\</section\>

### 2.3 — CSS à ajouter

Ajoute ces règles dans le `<style>`, juste après les règles `.sb .guarantee …` (elles réutilisent les variables existantes : \--card, \--line, \--grad3, \--muted, \--display, \--white) :

  .sb .proof-stats{display:flex;flex-wrap:wrap;gap:18px;justify-content:center;margin:30px 0 40px}

  .sb .stat{flex:1 1 180px;max-width:240px;text-align:center;background:var(--card);border:1px solid var(--line);border-radius:16px;padding:24px 18px}

  .sb .stat .num{font-family:var(--display);font-weight:800;font-size:clamp(32px,5vw,44px);line-height:1;background:var(--grad3);-webkit-background-clip:text;background-clip:text;color:transparent}

  .sb .stat .lab{color:var(--muted);font-size:14px;margin-top:8px;font-weight:600}

  .sb .tmoins{display:grid;grid-template-columns:1fr 1fr;gap:18px;max-width:900px;margin:0 auto}

  .sb .tmoi{background:var(--card);border:1px solid var(--line);border-radius:18px;padding:24px 22px;display:flex;flex-direction:column;gap:10px}

  .sb .tmoi .stars{color:\#ffb43a;font-size:15px;letter-spacing:2px}

  .sb .tmoi p{color:rgba(255,255,255,.9);font-size:15px;line-height:1.55;font-style:italic;margin:0}

  .sb .tmoi .who{font-family:var(--display);font-weight:700;font-size:14px;color:var(--white);margin-top:auto}

  @media(max-width:640px){.sb .tmoins{grid-template-columns:1fr}}

---

## PARTIE 3 — Tests

1. Plus AUCUN texte « gratuit avant d'acheter / tu ne paies rien / que si tu adores / que si elle te plaît » sur la page (vérifier par recherche).  
2. La barre du haut affiche « Un extrait à valider » et « Satisfaite ou remboursée ».  
3. Les 3 formules affichent « Un extrait à valider avant la version finale ».  
4. La section garantie parle bien de paiement PUIS extrait PUIS remboursement.  
5. La section preuve sociale s'affiche entre les formules et la garantie : 3 chiffres (3 000+, 98 %, 24h) \+ 4 témoignages en grille 2×2 (1 colonne sur mobile).  
6. Le reste de la page est inchangé.

Puis commit et push sur main.  
