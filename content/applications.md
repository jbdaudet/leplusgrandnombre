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

**Le contexte.** Le bruit étouffe l'idée. Stratèges et créatifs : vous lisez des dizaines d'articles pour nourrir vos reco. Mais entre la masse d'infos et l'urgence des délais, la "vérité universelle" — celle qui fait une grande campagne — reste souvent invisible. Faute de temps pour creuser, on finit par proposer des idées attendues, sans relief culturel.

**Le défi.** L'insight ne se décrète pas. Un article n'est qu'un tas de mots. En extraire une "Shower Thought" (une vérité humaine frappante) demande un recul que l'on n'a plus. Le défi n'est pas de résumer l'actualité, mais de percer la surface pour trouver l'opportunité créative unique qui fera vibrer une marque.

**Le résultat.** De l'article à l'idée, en un clic. Collez une URL. En quelques secondes, vous recevez une analyse culturelle, une vérité universelle et une idée créative concrète : "Et si [votre marque] faisait ceci…". Ce n'est pas une réponse générique, mais une proposition stratégique scorée sur sa capacité à provoquer et à générer du business.

### L'approche : la chaîne de valeur créative

Signal n'est pas un simple résumé. C'est un pipeline qui simule le cerveau d'un planneur stratégique senior via quatre étapes clés.

{{< signal-diagram >}}

**1. Le Curateur** — Scrape l'article et en extrait la substantifique moelle. Résumé structuré en français, streaming en temps réel.

**2. Le Chercheur** — Confronte l'article à une base de 3 000 vérités culturelles par similarité sémantique. Génère 5 candidats "Shower Thoughts", ne garde que le meilleur.

**3. Le Stratège** — Applique 3 prismes créatifs parmi 20 frameworks (détournement, analogie, provocation…) pour transformer le "Signal" en opportunité de marque.

**4. Le Juge** — Note les idées sur la surprise, la profondeur et la pertinence. Seule la meilleure est présentée.

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

### L'approche : quatre cerveaux pour une seule décision

Le système ne se contente pas de répondre. Il utilise un **aiguilleur** qui analyse votre intention pour activer le moteur le plus fiable.

**1. Le moteur tactique — Précision chiffrée.**
Ce qu'il fait : il extrait le chiffre exact via une couche sémantique déterministe (YAML). La garantie : pas d'hallucination SQL. Si la question est inédite, il génère du code mais le fait valider par un audit système (EXPLAIN) avant de l'exécuter.

**2. Le moteur stratégique — Réflexion profonde.**
Ce qu'il fait : il ne donne pas de réponse immédiate. Il engage un dialogue de clarification pour cerner votre besoin. La garantie : une approche en 4 étapes (diagnostic → vision → compromis → feuille de route) pour transformer une intuition en plan d'action.

**3. Le moteur corpus — Mémoire de l'expert.**
Ce qu'il fait : il fouille dans 10+ interviews et documents via une recherche vectorielle (pgvector) en 4 étapes. La garantie : la réponse est nourrie par des convictions réelles et un lexique spécifique, pas par une IA générique.

**4. Le moteur métadonnées — Transparence.**
Ce qu'il fait : il interroge directement le dictionnaire de données pour expliquer ce qu'il y a "sous le capot". La garantie : vous savez toujours d'où vient la donnée et comment elle est calculée.

{{< button href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}
