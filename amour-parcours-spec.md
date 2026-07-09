# StoryBeat — Refonte de la catégorie ❤️ Amour (parcours par lien)

**But :** la catégorie Amour ne doit plus se limiter au couple. Quand l'utilisateur choisit « ❤️ Amour », le questionnaire affiche d'abord un **sélecteur de lien** (« Cette chanson d'amour, c'est pour qui ? »), puis déroule un jeu de questions **propre au lien choisi**.

Les 6 liens : **couple, enfant, parent, fratrie, grands-parents, ami**.

⚠️ Périmètre volontaire (pour ne pas doublonner avec les autres catégories) :

- « enfant » \= un enfant **de tout âge** (la catégorie Naissance gère déjà les bébés).  
- « parent » et « grands-parents » \= personnes **vivantes** (la catégorie Hommage gère les défunts).  
- Amour n'est **pas** lié à une occasion (Anniversaire et Mariage existent déjà).

---

## PARTIE 1 — Modifications du moteur (JS)

Aujourd'hui, le moteur déroule `cat.screens[i]` de façon linéaire et lit `cat.recap`. Il n'y a aucun branchement. On ajoute la notion de **sous-parcours (« liens »)** UNIQUEMENT pour la catégorie Amour ; les 6 autres catégories ne changent pas.

### 1.1 — Nouvelle structure de données pour « amour »

La catégorie `amour` n'a plus `screens`/`recap` directement, mais un tableau `liens`, chaque lien ayant son propre `screens` \+ `recap` (voir Partie 2).

{ key:"amour", eyebrow:"❤️ Amour",

  liens:\[

    { key:"couple",  label:"💑 Mon chéri / Ma chérie", screens:\[...\], recap:\[...\] },

    { key:"enfant",  label:"👶 Mon enfant",            screens:\[...\], recap:\[...\] },

    { key:"parent",  label:"🫂 Ma mère / Mon père",     screens:\[...\], recap:\[...\] },

    { key:"fratrie", label:"👯 Mon frère / Ma sœur",    screens:\[...\], recap:\[...\] },

    { key:"gp",      label:"👵 Mes grands-parents",     screens:\[...\], recap:\[...\] },

    { key:"ami",     label:"🤝 Un(e) ami(e) proche",    screens:\[...\], recap:\[...\] }

  \]

}

### 1.2 — Une variable d'état pour le lien choisi

Ajouter une variable globale à côté de `cat`, `i`, `final` : `let lien=null;` (indice du lien choisi dans `cat.liens`, ou `null` tant qu'aucun n'est choisi).

### 1.3 — Deux helpers pour « le parcours actif »

Ajouter :

function activeScreens(){ return cat.liens ? cat.liens\[lien\].screens : cat.screens; }

function activeRecap(){   return cat.liens ? cat.liens\[lien\].recap   : cat.recap;   }

### 1.4 — Remplacer TOUTES les références `cat.screens` / `cat.recap`

Dans `render()`, `next()`, `back()`, `setProgress()`, `renderFinal()` :

- `cat.screens` → `activeScreens()`  
- `cat.recap`   → `activeRecap()` (Claude Code doit chercher toutes les occurrences et les remplacer.)

### 1.5 — Afficher le sélecteur de lien

Dans `render()`, juste après le bloc `if(!cat){...}` et avant de lire l'écran courant : si la catégorie a des liens ET qu'aucun lien n'est encore choisi, afficher le sélecteur au lieu d'un écran de questions :

if(cat.liens && lien===null){ renderLienPicker(); nav.style.display='none'; setProgress(); return; }

Nouvelle fonction `renderLienPicker()` (calquée sur `renderEntry`, mais qui liste `cat.liens` et affiche un titre « Cette chanson d'amour, c'est pour qui ? ») :

function renderLienPicker(){

  const step=document.createElement('div');step.className='step intro';

  step.innerHTML=\`

    \<div class="eyebrow"\>\<span class="dot"\>\</span\>${cat.eyebrow}\</div\>

    \<h1\>Cette chanson d'amour, c'est pour qui ?\</h1\>

    \<p\>Chaque lien a ses mots. On adapte les questions à la personne que tu veux toucher.\</p\>\`;

  const box=document.createElement('div');box.className='opts';

  cat.liens.forEach((l,idx)=\>{

    const el=document.createElement('button');el.className='opt';el.type='button';

    el.innerHTML=\`\<span\>${l.label}\</span\>\`;

    el.onclick=()=\>{ lien=idx; clearData(); i=0; final=false; render(); };

    box.appendChild(el);

  });

  step.appendChild(box);

  stage.appendChild(step); window.scrollTo({top:0,behavior:'smooth'});

}

### 1.6 — Navigation « retour »

Dans `back()` :

- si on est sur le 1er écran d'un lien (`i===0` et `lien!==null`) → revenir au sélecteur de lien : `lien=null; clearData(); i=0; final=false; render(); return;`  
- si on est sur le sélecteur de lien (déjà géré par `cat=null` plus bas) → revenir au choix de catégorie comme aujourd'hui.

### 1.7 — Réinitialiser `lien` quand on quitte la catégorie

Partout où on fait `cat=null` (retour catégorie) et dans `renderEntry` au clic catégorie, remettre `lien=null`. Dans le clic catégorie de `renderEntry` (`el.onclick=()=>{cat=c; ...}`), ajouter `lien=null;`.

### 1.8 — Barre de progression au stade « sélecteur de lien »

`setProgress()` : si `cat.liens && lien===null`, mettre une petite valeur fixe (ex. `p=4`) comme pour l'entrée.

### 1.9 — Récap e-mail : indiquer le lien choisi

Dans `submit()` (≈ ligne 574, `lines.push('Catégorie : '+...)`), ajouter juste après une ligne indiquant le lien, ex. :

if(cat.liens && lien\!==null) lines.push('Lien : '+cat.liens\[lien\].label);

### 1.10 — Compteur d'écrans

Le fil « i+1/TOTAL » dans l'eyebrow doit utiliser `activeScreens().length` (déjà couvert si on remplace bien `cat.screens` par `activeScreens()`).

---

## PARTIE 2 — Contenu des 6 parcours

Format identique à l'existant. Types de blocs : `text`, `area`, `choice`, `multi` (option `max`), `free`. Chaque parcours \= 6 écrans \+ un `recap`.

### 2.1 — LIEN « couple » (💑 Mon chéri / Ma chérie)

*(reprise du parcours couple actuel, inchangé)*

{ key:"couple", label:"💑 Mon chéri / Ma chérie",

  screens:\[

    { title:"Parle-nous de vous deux.", sub:"On plante le décor.", blocks:\[

      {type:"text", key:"qui", label:"Son prénom \+ depuis quand", ph:"Ex : Sarah, ensemble depuis 7 ans"},

      {type:"choice", key:"sexe", label:"C'est…", o:\["Un homme","Une femme"\]},

      {type:"choice", key:"etape", label:"Vous en êtes où ?", o:\["Le tout début","Quelques années","Toute une vie","Une demande spéciale","On s'est retrouvés après une épreuve"\]},

      {type:"text", key:"offrant", label:"Ton prénom (celui qui offre)", ph:"Ex : Naïm"},

      {type:"free", key:"lien\_libre", ph:"Ajouter un mot sur votre lien…"}

    \]},

    { title:"Le moment où tu t'es dit : c'est elle / c'est lui.", sub:"Le souvenir qui résume tout.", blocks:\[

      {type:"area", key:"moment", label:"Raconte-le simplement", ph:"Ex : le soir où on a parlé jusqu'à 4h sans voir le temps passer"},

      {type:"choice", key:"moment\_type", label:"Pas d'idée ? Pioche un type de moment", o:\["Notre rencontre","Un voyage","Une épreuve traversée","Un fou rire","Un matin tout simple"\]},

      {type:"free", key:"moment\_libre", ph:"Ajouter un détail qui compte…"}

    \]},

    { title:"Ce qui fait que c'est elle / lui, et personne d'autre.", sub:"L'empreinte unique de la chanson.", blocks:\[

      {type:"multi", key:"traits", label:"Coche tout ce qui lui ressemble", o:\["Drôle","Tendre","Têtu·e","Généreux·se","Lumineux·se","Protecteur·rice","Pétillant·e","Passionné·e","Maladroit·e","Calme"\]},

      {type:"text", key:"signature", label:"Sa manie, son surnom, sa phrase culte", ph:"Ex : elle vole toujours ma part de dessert"},

      {type:"free", key:"detail\_libre", ph:"Autre chose à ajouter…"}

    \]},

    { title:"En écoutant, tu veux qu'il / elle ressente quoi ?", sub:"", blocks:\[

      {type:"multi", key:"emotion", label:"L'émotion à provoquer (plusieurs possibles)", o:\["Une vague de tendresse","Des frissons","Un grand sourire","Les larmes (les belles)","La fierté d'être aimé·e"\]},

      {type:"free", key:"emotion\_libre", ph:"Préciser si tu veux…"}

    \]},

    { title:"Les messages à faire passer.", sub:"Jusqu'à 3 — un par couplet.", blocks:\[

      {type:"multi", key:"message", max:3, label:"Choisis jusqu'à 3 messages", o:\["Je t'aime comme au premier jour","Merci d'être là, toujours","Tu es mon repère","C'est toi, pour la vie","J'aime tout de toi","On a traversé tellement","Vieillir avec toi","Pardon, et continuons"\]},

      {type:"area", key:"message\_libre", label:"Dis-le avec tes mots", ph:"Ex : je n'ai jamais su te le dire en face"}

    \]},

    { title:"L'univers musical.", sub:"On finit par le son qui lui parlera dès les premières notes.", blocks:\[

      {type:"text", key:"artiste", label:"Un artiste ou une chanson que la personne adore ?", hint:"Les goûts du destinataire — pas les tiens.", ph:"Ex : Stromae, ou un truc à la Aya Nakamura"},

      {type:"choice", key:"style", label:"Pas d'artiste en tête ? Choisis un style", o:\["Pop","Variété française","Rap","R\&B / Soul","Ballade douce","Festif / Dansant"\]},

      {type:"free", key:"musique\_libre", ph:"Une 2ᵉ référence, ou une ambiance (douce, festive…)"}

    \]}

  \],

  recap:\[\["qui","Le couple"\],\["sexe","Sexe"\],\["etape","Étape"\],\["offrant","Offert par"\],\["lien\_libre","Lien (+)"\],\["moment","Le moment"\],\["moment\_type","Type de moment"\],\["moment\_libre","Détail (+)"\],\["traits","Ce qui le/la définit"\],\["signature","Détail signature"\],\["detail\_libre","En \+"\],\["emotion","Émotion"\],\["emotion\_libre","Émotion (+)"\],\["message","Messages"\],\["message\_libre","En tes mots"\],\["artiste","Artiste / chanson"\],\["style","Style"\],\["musique\_libre","Musique (+)"\]\]

}

### 2.2 — LIEN « enfant » (👶 Mon enfant)

{ key:"enfant", label:"👶 Mon enfant",

  screens:\[

    { title:"Parle-nous de ton enfant.", sub:"On plante le décor.", blocks:\[

      {type:"text", key:"qui", label:"Son prénom \+ son âge", ph:"Ex : Lina, 6 ans"},

      {type:"choice", key:"sexe", label:"C'est…", o:\["Un garçon","Une fille"\]},

      {type:"choice", key:"etape", label:"Il / elle en est où ?", o:\["Tout-petit·e","En pleine enfance","Ado","Jeune adulte","Adulte, envolé·e du nid"\]},

      {type:"text", key:"offrant", label:"Ton prénom (le parent qui offre)", ph:"Ex : Karim"},

      {type:"free", key:"lien\_libre", ph:"Ajouter un mot sur votre lien…"}

    \]},

    { title:"Le moment où ton cœur a fondu.", sub:"Le souvenir qui dit tout.", blocks:\[

      {type:"area", key:"moment", label:"Raconte-le simplement", ph:"Ex : la première fois qu'il m'a pris la main pour s'endormir"},

      {type:"choice", key:"moment\_type", label:"Pas d'idée ? Pioche un type de moment", o:\["Sa naissance / ses premiers pas","Un fou rire à deux","Un exploit dont tu es fier·e","Un câlin un soir difficile","Un matin tout simple"\]},

      {type:"free", key:"moment\_libre", ph:"Ajouter un détail qui compte…"}

    \]},

    { title:"Ce qui fait que c'est ton enfant, unique au monde.", sub:"", blocks:\[

      {type:"multi", key:"traits", label:"Coche tout ce qui lui ressemble", o:\["Curieux·se","Espiègle","Tendre","Têtu·e","Rêveur·se","Courageux·se","Rigolo·te","Sensible","Plein·e d'énergie","Doux·ce"\]},

      {type:"text", key:"signature", label:"Sa manie, son surnom, sa phrase culte", ph:"Ex : il m'appelle 'papa-lune'"},

      {type:"free", key:"detail\_libre", ph:"Autre chose à ajouter…"}

    \]},

    { title:"En écoutant, tu veux qu'il / elle ressente quoi ?", sub:"", blocks:\[

      {type:"multi", key:"emotion", label:"L'émotion à provoquer (plusieurs possibles)", o:\["Se sentir aimé·e sans condition","La fierté d'être ton enfant","Se sentir en sécurité","Un grand sourire","Garder ça toute sa vie"\]},

      {type:"free", key:"emotion\_libre", ph:"Préciser si tu veux…"}

    \]},

    { title:"Ce que tu veux lui dire.", sub:"Jusqu'à 3 — un par couplet.", blocks:\[

      {type:"multi", key:"message", max:3, label:"Choisis jusqu'à 3 messages", o:\["Tu es ce qui m'est arrivé de plus beau","Je serai toujours là pour toi","Grandis, je te tiens la main","Je suis fier·e de qui tu deviens","Tu peux tout me dire","N'aie jamais peur d'être toi","Je t'aime plus que tout"\]},

      {type:"area", key:"message\_libre", label:"Dis-le avec tes mots", ph:"Ex : le jour de ta naissance, ma vie a changé"}

    \]},

    { title:"L'univers musical.", sub:"Le son qui le / la fera sourire dès les premières notes.", blocks:\[

      {type:"text", key:"artiste", label:"Une chanson ou un artiste qu'il / elle adore ?", hint:"Les goûts de l'enfant.", ph:"Ex : une chanson de dessin animé, un artiste qu'il fredonne"},

      {type:"choice", key:"style", label:"Pas d'idée ? Choisis un style", o:\["Pop douce","Comptine moderne","Variété française","Ballade tendre","Festif / Joyeux","Rap doux / Slam"\]},

      {type:"free", key:"musique\_libre", ph:"Une 2ᵉ référence, ou une ambiance…"}

    \]}

  \],

  recap:\[\["qui","L'enfant"\],\["sexe","Sexe"\],\["etape","Âge / étape"\],\["offrant","Offert par"\],\["lien\_libre","Lien (+)"\],\["moment","Le moment"\],\["moment\_type","Type de moment"\],\["moment\_libre","Détail (+)"\],\["traits","Ce qui le/la définit"\],\["signature","Sa signature"\],\["detail\_libre","En \+"\],\["emotion","Émotion"\],\["emotion\_libre","Émotion (+)"\],\["message","Messages"\],\["message\_libre","En tes mots"\],\["artiste","Artiste / chanson"\],\["style","Style"\],\["musique\_libre","Musique (+)"\]\]

}

### 2.3 — LIEN « parent » (🫂 Ma mère / Mon père)

{ key:"parent", label:"🫂 Ma mère / Mon père",

  screens:\[

    { title:"Parle-nous de ce parent.", sub:"On plante le décor.", blocks:\[

      {type:"choice", key:"parent", label:"C'est…", o:\["Ma mère","Mon père","Celui / celle qui m'a élevé·e"\]},

      {type:"text", key:"qui", label:"Son prénom", ph:"Ex : Fatima"},

      {type:"choice", key:"etape", label:"Où en êtes-vous aujourd'hui ?", o:\["Très proches","Ça s'est construit avec le temps","On se retrouve après des années","Compliqué, mais l'amour est là"\]},

      {type:"text", key:"offrant", label:"Ton prénom", ph:"Ex : Sofia"},

      {type:"free", key:"lien\_libre", ph:"Ajouter un mot sur votre lien…"}

    \]},

    { title:"Un souvenir qui te ramène à lui / elle.", sub:"Le détail qui dit tout.", blocks:\[

      {type:"area", key:"moment", label:"Raconte-le simplement", ph:"Ex : ses mains qui sentaient la cuisine du dimanche"},

      {type:"choice", key:"moment\_type", label:"Pas d'idée ? Pioche un type de moment", o:\["Un geste du quotidien","Un sacrifice qu'il / elle a fait","Une leçon de vie","Un fou rire","Un moment de réconfort"\]},

      {type:"free", key:"moment\_libre", ph:"Ajouter un détail qui compte…"}

    \]},

    { title:"Ce qui fait que c'est lui / elle.", sub:"", blocks:\[

      {type:"multi", key:"traits", label:"Coche tout ce qui lui ressemble", o:\["Protecteur·rice","Dévoué·e","Fort·e","Généreux·se","Sage","Drôle","Discret·e","Travailleur·se","Aimant·e","Inébranlable"\]},

      {type:"text", key:"signature", label:"Son expression, son plat, son rituel", ph:"Ex : 'mange encore un peu' à chaque visite"},

      {type:"free", key:"detail\_libre", ph:"Autre chose à ajouter…"}

    \]},

    { title:"En écoutant, tu veux qu'il / elle ressente quoi ?", sub:"", blocks:\[

      {type:"multi", key:"emotion", label:"L'émotion à provoquer (plusieurs possibles)", o:\["La gratitude","Des larmes de tendresse","La fierté de l'avoir comme parent","Se sentir reconnu·e à son tour","La chaleur d'un souvenir"\]},

      {type:"free", key:"emotion\_libre", ph:"Préciser si tu veux…"}

    \]},

    { title:"Ce que tu veux lui dire.", sub:"Jusqu'à 3 — un par couplet.", blocks:\[

      {type:"multi", key:"message", max:3, label:"Choisis jusqu'à 3 messages", o:\["Merci pour tout ce que tu as fait","Je ne te le dis pas assez : je t'aime","Tu es mon repère","Tout ce que je suis, je te le dois","Je suis fier·e d'être ton enfant","Prends soin de toi maintenant"\]},

      {type:"area", key:"message\_libre", label:"Dis-le avec tes mots", ph:"Ex : je réalise aujourd'hui tout ce que tu as porté"}

    \]},

    { title:"L'univers musical.", sub:"Le son qui le / la touchera dès les premières notes.", blocks:\[

      {type:"text", key:"artiste", label:"Une chanson ou un artiste qu'il / elle aime ?", hint:"Les goûts du parent.", ph:"Ex : une chanson de sa jeunesse"},

      {type:"choice", key:"style", label:"Pas d'idée ? Choisis un style", o:\["Variété française","Chanson du pays","Ballade douce","Soul / Gospel","Classique / Piano","Festif"\]},

      {type:"free", key:"musique\_libre", ph:"Une 2ᵉ référence, ou une ambiance…"}

    \]}

  \],

  recap:\[\["parent","Le parent"\],\["qui","Son prénom"\],\["etape","Votre lien"\],\["offrant","Offert par"\],\["lien\_libre","Lien (+)"\],\["moment","Le moment"\],\["moment\_type","Type de moment"\],\["moment\_libre","Détail (+)"\],\["traits","Ce qui le/la définit"\],\["signature","Sa signature"\],\["detail\_libre","En \+"\],\["emotion","Émotion"\],\["emotion\_libre","Émotion (+)"\],\["message","Messages"\],\["message\_libre","En tes mots"\],\["artiste","Artiste / chanson"\],\["style","Style"\],\["musique\_libre","Musique (+)"\]\]

}

### 2.4 — LIEN « fratrie » (👯 Mon frère / Ma sœur)

{ key:"fratrie", label:"👯 Mon frère / Ma sœur",

  screens:\[

    { title:"Parle-nous de lui / d'elle.", sub:"On plante le décor.", blocks:\[

      {type:"text", key:"qui", label:"Son prénom \+ votre écart d'âge", ph:"Ex : Yanis, mon petit frère de 3 ans"},

      {type:"choice", key:"sexe", label:"C'est…", o:\["Mon frère","Ma sœur"\]},

      {type:"choice", key:"etape", label:"Vous êtes…", o:\["Inséparables depuis toujours","Fâchés puis retrouvés","Loin mais soudés","Aussi différents que complices"\]},

      {type:"text", key:"offrant", label:"Ton prénom", ph:"Ex : Inès"},

      {type:"free", key:"lien\_libre", ph:"Ajouter un mot sur votre lien…"}

    \]},

    { title:"Un souvenir qui résume votre complicité.", sub:"", blocks:\[

      {type:"area", key:"moment", label:"Raconte-le simplement", ph:"Ex : nos batailles d'oreillers qui finissaient en fou rire"},

      {type:"choice", key:"moment\_type", label:"Pas d'idée ? Pioche un type de moment", o:\["Une bêtise à deux","Un secret gardé","Une épreuve traversée ensemble","Une tradition rien qu'à vous","Un fou rire légendaire"\]},

      {type:"free", key:"moment\_libre", ph:"Ajouter un détail qui compte…"}

    \]},

    { title:"Ce qui fait que c'est lui / elle.", sub:"", blocks:\[

      {type:"multi", key:"traits", label:"Coche tout ce qui lui ressemble", o:\["Complice","Protecteur·rice","Casse-cou","Rigolo·te","Loyal·e","Râleur·se attachant·e","Le / la sage","Le / la rebelle","Généreux·se","Toujours là"\]},

      {type:"text", key:"signature", label:"Votre blague interne, son surnom", ph:"Ex : je l'appelle 'boss' depuis qu'on est petits"},

      {type:"free", key:"detail\_libre", ph:"Autre chose à ajouter…"}

    \]},

    { title:"En écoutant, tu veux qu'il / elle ressente quoi ?", sub:"", blocks:\[

      {type:"multi", key:"emotion", label:"L'émotion à provoquer (plusieurs possibles)", o:\["La complicité qui remonte","Un grand sourire","Des frissons","La fierté d'avoir ce lien","L'envie de le / la serrer fort"\]},

      {type:"free", key:"emotion\_libre", ph:"Préciser si tu veux…"}

    \]},

    { title:"Ce que tu veux lui dire.", sub:"Jusqu'à 3 — un par couplet.", blocks:\[

      {type:"multi", key:"message", max:3, label:"Choisis jusqu'à 3 messages", o:\["T'es plus qu'un frère / qu'une sœur","Merci d'avoir toujours été là","On a grandi ensemble, on vieillira ensemble","Je te protégerai toujours","Malgré tout, c'est toi","Je suis fier·e de toi"\]},

      {type:"area", key:"message\_libre", label:"Dis-le avec tes mots", ph:"Ex : sans toi, l'enfance n'aurait pas eu la même saveur"}

    \]},

    { title:"L'univers musical.", sub:"Le son qui lui ressemble.", blocks:\[

      {type:"text", key:"artiste", label:"Une chanson ou un artiste qu'il / elle adore ?", hint:"Ses goûts à lui / elle.", ph:"Ex : un son que vous écoutiez ensemble"},

      {type:"choice", key:"style", label:"Pas d'idée ? Choisis un style", o:\["Rap","Pop","Variété française","Festif / Dansant","R\&B / Soul","Rock"\]},

      {type:"free", key:"musique\_libre", ph:"Une 2ᵉ référence, ou une ambiance…"}

    \]}

  \],

  recap:\[\["qui","Frère / sœur"\],\["sexe","Sexe"\],\["etape","Votre lien"\],\["offrant","Offert par"\],\["lien\_libre","Lien (+)"\],\["moment","Le moment"\],\["moment\_type","Type de moment"\],\["moment\_libre","Détail (+)"\],\["traits","Ce qui le/la définit"\],\["signature","Sa signature"\],\["detail\_libre","En \+"\],\["emotion","Émotion"\],\["emotion\_libre","Émotion (+)"\],\["message","Messages"\],\["message\_libre","En tes mots"\],\["artiste","Artiste / chanson"\],\["style","Style"\],\["musique\_libre","Musique (+)"\]\]

}

### 2.5 — LIEN « gp » (👵 Mes grands-parents)

{ key:"gp", label:"👵 Mes grands-parents",

  screens:\[

    { title:"Pour qui, cette chanson ?", sub:"On plante le décor.", blocks:\[

      {type:"choice", key:"gp", label:"C'est pour…", o:\["Ma grand-mère","Mon grand-père","Mes deux grands-parents"\]},

      {type:"text", key:"qui", label:"Comment tu l'appelles / les appelles", ph:"Ex : Mamie Jo, Papi…"},

      {type:"choice", key:"etape", label:"Aujourd'hui…", o:\["On se voit souvent","On est loin mais liés","Chaque instant est précieux","Je veux graver nos souvenirs"\]},

      {type:"text", key:"offrant", label:"Ton prénom", ph:"Ex : Adam"},

      {type:"free", key:"lien\_libre", ph:"Ajouter un mot sur votre lien…"}

    \]},

    { title:"Un souvenir qui sent bon l'enfance.", sub:"", blocks:\[

      {type:"area", key:"moment", label:"Raconte-le simplement", ph:"Ex : ses histoires le soir, l'odeur de son jardin"},

      {type:"choice", key:"moment\_type", label:"Pas d'idée ? Pioche un type de moment", o:\["Un savoir-faire transmis","Les vacances chez eux","Une recette, une odeur","Une sagesse qu'il / elle t'a donnée","Un rituel du dimanche"\]},

      {type:"free", key:"moment\_libre", ph:"Ajouter un détail qui compte…"}

    \]},

    { title:"Ce qui fait que c'est lui / elle / eux.", sub:"", blocks:\[

      {type:"multi", key:"traits", label:"Coche tout ce qui lui / leur ressemble", o:\["Sage","Tendre","Généreux·se","Malicieux·se","Courageux·se","Patient·e","Bon·ne vivant·e","Doux·ce","Fort·e","Plein·e d'histoires"\]},

      {type:"text", key:"signature", label:"Son expression, son plat, sa chanson", ph:"Ex : 'de mon temps…' à chaque repas"},

      {type:"free", key:"detail\_libre", ph:"Autre chose à ajouter…"}

    \]},

    { title:"En écoutant, tu veux qu'il / elle ressente quoi ?", sub:"", blocks:\[

      {type:"multi", key:"emotion", label:"L'émotion à provoquer (plusieurs possibles)", o:\["Une tendresse profonde","La gratitude","Des larmes douces","La fierté de ses racines","Le réconfort des souvenirs"\]},

      {type:"free", key:"emotion\_libre", ph:"Préciser si tu veux…"}

    \]},

    { title:"Ce que tu veux lui / leur dire.", sub:"Jusqu'à 3 — un par couplet.", blocks:\[

      {type:"multi", key:"message", max:3, label:"Choisis jusqu'à 3 messages", o:\["Merci pour tout ce que tu m'as transmis","Tu es ma racine","Je n'oublierai jamais","Tu comptes tellement pour moi","J'aimerais arrêter le temps","Je t'aime, tout simplement"\]},

      {type:"area", key:"message\_libre", label:"Dis-le avec tes mots", ph:"Ex : tes bras ont été mon premier refuge"}

    \]},

    { title:"L'univers musical.", sub:"Le son de son époque, ou celui qui l'émeut.", blocks:\[

      {type:"text", key:"artiste", label:"Une chanson ou un artiste qu'il / elle aime ?", hint:"Ses goûts à lui / elle.", ph:"Ex : une chanson de son époque"},

      {type:"choice", key:"style", label:"Pas d'idée ? Choisis un style", o:\["Chanson d'autrefois","Variété française","Musique du pays","Classique / Piano","Accordéon / Musette","Douce ballade"\]},

      {type:"free", key:"musique\_libre", ph:"Une 2ᵉ référence, ou une ambiance…"}

    \]}

  \],

  recap:\[\["gp","Grand-parent"\],\["qui","Petit nom"\],\["etape","Votre lien"\],\["offrant","Offert par"\],\["lien\_libre","Lien (+)"\],\["moment","Le moment"\],\["moment\_type","Type de moment"\],\["moment\_libre","Détail (+)"\],\["traits","Ce qui le/la définit"\],\["signature","Sa signature"\],\["detail\_libre","En \+"\],\["emotion","Émotion"\],\["emotion\_libre","Émotion (+)"\],\["message","Messages"\],\["message\_libre","En tes mots"\],\["artiste","Artiste / chanson"\],\["style","Style"\],\["musique\_libre","Musique (+)"\]\]

}

### 2.6 — LIEN « ami » (🤝 Un(e) ami(e) proche)

{ key:"ami", label:"🤝 Un(e) ami(e) proche",

  screens:\[

    { title:"Parle-nous de lui / d'elle.", sub:"On plante le décor.", blocks:\[

      {type:"text", key:"qui", label:"Son prénom \+ depuis quand", ph:"Ex : Chloé, ma best depuis le lycée"},

      {type:"choice", key:"sexe", label:"C'est…", o:\["Un ami","Une amie"\]},

      {type:"choice", key:"etape", label:"Votre amitié, c'est…", o:\["Une amitié d'enfance","Rencontrés à l'âge adulte","Comme un frère / une sœur de cœur","La distance, mais indéfectible"\]},

      {type:"text", key:"offrant", label:"Ton prénom", ph:"Ex : Malik"},

      {type:"free", key:"lien\_libre", ph:"Ajouter un mot sur votre lien…"}

    \]},

    { title:"Un moment qui résume votre amitié.", sub:"", blocks:\[

      {type:"area", key:"moment", label:"Raconte-le simplement", ph:"Ex : la nuit où on a refait le monde jusqu'au lever du soleil"},

      {type:"choice", key:"moment\_type", label:"Pas d'idée ? Pioche un type de moment", o:\["Un fou rire mémorable","Une épreuve où il / elle était là","Une aventure, un voyage","Une bêtise de jeunesse","Un soutien sans faille"\]},

      {type:"free", key:"moment\_libre", ph:"Ajouter un détail qui compte…"}

    \]},

    { title:"Ce qui fait que c'est lui / elle.", sub:"", blocks:\[

      {type:"multi", key:"traits", label:"Coche tout ce qui lui ressemble", o:\["Fidèle","Drôle","Toujours là","Sincère","Déjanté·e","Bienveillant·e","Le / la confident·e","Pétillant·e","Solide","Généreux·se"\]},

      {type:"text", key:"signature", label:"Votre private joke, son surnom", ph:"Ex : on s'appelle 'binôme' depuis 10 ans"},

      {type:"free", key:"detail\_libre", ph:"Autre chose à ajouter…"}

    \]},

    { title:"En écoutant, tu veux qu'il / elle ressente quoi ?", sub:"", blocks:\[

      {type:"multi", key:"emotion", label:"L'émotion à provoquer (plusieurs possibles)", o:\["Un grand sourire","La gratitude d'une telle amitié","Des frissons","Un fou rire","L'envie de trinquer à vous deux"\]},

      {type:"free", key:"emotion\_libre", ph:"Préciser si tu veux…"}

    \]},

    { title:"Ce que tu veux lui dire.", sub:"Jusqu'à 3 — un par couplet.", blocks:\[

      {type:"multi", key:"message", max:3, label:"Choisis jusqu'à 3 messages", o:\["Merci d'être toujours là","Une amitié comme la nôtre, c'est rare","T'es ma famille de cœur","On sera vieux et toujours amis","Sans toi, rien n'est pareil","Je te le dis trop peu : je t'aime"\]},

      {type:"area", key:"message\_libre", label:"Dis-le avec tes mots", ph:"Ex : tu m'as relevé le jour où j'ai touché le fond"}

    \]},

    { title:"L'univers musical.", sub:"Le son qui vous ressemble.", blocks:\[

      {type:"text", key:"artiste", label:"Une chanson ou un artiste qu'il / elle adore ?", hint:"Ses goûts à lui / elle.", ph:"Ex : votre hymne de soirée"},

      {type:"choice", key:"style", label:"Pas d'idée ? Choisis un style", o:\["Pop","Rap","Variété française","Festif / Dansant","Rock","R\&B / Soul"\]},

      {type:"free", key:"musique\_libre", ph:"Une 2ᵉ référence, ou une ambiance…"}

    \]}

  \],

  recap:\[\["qui","L'ami·e"\],\["sexe","Sexe"\],\["etape","Votre amitié"\],\["offrant","Offert par"\],\["lien\_libre","Lien (+)"\],\["moment","Le moment"\],\["moment\_type","Type de moment"\],\["moment\_libre","Détail (+)"\],\["traits","Ce qui le/la définit"\],\["signature","Sa signature"\],\["detail\_libre","En \+"\],\["emotion","Émotion"\],\["emotion\_libre","Émotion (+)"\],\["message","Messages"\],\["message\_libre","En tes mots"\],\["artiste","Artiste / chanson"\],\["style","Style"\],\["musique\_libre","Musique (+)"\]\]

}

---

## PARTIE 3 — Tests à faire après implémentation

1. Cliquer « ❤️ Amour » → le sélecteur des 6 liens s'affiche (pas les questions).  
2. Choisir chaque lien → les bonnes questions s'affichent, compteur « x/6 » correct.  
3. Bouton « Retour » depuis le 1er écran d'un lien → revient au sélecteur de lien.  
4. Bouton « Retour » depuis le sélecteur de lien → revient au choix de catégorie.  
5. Aller au bout d'un lien → le récap final affiche bien les bonnes lignes.  
6. L'e-mail reçu (EmailJS) contient la ligne « Lien : … ».  
7. Vérifier que les 6 AUTRES catégories fonctionnent toujours normalement.

