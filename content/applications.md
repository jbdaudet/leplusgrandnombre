---
title: "Quelques idées"
description: "Cinq preuves de concept qui montrent comment la data et l'IA peuvent s'adapter à n'importe quel contexte métier."
showTableOfContents: true
---

Chaque projet ci-dessous répond à une étape clé d'un projet marketing. Ce sont des démonstrateurs — construits pour des secteurs très différents — qui illustrent une même capacité : comprendre un contexte, identifier le bon levier, et construire la solution adaptée.

---

<h2 id="geomarketing">Comprendre son marché <a href="https://geomarketing.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">geomarketing</a></h2>

Avant de lancer quoi que ce soit, il faut savoir à qui on s'adresse, où ils sont, et ce qui compte pour eux.

**Le contexte.** Un professionnel qui cherche le bon emplacement pour ouvrir un commerce en France. Ou un réseau qui veut comprendre le potentiel d'une zone avant d'investir.

**Le défi.** Les données existent — démographie, revenus, transports, concurrence — mais elles sont éparpillées dans des dizaines de sources publiques, chacune avec son format et sa granularité. En pratique, personne ne les croise. On se fie à l'intuition.

**Le résultat.** Entrez une adresse. En quelques secondes, vous obtenez un rapport complet : qui habite autour, quel pouvoir d'achat, quels transports, quels commerces, quelle concurrence — le tout comparé à la moyenne du département. La décision d'implantation devient factuelle.

### L'approche

Cinq sections d'analyse, chacune avec sa propre logique de données, assemblées automatiquement en un rapport cohérent.

{{< mermaid >}}
flowchart LR
    A["Adresse"] --> B["Géocodage\n+ zone de\nchalandise"]
    B --> C["Zone &\nflux piétons"]
    B --> D["Population &\nrevenus"]
    B --> E["Transports"]
    B --> F["Commerces &\néquipements"]
    B --> G["Concurrence"]

    style A fill:#FFF0E9,stroke:#FF6B35
    style C fill:#ffffff,stroke:#d2d2d7
    style D fill:#ffffff,stroke:#d2d2d7
    style E fill:#ffffff,stroke:#d2d2d7
    style F fill:#ffffff,stroke:#d2d2d7
    style G fill:#ffffff,stroke:#d2d2d7
{{< /mermaid >}}

**La granularité la plus fine disponible.** Chaque donnée est exploitée à son niveau de précision maximal : population et démographie à l'**IRIS** (~2 000 habitants), revenus et pauvreté à la commune, équipements géolocalisés au point. Le rapport ne lisse pas tout à la même échelle — il tire le meilleur de chaque source.

**Zone de chalandise intelligente.** Le rayon ne se fixe pas au doigt mouillé. Le système identifie la **classification urbaine INSEE** de la commune (ville-centre, banlieue, rural) et adapte automatiquement : 750 m en centre-ville dense, jusqu'à 3 km en zone rurale. La zone croise cinq sources de données publiques (INSEE RP, Filosofi, BPE, IGN, OpenStreetMap).

**Estimation de flux piétons.** Plutôt qu'un simple comptage de population, le système modélise les **déplacements réels** domicile → commerces/transports via la méthode de centralité de Sevtsuk (2021). Le score combine le volume de flux piétons passant par le point (50%), la proximité aux destinations commerciales et de transport (30%), et la connectivité du réseau de rues (20%). Résultat : une carte avec gradient de chaleur montrant les vrais couloirs de passage.

**Benchmarking systématique.** Chaque indicateur (densité, revenus, équipements, transports) est comparé à la **moyenne du département** et, quand c'est disponible, aux **données historiques 2016** pour détecter les tendances. Un quartier qui se gentrifie ou qui perd ses commerces, ça se voit immédiatement.

**Analyse concurrentielle automatique.** L'utilisateur entre un nom de marque. L'IA identifie les 3 principaux concurrents directs et le rayon de recherche adapté au type de commerce. Google Places localise les points de vente concurrents, récupère les notes et les avis, et l'IA synthétise les forces et faiblesses de chaque concurrent. Score de pression concurrentielle de 0 à 100.

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

    style A fill:#FFF0E9,stroke:#FF6B35
    style B fill:#ffffff,stroke:#d2d2d7
    style C fill:#ffffff,stroke:#d2d2d7
    style D fill:#FFF0E9,stroke:#FF6B35
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

    style Q fill:#FFF0E9,stroke:#FF6B35
    style G fill:#FFF0E9,stroke:#FF6B35
{{< /mermaid >}}

**79 convictions extraites automatiquement.** Le corpus (918 documents, 792 000 mots) est analysé par clustering non supervisé (BERTopic). L'algorithme découvre 79 "convictions" — les croyances profondes qui reviennent dans l'écriture du coach, même quand il parle de sujets différents. Ces convictions sont organisées en hiérarchie (79 feuilles, 59 nœuds intermédiaires) et reliées par un graphe de co-occurrence : quand le coach parle de X, il amène naturellement Y.

**HyDE — traduire la langue de l'utilisateur en langue du coach.** L'utilisateur demande "comment gérer un collègue difficile ?", mais la conviction pertinente du coach parle de "colère réprimée". Une IA rapide génère d'abord une **réponse hypothétique** dans la voix du coach, puis c'est *cette* réponse qui est comparée aux convictions — pas la question brute. Ça comble le fossé linguistique.

**Récupération en 4 niveaux.** Le système cherche d'abord les documents qui correspondent à la fois à la conviction ET au thème détectés (filtre le plus strict). Si pas assez de résultats, il élargit : conviction seule → thème seul → tout le corpus. Chaque niveau est un filet de sécurité — on trouve toujours quelque chose de pertinent.

**La voix, pas juste les idées.** Le prompt du chatbot inclut 6 dimensions de style extraites du corpus : échantillons d'écriture réels, lexique de prédilection, **anti-lexique** (les mots que le coach n'utilise *jamais* — "résilience", "bienveillance", "soft skills"), profil stylistique, biographie, et cadres théoriques qu'il utilise. Le résultat ne sonne pas comme une IA — il sonne comme lui.

#### La réécriture TED

L'autre moteur restructure les articles du coach en utilisant les **patterns narratifs des meilleurs TED talks**.

{{< mermaid >}}
flowchart LR
    A["Article\ndu coach"] --> B["Analyse\nstructurelle"]
    B --> C["Comparaison\nvs 267 TED talks"]
    C --> D["Arc narratif\nsuggéré"]
    D --> E["Réécriture\nsection par section"]
    E --> F["Simulation\nlecteur"]

    style A fill:#FFF0E9,stroke:#FF6B35
    style F fill:#FFF0E9,stroke:#FF6B35
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

    style A fill:#FFF0E9,stroke:#FF6B35
    style D fill:#FFF0E9,stroke:#FF6B35
{{< /mermaid >}}

**17 métiers, 17 vocabulaires.** L'IA ne parle pas "en général" — elle connaît les spécificités de chaque profession. Un menuisier et un plombier ne décrivent pas un chantier de la même façon, n'utilisent pas les mêmes matériaux, ne structurent pas leurs devis pareil. Le système s'adapte dès la sélection du métier.

**Conversation de cadrage intelligente.** L'IA guide l'artisan sur 4 axes — portée, spécifications techniques, matériaux, installation — mais elle **saute les questions auxquelles le descriptif initial a déjà répondu**. Si l'artisan écrit "pose de parquet chêne massif dans un salon de 25 m²", l'IA ne redemande ni le matériau, ni la surface. Elle se concentre sur ce qui manque : le type de pose, la préparation du sol, les finitions.

**La gamme structure le devis.** Chaque étape générée est catégorisée automatiquement — conception, fabrication en atelier, installation sur site. Ces catégories deviennent les sections du devis, sans retraitement manuel. Durées, outils et consommables sont croisés avec le taux horaire de l'artisan pour calculer les coûts.

**Optimisation des chutes par bin-packing.** Les feuilles de débit (PDF, image, Excel) sont parsées par IA qui extrait les pièces avec leurs dimensions et essences. L'algorithme **first-fit decreasing** confronte ces pièces au stock de chutes existant et optimise la découpe : quelles pièces sont couvertes par le stock, lesquelles nécessitent un achat, et combien de chutes resteront après.

**Génération de stock en 6 étapes.** Générer un inventaire complet en un seul appel IA échoue — la sortie est trop longue et se tronque. Le pipeline découpe le problème : d'abord lister les outils → les catégoriser → les enrichir avec des specs, puis idem pour les consommables. Chaque étape est un appel IA séparé, court, vérifiable. L'utilisateur voit la progression en temps réel via streaming (SSE).

**Conformité française.** Les devis respectent les exigences légales : numéro séquentiel, identification du professionnel (adresse, SIREN), TVA à 20%, acompte configurable, espace de signature "Bon pour accord", export PDF propre.

{{< button href="https://artisans.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="theshiftproject">Piloter par les résultats <a href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">theshiftproject</a></h2>

Quand les données sont là, encore faut-il pouvoir les interroger, les comprendre et en tirer des décisions — sans être data analyst.

**Le contexte.** N'importe qui s'intéressant aux questions d'énergie et de climat : un analyste, un journaliste, un étudiant, un décideur. Les données mondiales existent, mais elles sont techniques, éparpillées et muettes.

**Le défi.** Pour obtenir un graphique de l'évolution de la consommation d'énergie par secteur en France, il faut savoir écrire du SQL, connaître les sources (EIA, Eurostat, Banque Mondiale, PRIMAP), et interpréter les résultats dans leur contexte. La barrière d'entrée est trop haute.

**Le résultat.** Posez une question en français. Vous obtenez un graphique, des chiffres, et une analyse qui met les données en perspective — comme si vous aviez un expert énergie en face de vous pour décrypter les chiffres.

### L'approche

Le défi technique central : **comment laisser une IA interroger une base de données sans qu'elle invente des tables, des colonnes ou des chiffres ?** La réponse : une couche sémantique qui sépare l'intention de l'exécution.

{{< mermaid >}}
flowchart TB
    Q["Question en\nlangage naturel"] --> CL["Classification"]
    CL -->|tactique| SL["Couche sémantique\n(catalogue de métriques)"]
    CL -->|stratégique| ST["Clarification\nmulti-étapes"]
    CL -->|corpus| RAG["Persona + RAG\n(convictions)"]
    SL --> SQL["SQL\ndéterministe"]
    SL -->|"métrique\ninconnue"| T2S["Text-to-SQL\n+ EXPLAIN"]
    SQL --> VIZ["Visualisation\nautomatique"]
    T2S --> VIZ
    VIZ --> AN["Analyse\nstreaming"]
    ST --> AN

    style Q fill:#FFF0E9,stroke:#FF6B35
    style AN fill:#FFF0E9,stroke:#FF6B35
{{< /mermaid >}}

**5 sources, 1 base.** Les données proviennent de l'EIA américaine (production/consommation), de l'ONU (flux par secteur), de PIK/PRIMAP (émissions de gaz à effet de serre), de la Banque Mondiale (population, PIB), et d'Eurostat (détail sectoriel européen). Cinq notebooks d'ingestion nettoient, normalisent et unifient tout dans une base PostgreSQL unique — 220 pays, 1960 à 2024.

**La couche sémantique : zéro hallucination SQL.** Un catalogue YAML décrit chaque métrique disponible : nom, table source, formule, dimensions possibles, filtres valides. Quand l'utilisateur pose une question, une IA rapide identifie la métrique et les filtres — puis le **SQL est assemblé de manière déterministe** à partir du catalogue. Pas de génération libre, pas d'invention de colonnes. Les métriques virtuelles (intensité énergétique, émissions par habitant...) sont définies déclarativement — l'IA choisit, le code calcule.

**Fallback contrôlé.** Pour les questions hors-catalogue, un Text-to-SQL génère la requête PostgreSQL, mais elle est **validée par EXPLAIN avant exécution**. Si le plan d'exécution échoue, la requête est rejetée — jamais de résultat faux silencieux.

**Classification multi-agents.** Chaque question est classée : *tactique* (donnée chiffrée), *stratégique* (question complexe nécessitant plusieurs sous-requêtes), *corpus* (question d'opinion — "que pense Jancovici du nucléaire ?"), ou *métadonnée* (question sur les données elles-mêmes). Chaque type suit un chemin différent avec ses propres agents.

**Visualisation automatique.** Le système choisit le bon type de graphique selon la forme des données — pas besoin de le spécifier :

| Forme des données | Visualisation |
|---|---|
| 1 valeur numérique | Carte KPI |
| Série temporelle | Courbe (Vega-Lite) |
| Comparaison catégorielle | Barres (Vega-Lite) |
| Multi-dimensions | Tableau |
| Autre | Graphique généré par IA |

**La personnalité Jancovici.** Le même pipeline que le projet coaching — 10 interviews transcrites, convictions extraites par clustering, lexique et anti-lexique, style d'analyse. L'analyste ne débite pas des chiffres : il les met en perspective, fait des analogies, et pose les bonnes questions de suivi.

**Feedback utilisateur.** Chaque réponse peut être notée (pouce haut/bas). Les votes sont stockés avec les métadonnées du pipeline (quel chemin, quelle métrique, quels filtres), ce qui permet d'identifier précisément les points faibles : est-ce la couche sémantique qui a choisi la mauvaise métrique ? Le Text-to-SQL qui a généré une requête bancale ? L'analyse qui n'a pas contextualisé ?

{{< button href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}
