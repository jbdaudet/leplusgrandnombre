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

**L'approche.** Le rapport croise cinq sources de données publiques françaises (INSEE, IGN, BPE, OpenStreetMap) à l'échelle la plus fine disponible — l'IRIS, un découpage de 2 000 habitants. La zone de chalandise s'adapte automatiquement au tissu urbain : 750 m en centre-ville, jusqu'à 3 km en zone rurale. L'estimation de flux piétons utilise une méthode de centralité de Sevtsuk (2021) qui modélise les déplacements réels domicile-commerces plutôt qu'un calcul théorique entre tous les points. L'analyse concurrentielle identifie automatiquement les compétiteurs via IA et les localise via Google Places, avec synthèse des avis clients.

{{< button href="https://geomarketing.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="signal">Trouver le bon angle <a href="https://signal.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">signal</a></h2>

Une fois qu'on connaît son marché, il faut trouver le message qui va résonner. Pas un message générique — un angle culturel, ancré dans l'actualité, qui crée une connexion authentique.

**Le contexte.** Un stratège de marque ou un créatif qui lit des dizaines d'articles par semaine en cherchant l'insight qui deviendra une campagne.

**Le défi.** Repérer la vérité universelle cachée dans un article, puis la transformer en opportunité créative pour une marque, c'est un travail d'expert qui prend des heures. Et c'est difficilement reproductible.

**Le résultat.** Collez le lien d'un article. En 30 secondes, vous obtenez une analyse culturelle, la vérité universelle cachée dans le texte, et une idée créative prête à explorer pour votre marque. Ce que fait un planneur stratégique en une demi-journée, automatisé et reproductible.

**L'approche.** Le système ne génère pas une idée au hasard. Il cherche d'abord dans un index de 3 000 "vérités culturelles" pré-construites celle qui résonne le plus avec l'article (par similarité sémantique). Puis il génère 5 candidats, que l'IA évalue sur des critères précis : simplicité, surprise, profondeur. Pour l'idée créative, 3 prismes sont testés parmi une bibliothèque de 20 frameworks créatifs, et chaque idée est notée sur sa justesse stratégique, son potentiel génératif et sa force de provocation. Seuls les meilleurs résultats sont présentés. Des documents de marque (PDF, Word) peuvent être uploadés pour ancrer le tout dans un contexte réel.

{{< button href="https://signal.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="coaching">Créer du contenu qui me ressemble <a href="https://coaching.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">coaching</a></h2>

Produire du contenu à grande échelle, c'est facile. Produire du contenu qui sonne comme soi — avec ses convictions, son vocabulaire, ses tics de langage — c'est un autre problème.

**Le contexte.** Un coach avec 20 ans de pratique, 918 documents publiés, une voix unique. Il veut démultiplier son impact sans diluer ce qui fait sa singularité.

**Le défi.** Un chatbot classique retrouve les passages pertinents dans un corpus et les restitue. Ça marche quand le sujet a été traité. Mais la valeur d'un expert, c'est sa perspective — sa capacité à avoir un avis sur des sujets qu'il n'a jamais explicitement abordés. Là, le chatbot classique se tait.

**Le résultat.** Un double numérique du coach qui répond comme lui — y compris sur des sujets qu'il n'a jamais directement traités. Et un outil qui restructure ses articles avec l'efficacité narrative des meilleurs TED talks, sans perdre sa voix.

**L'approche.** Plutôt que d'indexer des documents, le système extrait les 79 convictions profondes du coach par clustering non supervisé (BERTopic), puis construit une hiérarchie et un graphe de co-occurrence entre convictions. Quand un utilisateur pose une question hors-corpus, le système identifie quelles convictions s'appliquent, de quelles croyances plus profondes elles découlent, et quelles opinions adjacentes le coach amènerait naturellement. La voix est capturée sur six dimensions : échantillons d'écriture, lexique de prédilection, anti-lexique (les mots qu'il n'utilise jamais), profil stylistique, biographie et cadres de référence. Le moteur de réécriture d'articles analyse la structure de 267 TED talks pour en extraire les schémas narratifs par intention (enseigner, convaincre, inspirer), puis réécrit l'article section par section avec les mêmes techniques rhétoriques — sans toucher aux idées.

{{< button href="https://coaching.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="artisans-ai">Gagner du temps au quotidien <a href="https://artisans.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">artisans.ai</a></h2>

Automatiser les tâches chronophages pour se concentrer sur ce qu'on sait faire — sans sacrifier la qualité.

**Le contexte.** Un artisan du bâtiment — menuisier, plombier, électricien — qui passe ses soirées à rédiger des devis, gérer son stock et planifier ses chantiers au lieu d'exercer son métier.

**Le défi.** Les outils de gestion existants sont soit trop génériques (ils ne comprennent pas le métier), soit trop lourds (pensés pour des entreprises de 50 personnes). L'artisan finit par tout faire à la main, sur Excel ou sur papier.

**Le résultat.** L'artisan décrit son chantier en quelques phrases. L'outil génère les étapes de fabrication, identifie les matériaux nécessaires dans son stock, et produit un devis professionnel conforme aux normes françaises. Ce qui prenait une soirée prend 10 minutes.

**L'approche.** L'IA est adaptée à 17 métiers du bâtiment, avec un vocabulaire et des références propres à chaque profession. La conversation de cadrage suit un guide structuré sur 4 axes (portée, spécifications, matériaux, installation) et ne pose que les questions auxquelles le descriptif initial n'a pas déjà répondu. La gamme opératoire catégorise chaque étape (conception, fabrication, installation) pour alimenter directement le devis. Les feuilles de débit (PDF, image, Excel) sont parsées par IA puis confrontées au stock via un algorithme de bin-packing (first-fit decreasing) qui optimise l'utilisation des chutes. Le stock initial est généré par un pipeline IA en 6 étapes séparées — chacune suffisamment courte pour éviter les troncatures — avec progression en temps réel via Server-Sent Events. Les devis respectent les exigences légales françaises : numéro séquentiel, SIREN, TVA, acompte configurable, export PDF.

{{< button href="https://artisans.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}

---

<h2 id="theshiftproject">Piloter par les résultats <a href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" rel="noopener noreferrer" class="tool-name">theshiftproject</a></h2>

Quand les données sont là, encore faut-il pouvoir les interroger, les comprendre et en tirer des décisions — sans être data analyst.

**Le contexte.** N'importe qui s'intéressant aux questions d'énergie et de climat : un analyste, un journaliste, un étudiant, un décideur. Les données mondiales existent, mais elles sont techniques, éparpillées et muettes.

**Le défi.** Pour obtenir un graphique de l'évolution de la consommation d'énergie par secteur en France, il faut savoir écrire du SQL, connaître les sources (EIA, Eurostat, Banque Mondiale, PRIMAP), et interpréter les résultats dans leur contexte. La barrière d'entrée est trop haute.

**Le résultat.** Posez une question en français. Vous obtenez un graphique, des chiffres, et une analyse qui met les données en perspective — comme si vous aviez un expert énergie en face de vous pour décrypter les chiffres.

**L'approche.** Cinq sources de données internationales sont normalisées dans une base unique (220 pays, 1960-2024). Plutôt que de laisser l'IA générer du SQL à l'aveugle (avec les hallucinations que ça implique), un catalogue sémantique (YAML) décrit chaque métrique disponible. L'IA choisit la métrique, et le SQL est assemblé de manière déterministe — zéro risque d'erreur. Pour les questions inédites, un fallback text-to-SQL génère la requête, validée par EXPLAIN avant exécution. Les visualisations (Vega-Lite) sont choisies automatiquement selon la forme des données : KPI pour une valeur unique, courbe pour une série temporelle, barres pour une comparaison. La personnalité de l'analyste est construite à partir de 10 interviews transcrites et analysées par le même pipeline de conviction que le projet coaching — lexique, convictions, style, anti-lexique.

{{< button href="https://theshiftproject.leplusgrandnombre.fr/" target="_blank" >}}Essayer{{< /button >}}
