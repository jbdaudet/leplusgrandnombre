---
title: "Quelques idées"
description: "Cinq cas d'application qui montrent comment la data et l'IA peuvent s'adapter à n'importe quel contexte métier."
showTableOfContents: true
layout: "applications"
---

Chaque projet ci-dessous répond à une étape clé d'un projet marketing. Ce sont des applications construites pour des secteurs très différents, qui illustrent une même capacité : comprendre un contexte, identifier le bon levier, et construire la solution adaptée.

---

<h2 id="geomarketing">Comprendre son marché <a href="https://geomarketing.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">geomarketing</a></h2>

Avant de lancer quoi que ce soit, il faut savoir à qui on s'adresse, où ils sont, et ce qui compte pour eux.

**Le contexte.** Un professionnel qui cherche le bon emplacement pour ouvrir un commerce en France. Ou un réseau qui veut comprendre le potentiel d'une zone avant d'investir.

**Le défi.** Les données existent — démographie, revenus, transports, centres d'intérêt, concurrence — mais elles sont éparpillées dans des dizaines de sources publiques et privées, chacune avec son format et sa granularité. En pratique, personne ne les croise. On se fie à l'intuition.

**Le résultat.** Entrez une adresse. En quelques secondes, vous obtenez un rapport complet : qui habite autour, quel pouvoir d'achat, quels transports, quels commerces, ce qui les intéresse, quelle concurrence — le tout comparé à la moyenne du département. La décision d'implantation devient factuelle.

### L'approche

Six sections d'analyse, chacune avec sa propre logique de données, assemblées automatiquement en un rapport cohérent.

{{< mermaid >}}
flowchart LR
    A["Adresse"] --> B["Géocodage\n+ zone de\nchalandise"]
    B --> C["Zone &\nflux piétons"]
    B --> D["Population &\nrevenus"]
    B --> E["Transports"]
    B --> F["Commerces &\néquipements"]
    B --> H["Centres\nd'intérêt"]
    B --> G["Concurrence"]
{{< /mermaid >}}

**La granularité la plus fine disponible.** Chaque donnée est exploitée à son niveau de précision maximal : population et démographie à l'**IRIS** (~2 000 habitants), revenus et pauvreté à la commune, équipements géolocalisés au point. Le rapport ne lisse pas tout à la même échelle — il tire le meilleur de chaque source.

**Zone de chalandise intelligente.** Le rayon ne se fixe pas au doigt mouillé. Le système identifie la **classification urbaine INSEE** de la commune (ville-centre, banlieue, rural) et adapte automatiquement : 750 m en centre-ville dense, jusqu'à 3 km en zone rurale. La zone croise cinq sources de données publiques (INSEE RP, Filosofi, BPE, IGN, OpenStreetMap).

**Estimation de flux piétons.** Plutôt qu'un simple comptage de population, le système modélise les **déplacements réels** domicile → commerces/transports via la méthode de centralité de Sevtsuk (2021). Le score combine le volume de flux piétons passant par le point (50%), la proximité aux destinations commerciales et de transport (30%), et la connectivité du réseau de rues (20%). Résultat : une carte avec gradient de chaleur montrant les vrais couloirs de passage.

**Benchmarking systématique.** Chaque indicateur (densité, revenus, équipements, transports) est comparé à la **moyenne du département** et, quand c'est disponible, aux **données historiques 2016** pour détecter les tendances. Un quartier qui se gentrifie ou qui perd ses commerces, ça se voit immédiatement.

**Profilage par centres d'intérêt.** La section la plus originale : le système interroge l'**API Meta (Facebook)** pour estimer la part d'utilisateurs dans la zone de chalandise qui s'intéressent à 12 catégories prédéfinies — réparties en 4 axes : Patrimoine & Statut, Valeurs & Éthique, Capital Culturel, Mode de vie & Densité. Chaque catégorie est convertie en **indice d'affinité** comparé à une référence nationale (moyenne de 7 grandes villes françaises, interrogées avec le même rayon pour éliminer les biais d'échelle). Un indice supérieur à 100 signifie que l'intérêt est sur-représenté localement. Résultat : on ne sait pas seulement *combien* de gens habitent là, mais *ce qui les intéresse*.

**Analyse concurrentielle automatique.** L'utilisateur entre un nom de marque. L'IA identifie les 3 principaux concurrents directs et le rayon de recherche adapté au type de commerce. Google Places (API v1) localise les points de vente concurrents, récupère les notes et les avis en un seul appel par concurrent, et l'IA synthétise les forces et faiblesses de chacun. Score de pression concurrentielle de 0 à 100.

{{< button href="https://geomarketing.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="signal">Trouver le bon angle <a href="https://signal.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">signal</a></h2>

Une fois qu'on connaît son marché, il faut trouver le message qui va résonner. Pas un message générique — un angle culturel, ancré dans l'actualité, qui crée une connexion authentique.

**Le contexte.** Un stratège de marque ou un créatif qui lit des dizaines d'articles par semaine en cherchant l'insight qui deviendra une campagne.

**Le défi.** Repérer la vérité universelle cachée dans un article, puis la transformer en opportunité créative pour une marque, c'est un travail d'expert qui prend des heures. Et c'est difficilement reproductible.

**Le résultat.** Collez le lien d'un article. En 30 secondes, vous obtenez une analyse culturelle, la vérité universelle cachée dans le texte, et une idée créative prête à explorer pour votre marque. Ce que fait un planneur stratégique en une demi-journée, automatisé et reproductible.

### L'approche

Le principe fondateur : **ne jamais faire confiance au premier jet de l'IA.** Chaque sortie passe par un cycle de génération multiple + évaluation automatique. Ce n'est pas du ChatGPT copié-collé — c'est un pipeline de 3 étapes où chacune a sa propre logique.

{{< mermaid >}}
flowchart LR
    A["Article\n(URL)"] --> B["Extraction\n+ résumé"]
    B --> C["Shower\nThought"]
    C --> D["Idée\ncréative"]
{{< /mermaid >}}

**Étape 1 — Extraction.** L'URL est scrapée et l'article est analysé par une IA rapide qui en extrait un résumé structuré en français. Le résumé apparaît immédiatement dans le navigateur (streaming SSE) — l'utilisateur voit le résultat se construire en temps réel pendant que les étapes suivantes travaillent en arrière-plan.

**Étape 2 — Le shower thought.** C'est ici que le système se distingue d'un simple "résume-moi cet article". L'objectif est de trouver la **vérité universelle** cachée dans le texte — le genre d'insight qu'on a sous la douche, qui fait dire "mais oui, évidemment".

Le système ne part pas de zéro. Il s'appuie sur un **index de 3 000 vérités culturelles pré-construites**, encodées en vecteurs. L'article est comparé par similarité sémantique à cet index pour trouver les shower thoughts les plus proches — ceux qui résonnent avec les thèmes de l'article. Ensuite :

1. L'IA génère **5 candidats** inspirés de l'article et des correspondances trouvées
2. Une **IA évaluatrice** note chaque candidat sur trois critères : **Simplicité** (est-ce limpide ?), **Surprise** (est-ce inattendu ?), **Profondeur** (est-ce que ça touche à quelque chose de fondamental ?)
3. Seul le **meilleur** est retenu

Ce pattern **"générer puis juger"** est ce qui fait la différence entre un premier jet médiocre et un insight qui claque.

**Étape 3 — L'idée créative.** Le shower thought est transformé en opportunité pour la marque. Là encore, pas de tirage au sort :

1. L'IA sélectionne **3 prismes créatifs** parmi une bibliothèque de **20 frameworks** (détournement, analogie, provocation, confession, inversion, rituel...) — les mêmes outils qu'utilise un planneur stratégique en agence
2. Elle génère **une idée "Et si [marque]..."** par prisme
3. Chaque idée est évaluée sur **4 critères** : justesse par rapport à la vérité culturelle, pertinence stratégique pour la marque, potentiel génératif (est-ce que ça ouvre des pistes ?), et force de provocation
4. Seule la **meilleure** est présentée

**Le contexte de marque est injectable.** L'utilisateur peut uploader des documents (PDF, Word, texte) qui décrivent sa marque. Ces documents sont découpés, encodés en vecteurs **en mémoire** (rien n'est stocké), et consultés par similarité sémantique à chaque étape de génération. Le shower thought et l'idée créative sont ainsi ancrés dans le positionnement réel de la marque — pas dans une hallucination. Si aucun document n'est fourni, l'IA génère automatiquement un territoire de marque à partir du nom seul.

{{< button href="https://signal.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="coaching">Créer du contenu qui me ressemble <a href="https://coaching.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">coaching</a></h2>

Produire du contenu à grande échelle, c'est facile. Produire du contenu qui sonne comme soi — avec ses convictions, son vocabulaire, ses tics de langage — c'est un autre problème.

**Le contexte.** Un coach avec 20 ans de pratique, 918 documents publiés, une voix unique. Il veut démultiplier son impact sans diluer ce qui fait sa singularité.

**Le défi.** Un chatbot classique retrouve les passages pertinents dans un corpus et les restitue. Ça marche quand le sujet a été traité. Mais la valeur d'un expert, c'est sa perspective — sa capacité à avoir un avis sur des sujets qu'il n'a jamais explicitement abordés. Là, le chatbot classique se tait.

**Le résultat.** Un double numérique du coach qui répond comme lui — y compris sur des sujets qu'il n'a jamais directement traités. Et un outil qui restructure ses articles avec l'efficacité narrative des meilleurs TED talks, sans perdre sa voix.

### L'approche

Deux moteurs distincts — un chatbot qui pense comme le coach, et un outil de réécriture qui écrit comme les meilleurs TED talks.

#### Le chatbot : au-delà du RAG classique

Un chatbot classique (RAG) fonctionne ainsi : l'utilisateur pose une question → on cherche les passages similaires dans le corpus → on les envoie à l'IA pour formuler une réponse. **Le problème** : si le coach n'a jamais écrit sur le sujet, il n'y a rien à retrouver.

Notre approche ajoute une **couche structurelle** entre le corpus brut et le chatbot :

{{< mermaid >}}
flowchart TB
    Q["Question\nutilisateur"] --> H["HyDE\n(réponse hypothétique\ndans la voix du coach)"]
    H --> T["Détection\ndes thèmes"]
    H --> C["Détection des\nconvictions"]
    C --> E["Expansion\n(hiérarchie +\nco-occurrences)"]
    T --> E
    E --> R["Récupération\n4 niveaux"]
    R --> G["Réponse\nstreaming"]
{{< /mermaid >}}

**79 convictions extraites automatiquement.** Le corpus (918 documents, 792 000 mots) est analysé par clustering non supervisé (BERTopic). L'algorithme découvre 79 "convictions" — les croyances profondes qui reviennent dans l'écriture du coach, même quand il parle de sujets différents. Ces convictions sont organisées en hiérarchie (79 feuilles, 59 nœuds intermédiaires) et reliées par un graphe de co-occurrence : quand le coach parle de X, il amène naturellement Y.

**HyDE — traduire la langue de l'utilisateur en langue du coach.** L'utilisateur demande "comment gérer un collègue difficile ?", mais la conviction pertinente du coach parle de "colère réprimée". Une IA rapide génère d'abord une **réponse hypothétique** dans la voix du coach, puis c'est *cette* réponse qui est comparée aux convictions — pas la question brute. Ça comble le fossé linguistique.

**Récupération en 4 niveaux.** Le système cherche d'abord les documents qui correspondent à la fois à la conviction ET au thème détectés (filtre le plus strict). Si pas assez de résultats, il élargit : conviction seule → thème seul → tout le corpus. Chaque niveau est un filet de sécurité — on trouve toujours quelque chose de pertinent.

**La voix, pas juste les idées.** Le prompt du chatbot inclut 6 dimensions de style extraites du corpus : échantillons d'écriture réels, lexique de prédilection, **anti-lexique** (les mots que le coach n'utilise *jamais* — "résilience", "bienveillance", "soft skills"), profil stylistique, biographie, et cadres théoriques qu'il utilise. Le résultat ne sonne pas comme une IA — il sonne comme lui.

#### La réécriture TED

L'autre moteur restructure les articles du coach en utilisant les **patterns narratifs des meilleurs TED talks**.

{{< mermaid >}}
flowchart TB
  subgraph row1 [" "]
    direction LR
    A["Article\ndu coach"] --> B["Analyse\nstructurelle"] --> C["Comparaison\nvs 267 TED talks"]
  end
  subgraph row2 [" "]
    direction RL
    F["Simulation\nlecteur"] --> E["Réécriture\nsection par section"] --> D["Arc narratif\nsuggéré"]
  end
  C --> D
{{< /mermaid >}}

**267 TED talks analysés.** Chaque talk est découpé en sections étiquetées (ouverture, histoire, concept, exemple, application, conclusion) avec leur *purpose* (créer de la curiosité, ancrer en mémoire, reframer...). Un **graphe de transitions** pondéré capture les enchaînements : après une "histoire", un talk qui "enseigne" enchaîne sur un "concept" 40% du temps, mais un talk qui "challenge" passe à une "objection" 25% du temps.

**L'arc narratif est généré algorithmiquement.** Le système marche le graphe de transitions en respectant 3 contraintes : les probabilités spécifiques à l'intention du texte, des budgets par section (pas plus de 30% de "concept" comme dans les TED), et un bonus pour les sections que l'article contient déjà (on réutilise au lieu d'inventer).

**Réécriture en 6 phases.** Chaque section est réécrite avec des **extraits réels de TED talks** comme démonstrations de technique rhétorique. Phase 4 est critique : les transitions entre sections — les phrases de liaison qui transforment des blocs juxtaposés en un texte fluide, construites à partir d'un index de 397 transitions réelles de TED talks.

**Simulation lecteur + optimisation.** Une IA lit l'article fini comme un lecteur naïf et note chaque section (engagement élevé / moyen / faible). Les sections faibles sont régénérées avec le feedback spécifique du lecteur simulé. Les sections fortes ne sont pas touchées.

**Le coach garde sa voix.** Les mêmes contraintes de style s'appliquent : anti-lexique, pas de buzzwords, contenu inventé signalé par des balises visuelles. L'article est restructuré, pas réécrit — les idées restent celles du coach.

{{< button href="https://coaching.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="artisans-ai">Gagner du temps au quotidien <a href="https://artisans.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">artisans.ai</a></h2>

Automatiser les tâches chronophages pour se concentrer sur ce qu'on sait faire — sans sacrifier la qualité.

**Le contexte.** Un artisan du bâtiment — menuisier, plombier, électricien — qui passe ses soirées à rédiger des devis, gérer son stock et planifier ses chantiers au lieu d'exercer son métier.

**Le défi.** Les outils de gestion existants sont soit trop génériques (ils ne comprennent pas le métier), soit trop lourds (pensés pour des entreprises de 50 personnes). L'artisan finit par tout faire à la main, sur Excel ou sur papier.

**Le résultat.** L'artisan décrit son chantier en quelques phrases. L'outil génère les étapes de fabrication, identifie les matériaux nécessaires dans son stock, et produit un devis professionnel conforme aux normes françaises. Ce qui prenait une soirée prend 10 minutes.

### L'approche

Le principe : **chaque fonctionnalité alimente la suivante.** Le projet génère la gamme, la gamme génère le devis, le devis ajuste le stock. Rien n'est saisi deux fois.

{{< mermaid >}}
flowchart LR
    A["Description\ndu chantier"] --> B["Conversation\nIA guidée"]
    B --> C["Gamme\nopératoire"]
    C --> D["Devis\nprofessionnel"]
    C --> E["Stock\najusté"]
    F["Feuilles\nde débit"] --> E
{{< /mermaid >}}

**17 métiers, 17 vocabulaires.** L'IA ne parle pas "en général" — elle connaît les spécificités de chaque profession. Un menuisier et un plombier ne décrivent pas un chantier de la même façon, n'utilisent pas les mêmes matériaux, ne structurent pas leurs devis pareil. Le système s'adapte dès la sélection du métier.

**Conversation de cadrage intelligente.** L'IA guide l'artisan sur 4 axes — portée, spécifications techniques, matériaux, installation — mais elle **saute les questions auxquelles le descriptif initial a déjà répondu**. Si l'artisan écrit "pose de parquet chêne massif dans un salon de 25 m²", l'IA ne redemande ni le matériau, ni la surface. Elle se concentre sur ce qui manque : le type de pose, la préparation du sol, les finitions.

**La gamme structure le devis.** Chaque étape générée est catégorisée automatiquement — conception, fabrication en atelier, installation sur site. Ces catégories deviennent les sections du devis, sans retraitement manuel. Durées, outils et consommables sont croisés avec le taux horaire de l'artisan pour calculer les coûts.

**Optimisation des chutes par bin-packing.** Les feuilles de débit (PDF, image, Excel) sont parsées par IA qui extrait les pièces avec leurs dimensions et essences. L'algorithme **first-fit decreasing** confronte ces pièces au stock de chutes existant et optimise la découpe : quelles pièces sont couvertes par le stock, lesquelles nécessitent un achat, et combien de chutes resteront après.

**Génération de stock en 6 étapes.** Générer un inventaire complet en un seul appel IA échoue — la sortie est trop longue et se tronque. Le pipeline découpe le problème : d'abord lister les outils → les catégoriser → les enrichir avec des specs, puis idem pour les consommables. Chaque étape est un appel IA séparé, court, vérifiable. L'utilisateur voit la progression en temps réel via streaming (SSE).

**Conformité française.** Les devis respectent les exigences légales : numéro séquentiel, identification du professionnel (adresse, SIREN), TVA à 20%, acompte configurable, espace de signature "Bon pour accord", export PDF propre.

{{< button href="https://artisans.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="theshiftproject">Le KPI utile, votre vision en plus <a href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">theshiftproject</a></h2>

Personne ne lit les dashboards. Trop de chiffres, trop de graphiques, trop peu de sens. Ici, vous n'attendez plus qu'un expert soit libre pour traduire vos données : posez votre question, obtenez la décision.

**Le contexte.** La donnée est là, mais elle est muette. Analyste, décideur ou expert métier : vous saturez d'informations. Qu'elles soient publiques (Open Data) ou internes (CRM, ERP), vos données dorment par millions de lignes dans des bases techniques inaccessibles. Faute de pouvoir interroger la réalité du terrain en un clic, vous finissez par décider à l'instinct.

**Le défi.** Les chiffres ne parlent pas seuls. Le problème n'est pas l'accès à la donnée, mais son interprétation. Un dashboard classique se contente d'afficher le chiffre. Un expert, lui, lui donne du sens.

**Le résultat.** Interrogez, décidez. Posez une question, vous obtiendrez, en plus des chiffres, une analyse incarnée. Ce n'est pas un résumé neutre : c'est une lecture signée par une persona qui a un point de vue, fait des analogies et vous propose les prochaines étapes.

### L'approche

<svg viewBox="0 0 900 620" xmlns="http://www.w3.org/2000/svg" style="width:100%;max-width:900px;margin:2rem auto;display:block;font-family:'Raleway','Segoe UI',sans-serif">
  <style>
    .tip .tip-box { opacity: 0; pointer-events: none; transition: opacity 0.25s ease; }
    .tip:hover .tip-box { opacity: 1; }
    .tip:hover .tip-target { stroke-width: 2; }
    .tip { cursor: default; }
  </style>
  <defs>
    <marker id="arrow" viewBox="0 0 10 7" refX="10" refY="3.5" markerWidth="8" markerHeight="6" orient="auto-start-reverse"><path d="M0 0 L10 3.5 L0 7z" fill="#9c9ca6"/></marker>
    <marker id="arrow-accent" viewBox="0 0 10 7" refX="10" refY="3.5" markerWidth="8" markerHeight="6" orient="auto-start-reverse"><path d="M0 0 L10 3.5 L0 7z" fill="#FF6B35"/></marker>
    <symbol id="agent" viewBox="0 0 20 20">
      <circle cx="10" cy="10" r="4" fill="none" stroke="#FF6B35" stroke-width="1.5"/>
      <circle cx="10" cy="10" r="1.5" fill="#FF6B35"/>
      <line x1="10" y1="2" x2="10" y2="6" stroke="#FF6B35" stroke-width="1.2" stroke-linecap="round"/>
      <line x1="3.2" y1="14" x2="6.6" y2="12" stroke="#FF6B35" stroke-width="1.2" stroke-linecap="round"/>
      <line x1="16.8" y1="14" x2="13.4" y2="12" stroke="#FF6B35" stroke-width="1.2" stroke-linecap="round"/>
      <circle cx="10" cy="1.5" r="1.5" fill="#FF6B35"/>
      <circle cx="2.5" cy="15" r="1.5" fill="#FF6B35"/>
      <circle cx="17.5" cy="15" r="1.5" fill="#FF6B35"/>
    </symbol>
    <filter id="shadow" x="-4%" y="-4%" width="108%" height="116%">
      <feDropShadow dx="0" dy="2" stdDeviation="4" flood-opacity="0.12"/>
    </filter>
  </defs>

  <!-- ── Question ── -->
  <rect x="350" y="8" width="200" height="44" rx="22" fill="#fff" stroke="#e0e0e4"/>
  <text x="450" y="35" text-anchor="middle" font-size="13" fill="#1a1a1a" font-weight="600">Question utilisateur</text>

  <line x1="450" y1="52" x2="450" y2="82" stroke="#9c9ca6" marker-end="url(#arrow)"/>

  <!-- ── Classifieur ── -->
  <g class="tip">
    <rect class="tip-target" x="355" y="82" width="190" height="50" rx="10" fill="#FFF0E9" stroke="#FF6B35"/>
    <use href="#agent" x="370" y="95" width="22" height="22"/>
    <text x="400" y="103" font-size="12" fill="#1a1a1a" font-weight="600">Classifieur</text>
    <text x="400" y="119" font-size="10" fill="#5c5c66">Quel type de question ?</text>
    <g class="tip-box">
      <rect x="560" y="62" width="280" height="72" rx="8" fill="#fff" stroke="#e0e0e4" filter="url(#shadow)"/>
      <text x="576" y="82" font-size="11" fill="#1a1a1a" font-weight="600">Classifieur</text>
      <text x="576" y="98" font-size="10" fill="#5c5c66">Lit la question et la classe : chiffre factuel,</text>
      <text x="576" y="112" font-size="10" fill="#5c5c66">question complexe, opinion, ou métadonnée.</text>
      <text x="576" y="126" font-size="10" fill="#5c5c66">Chaque type active un agent spécialisé.</text>
    </g>
  </g>

  <!-- 3 arrows from classifieur to agents -->
  <line x1="395" y1="132" x2="155" y2="182" stroke="#9c9ca6" marker-end="url(#arrow)"/>
  <line x1="450" y1="132" x2="450" y2="182" stroke="#9c9ca6" marker-end="url(#arrow)"/>
  <line x1="505" y1="132" x2="745" y2="182" stroke="#9c9ca6" marker-end="url(#arrow)"/>

  <text x="255" y="155" text-anchor="middle" font-size="10" fill="#5c5c66" font-style="italic">un chiffre</text>
  <text x="450" y="168" text-anchor="middle" font-size="10" fill="#5c5c66" font-style="italic">question complexe</text>
  <text x="650" y="155" text-anchor="middle" font-size="10" fill="#5c5c66" font-style="italic">une opinion</text>

  <!-- ── Agent Tactique ── -->
  <g class="tip">
    <rect class="tip-target" x="50" y="182" width="210" height="70" rx="10" fill="#fff" stroke="#e0e0e4"/>
    <use href="#agent" x="62" y="196" width="22" height="22"/>
    <text x="92" y="207" font-size="12" fill="#1a1a1a" font-weight="600">Agent tactique</text>
    <text x="92" y="223" font-size="10" fill="#5c5c66">Identifie la métrique</text>
    <text x="92" y="237" font-size="10" fill="#5c5c66">→ SQL déterministe → graphique</text>
    <g class="tip-box">
      <rect x="50" y="258" width="280" height="86" rx="8" fill="#fff" stroke="#e0e0e4" filter="url(#shadow)"/>
      <text x="66" y="278" font-size="11" fill="#1a1a1a" font-weight="600">Agent tactique</text>
      <text x="66" y="294" font-size="10" fill="#5c5c66">« Consommation d'énergie de la France ? »</text>
      <text x="66" y="308" font-size="10" fill="#5c5c66">Identifie la bonne métrique dans le catalogue,</text>
      <text x="66" y="322" font-size="10" fill="#5c5c66">assemble le SQL, choisit le graphique adapté.</text>
      <text x="66" y="336" font-size="10" fill="#FF6B35">Aucun SQL n'est généré par l'IA.</text>
    </g>
  </g>

  <!-- ── Agent Stratégique ── -->
  <g class="tip">
    <rect class="tip-target" x="310" y="182" width="280" height="70" rx="10" fill="#fff" stroke="#e0e0e4"/>
    <use href="#agent" x="322" y="196" width="22" height="22"/>
    <text x="352" y="207" font-size="12" fill="#1a1a1a" font-weight="600">Agent stratégique</text>
    <text x="352" y="223" font-size="10" fill="#5c5c66">Décompose en sous-questions</text>
    <text x="352" y="237" font-size="10" fill="#5c5c66">→ données de chacune → synthèse</text>
    <g class="tip-box">
      <rect x="310" y="258" width="290" height="86" rx="8" fill="#fff" stroke="#e0e0e4" filter="url(#shadow)"/>
      <text x="326" y="278" font-size="11" fill="#1a1a1a" font-weight="600">Agent stratégique</text>
      <text x="326" y="294" font-size="10" fill="#5c5c66">« Pourquoi les émissions baissent en Europe ? »</text>
      <text x="326" y="308" font-size="10" fill="#5c5c66">Décompose en sous-questions, récupère les</text>
      <text x="326" y="322" font-size="10" fill="#5c5c66">données de chacune, puis synthétise une</text>
      <text x="326" y="336" font-size="10" fill="#5c5c66">réponse structurée multi-étapes.</text>
    </g>
  </g>

  <!-- ── Agent Corpus ── -->
  <g class="tip">
    <rect class="tip-target" x="640" y="182" width="210" height="70" rx="10" fill="#fff" stroke="#e0e0e4"/>
    <use href="#agent" x="652" y="196" width="22" height="22"/>
    <text x="682" y="207" font-size="12" fill="#1a1a1a" font-weight="600">Agent expert</text>
    <text x="682" y="223" font-size="10" fill="#5c5c66">Cherche dans les convictions</text>
    <text x="682" y="237" font-size="10" fill="#5c5c66">et le corpus de l'expert</text>
    <g class="tip-box">
      <rect x="580" y="258" width="280" height="86" rx="8" fill="#fff" stroke="#e0e0e4" filter="url(#shadow)"/>
      <text x="596" y="278" font-size="11" fill="#1a1a1a" font-weight="600">Agent expert</text>
      <text x="596" y="294" font-size="10" fill="#5c5c66">« Que pense Jancovici du nucléaire ? »</text>
      <text x="596" y="308" font-size="10" fill="#5c5c66">Pas de données chiffrées : cherche par</text>
      <text x="596" y="322" font-size="10" fill="#5c5c66">similarité sémantique dans les convictions</text>
      <text x="596" y="336" font-size="10" fill="#5c5c66">et le corpus, puis répond dans sa voix.</text>
    </g>
  </g>

  <!-- ── Couche sémantique ── -->
  <rect x="50" y="296" width="500" height="80" rx="12" fill="#fafafa" stroke="#e0e0e4" stroke-dasharray="6 3"/>
  <text x="70" y="318" font-size="11" fill="#9c9ca6" font-weight="600" letter-spacing="1">COUCHE SÉMANTIQUE</text>
  <text x="70" y="336" font-size="11" fill="#5c5c66">Catalogue YAML · métriques, formules, filtres valides</text>
  <text x="70" y="352" font-size="11" fill="#5c5c66">→ SQL assemblé mécaniquement (pas généré par l'IA)</text>
  <text x="70" y="366" font-size="10" fill="#FF6B35" font-weight="600">Zéro hallucination</text>

  <line x1="155" y1="252" x2="155" y2="296" stroke="#9c9ca6" marker-end="url(#arrow)"/>
  <line x1="450" y1="252" x2="450" y2="296" stroke="#9c9ca6" marker-end="url(#arrow)"/>

  <!-- ── Persona ── -->
  <rect x="600" y="296" width="260" height="80" rx="12" fill="#FFF0E9" stroke="#FF6B35" stroke-dasharray="6 3"/>
  <text x="620" y="318" font-size="11" fill="#FF6B35" font-weight="600" letter-spacing="1">PERSONA</text>
  <text x="620" y="336" font-size="11" fill="#5c5c66">34 convictions extraites</text>
  <text x="620" y="352" font-size="11" fill="#5c5c66">de 10 interviews (clustering)</text>
  <text x="620" y="366" font-size="11" fill="#5c5c66">Lexique · anti-lexique · style</text>

  <line x1="745" y1="252" x2="730" y2="296" stroke="#FF6B35" marker-end="url(#arrow-accent)"/>

  <!-- ── Visualisation ── -->
  <rect x="170" y="416" width="200" height="44" rx="10" fill="#fff" stroke="#e0e0e4"/>
  <text x="270" y="443" text-anchor="middle" font-size="12" fill="#1a1a1a" font-weight="600">Visualisation auto</text>

  <line x1="300" y1="376" x2="270" y2="416" stroke="#9c9ca6" marker-end="url(#arrow)"/>

  <!-- ── Analyse incarnée ── -->
  <g class="tip">
    <rect class="tip-target" x="440" y="416" width="260" height="50" rx="10" fill="#FFF0E9" stroke="#FF6B35"/>
    <use href="#agent" x="452" y="425" width="22" height="22"/>
    <text x="482" y="439" font-size="12" fill="#1a1a1a" font-weight="600">Analyse incarnée</text>
    <text x="482" y="455" font-size="10" fill="#5c5c66">Chiffres lus avec un point de vue</text>
    <g class="tip-box">
      <rect x="440" y="360" width="280" height="52" rx="8" fill="#fff" stroke="#e0e0e4" filter="url(#shadow)"/>
      <text x="456" y="380" font-size="11" fill="#1a1a1a" font-weight="600">Analyse incarnée</text>
      <text x="456" y="396" font-size="10" fill="#5c5c66">Commente les chiffres avec le style, les convictions</text>
      <text x="456" y="408" font-size="10" fill="#5c5c66">et les analogies de la persona. Propose les prochaines étapes.</text>
    </g>
  </g>

  <line x1="370" y1="438" x2="440" y2="438" stroke="#9c9ca6" marker-end="url(#arrow)"/>
  <line x1="730" y1="376" x2="630" y2="416" stroke="#FF6B35" marker-end="url(#arrow-accent)"/>
  <path d="M 850 217 Q 880 440, 700 440" fill="none" stroke="#FF6B35" stroke-dasharray="4 3" marker-end="url(#arrow-accent)"/>

  <!-- ── Réponse ── -->
  <rect x="440" y="500" width="260" height="44" rx="22" fill="#FF6B35" stroke="none"/>
  <text x="570" y="527" text-anchor="middle" font-size="13" fill="#fff" font-weight="600">Réponse + graphique + analyse</text>

  <line x1="570" y1="460" x2="570" y2="500" stroke="#FF6B35" marker-end="url(#arrow-accent)"/>

  <!-- ── Légende ── -->
  <use href="#agent" x="55" y="571" width="16" height="16"/>
  <text x="78" y="584" font-size="10" fill="#5c5c66">Agent IA</text>
  <text x="78" y="596" font-size="9" fill="#9c9ca6" font-style="italic">survolez pour détails</text>
  <line x1="200" y1="580" x2="230" y2="580" stroke="#9c9ca6" marker-end="url(#arrow)"/>
  <text x="240" y="584" font-size="10" fill="#9c9ca6">Flux de données</text>
  <line x1="380" y1="580" x2="410" y2="580" stroke="#FF6B35" marker-end="url(#arrow-accent)"/>
  <text x="420" y="584" font-size="10" fill="#FF6B35">Persona injectée</text>
  <rect x="560" y="573" width="14" height="14" rx="3" fill="#FFF0E9" stroke="#FF6B35"/>
  <text x="582" y="584" font-size="10" fill="#5c5c66">Couche persona</text>
  <rect x="710" y="573" width="14" height="14" rx="3" fill="#fafafa" stroke="#e0e0e4" stroke-dasharray="4 2"/>
  <text x="732" y="584" font-size="10" fill="#5c5c66">Anti-hallucination</text>
</svg>

{{< button href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}
