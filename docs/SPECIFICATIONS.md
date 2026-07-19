# Spécifications initiales de SentinelFonctionnelle

- **Statut :** base de cadrage à valider et affiner
- **Projet :** SentinelFonctionnelle
- **Dépôt :** `pessonnier/SentinelFonctionnelle`
- **Branche principale :** `main`
- **Licence :** MIT

## 1. Objet du document

Ce document consolide les besoins exprimés lors des échanges préalables. Il distingue :

1. les décisions déjà confirmées ;
2. les exigences déduites directement du besoin ;
3. les hypothèses techniques proposées pour rendre le produit réalisable ;
4. les questions qui nécessitent encore une décision.

Il constitue la référence initiale du produit, mais ne remplace pas la validation progressive des scénarios et critères d’acceptation pendant le développement.

## 2. Vision du produit

SentinelFonctionnelle est une application web qui aide un utilisateur à définir des jeux de tests fonctionnels à partir :

- d’indications exprimées en langage naturel ;
- de captures d’écran d’une application web ;
- de l’observation contrôlée de l’interface cible ;
- d’un dialogue avec un LLM configurable.

Le résultat attendu est du code de test fonctionnel :

- exécutable ;
- déterministe autant que possible ;
- lisible par une équipe de développement ou de QA ;
- réutilisable ;
- maintenable lorsque l’interface évolue ;
- accompagné de ses hypothèses, données et preuves d’exécution.

## 3. Décisions confirmées

### 3.1 Identité et responsabilité

Le nom officiel du projet est **SentinelFonctionnelle**.

Sa responsabilité est de générer des tests fonctionnels avec l’aide d’un LLM et de l’analyse visuelle d’interfaces.

### 3.2 Séparation avec AI Tester

SentinelFonctionnelle ne doit pas absorber les fonctions d’AI Tester.

| Projet | Responsabilité |
|---|---|
| **SentinelFonctionnelle** | Définition, génération et exécution assistées de tests fonctionnels d’interfaces web |
| **AI Tester** | Vérification et diagnostic du matériel, notamment CPU et GPU |

Les dépendances liées à `rocm-smi`, `nvidia-smi` ou au diagnostic matériel sont hors périmètre de SentinelFonctionnelle.

### 3.3 Interaction principale

Le produit doit proposer une application web dans laquelle l’utilisateur peut :

1. décrire un objectif ou un parcours fonctionnel ;
2. fournir des captures d’écran et du contexte ;
3. dialoguer avec un LLM configurable ;
4. corriger ou approuver le scénario proposé ;
5. obtenir du code de test réutilisable et maintenable.

### 3.4 Modèle d’exploration

L’exploration d’une interface suit la boucle :

1. **Observer** : charger la page, collecter une représentation du DOM et une capture d’écran ;
2. **Raisonner** : interpréter l’état courant au regard du scénario approuvé ;
3. **Agir** : cliquer, saisir une valeur, naviguer ou attendre un changement ;
4. **Vérifier** : contrôler les résultats attendus et conserver les preuves utiles ;
5. **Itérer** jusqu’à la fin du scénario ou jusqu’à un blocage explicite.

### 3.5 Méthode de développement

Le produit doit être développé en TDD :

1. écrire un test en échec pour un comportement observable ;
2. vérifier que l’échec correspond au comportement manquant ;
3. ajouter l’implémentation minimale ;
4. exécuter le test ciblé ;
5. exécuter toute la suite ;
6. refactoriser uniquement lorsque les tests sont verts.

## 4. Utilisateurs visés

### 4.1 Testeur fonctionnel ou responsable QA

Il décrit les parcours métier, fournit des exemples visuels, examine les scénarios proposés et valide le code généré sans devoir écrire l’intégralité du test.

### 4.2 Développeur

Il récupère un test versionnable, compréhensible et compatible avec les pratiques du dépôt cible. Il doit pouvoir le modifier sans dépendre du LLM.

### 4.3 Responsable produit

Il exprime un comportement attendu sous forme de parcours et de critères d’acceptation, puis vérifie que le scénario généré correspond au besoin métier.

## 5. Parcours nominal

1. L’utilisateur crée un projet de test.
2. Il renseigne l’URL ou l’environnement cible.
3. Il configure ou sélectionne un profil LLM.
4. Il crée une campagne et décrit son objectif métier.
5. Il ajoute une ou plusieurs captures d’écran annotées ou non.
6. Le LLM pose les questions nécessaires pour lever les ambiguïtés.
7. Le système produit un scénario structuré comprenant préconditions, étapes, données et résultats attendus.
8. L’utilisateur modifie puis approuve explicitement ce scénario.
9. Le moteur de navigateur explore l’application dans un environnement contrôlé.
10. Chaque action et chaque observation utile sont consignées.
11. Le générateur produit un test Playwright et ses artefacts associés.
12. Le test est exécuté dans un environnement isolé.
13. L’utilisateur consulte le résultat, les journaux, captures et traces.
14. Le code validé peut être téléchargé ou intégré à un dépôt cible selon une politique d’accès explicite.

## 6. Exigences fonctionnelles

### EF-001 — Gestion des projets de test

Le système doit permettre de créer, consulter, modifier et archiver un projet de test contenant au minimum un nom, une description et une cible web.

### EF-002 — Configuration du LLM

Le système doit permettre de configurer plusieurs profils LLM sans lier le produit à un fournisseur unique.

Un profil doit pouvoir définir :

- le fournisseur ou protocole compatible ;
- l’URL de service ;
- le modèle ;
- les paramètres d’inférence autorisés ;
- une référence vers un secret, sans exposer sa valeur dans les exports ou journaux.

La connectivité doit être vérifiée indépendamment de la génération d’un scénario.

### EF-003 — Dialogue de cadrage

Le système doit conserver un dialogue contextualisé permettant au LLM de demander des précisions sur :

- l’objectif métier ;
- les préconditions ;
- les rôles utilisateurs ;
- les données de test ;
- les résultats attendus ;
- les erreurs ou variantes à couvrir.

Une proposition du LLM ne doit jamais être présentée comme une exigence confirmée sans validation humaine.

### EF-004 — Gestion des captures d’écran

L’utilisateur doit pouvoir joindre des captures à un projet, une campagne ou une étape.

Chaque capture doit pouvoir recevoir :

- un libellé ;
- une description ;
- l’état du parcours auquel elle correspond ;
- des annotations ou zones d’intérêt ;
- une date et une provenance.

Le système doit conserver le lien entre une capture, l’étape dérivée et la version du scénario.

### EF-005 — Scénario structuré

Le système doit transformer le dialogue en une représentation structurée contenant au minimum :

- un titre et un objectif ;
- les préconditions ;
- les données de test ;
- une suite ordonnée d’actions ;
- un résultat attendu par étape lorsque pertinent ;
- les postconditions ;
- les hypothèses et ambiguïtés restantes.

La représentation structurée est la source de vérité de la génération. Le code ne doit pas être généré directement à partir d’un historique de conversation non validé.

### EF-006 — Validation humaine

L’utilisateur doit pouvoir :

- éditer le scénario proposé ;
- accepter ou refuser une étape ;
- demander une nouvelle proposition ;
- approuver une version immuable avant exploration ou génération.

Toute modification après approbation doit créer une nouvelle version traçable.

### EF-007 — Exploration contrôlée du navigateur

Le système doit pouvoir naviguer vers la cible et réaliser les actions approuvées :

- navigation ;
- clic ;
- saisie ;
- sélection ;
- attente d’un état observable ;
- lecture d’un contenu ;
- capture d’une preuve.

Les références temporaires issues d’un instantané de navigateur ou les coordonnées visuelles ne doivent pas être copiées telles quelles dans le code final. Elles servent à l’exploration, puis doivent être converties en localisateurs stables.

### EF-008 — Gestion des blocages

L’exploration doit s’arrêter proprement lorsque :

- la cible n’est pas accessible ;
- un élément est introuvable ou ambigu ;
- une authentification ou une donnée manque ;
- l’action proposée sort du scénario approuvé ;
- une politique de sécurité interdit la cible ou l’action ;
- le nombre maximal d’étapes ou la durée maximale est atteint.

Le système doit expliquer le blocage et demander une décision plutôt que d’inventer une réussite.

### EF-009 — Génération de code

La première cible de génération proposée est **Playwright**. Le langage exact du MVP reste à confirmer ; Python est privilégié dans les échanges initiaux.

Le code généré doit :

- être autonome vis-à-vis du LLM pendant son exécution normale ;
- utiliser des localisateurs stables, prioritairement rôle, libellé, texte ou attribut de test ;
- éviter les temporisations arbitraires lorsque Playwright peut attendre un état ;
- isoler les données variables ;
- produire des assertions explicites ;
- nettoyer l’état qu’il crée lorsque nécessaire ;
- ne contenir aucun secret en clair ;
- factoriser les comportements réellement partagés sans surarchitecture ;
- inclure un commentaire uniquement lorsqu’il explique une décision non évidente.

### EF-010 — Prévisualisation et traçabilité

Avant export, l’utilisateur doit pouvoir consulter :

- le scénario source ;
- le code généré ;
- les hypothèses ;
- les localisateurs retenus ;
- les différences avec une version précédente ;
- la version du générateur et du modèle ayant participé à la génération.

### EF-011 — Exécution et résultats

Le système doit pouvoir lancer le test généré dans un environnement isolé et afficher :

- le statut global ;
- le statut de chaque étape ;
- les erreurs et traces ;
- les captures pertinentes ;
- la durée ;
- la version exacte du scénario et du code exécutés.

Une exploration réussie ne doit pas être confondue avec une exécution réussie du code généré.

### EF-012 — Export

Le système doit au minimum permettre de télécharger :

- le code du test ;
- sa représentation structurée ;
- les données d’exemple non secrètes ;
- les instructions d’installation et d’exécution ;
- les preuves sélectionnées.

L’écriture directe dans un dépôt Git est une capacité ultérieure et doit être soumise à une autorisation dédiée, limitée au dépôt concerné.

## 7. Exigences de qualité du code généré

### 7.1 Déterminisme

À scénario, état initial et version de générateur identiques, le test doit conserver la même structure fonctionnelle. Les variations stylistiques du LLM ne doivent pas modifier silencieusement le comportement approuvé.

### 7.2 Maintenabilité

Le test doit privilégier :

1. les rôles et noms accessibles ;
2. les libellés de formulaire ;
3. les attributs de test convenus avec l’équipe ;
4. les sélecteurs CSS stables en dernier recours.

Les sélecteurs dépendant d’une position, d’un identifiant généré ou d’une structure DOM profonde doivent être signalés comme fragiles.

### 7.3 Lisibilité

Chaque test doit exprimer clairement :

- son intention ;
- sa préparation ;
- son action principale ;
- ses assertions ;
- son nettoyage éventuel.

### 7.4 Réutilisabilité

Les parcours partagés peuvent être extraits en fonctions, fixtures ou objets de page lorsqu’au moins deux scénarios justifient cette factorisation. Le MVP ne doit pas générer une hiérarchie abstraite par anticipation.

### 7.5 Reproductibilité

Un export doit déclarer ses versions minimales, sa commande d’installation, sa commande d’exécution et les variables d’environnement attendues.

## 8. Exigences non fonctionnelles

### ENF-001 — Sécurité des destinations

Les URL fournies par l’utilisateur doivent être validées avant tout accès. Par défaut, le moteur doit refuser ou encadrer strictement :

- les schémas non HTTP(S) ;
- les métadonnées cloud ;
- les adresses internes ou de boucle locale lorsqu’elles ne sont pas explicitement autorisées ;
- les redirections vers une destination interdite.

Cette politique doit être configurable par un administrateur, pas contournable par le LLM.

### ENF-002 — Protection des secrets

Les mots de passe, jetons et clés doivent être stockés dans un mécanisme de secrets. Ils doivent être masqués dans l’interface, les prompts, les journaux, les captures et les exports.

### ENF-003 — Isolation d’exécution

Le navigateur et le code généré doivent s’exécuter avec des ressources, une durée et un accès réseau limités. L’exécution ne doit pas avoir accès au système hôte ou à d’autres projets par défaut.

### ENF-004 — Auditabilité

Chaque décision automatisée importante doit être rattachée à :

- une demande utilisateur ;
- une version de scénario ;
- une version de modèle et de prompt système ;
- une observation ou preuve ;
- une action résultante.

### ENF-005 — Confidentialité

La politique de conservation des conversations, captures, traces et prompts doit être explicite. La suppression d’un projet doit pouvoir supprimer ses artefacts selon la politique définie.

### ENF-006 — Portabilité

Le produit doit séparer son domaine métier des adaptateurs de LLM, de stockage et de navigateur afin de permettre leur remplacement sans réécrire le modèle de scénario.

### ENF-007 — Résilience

Une indisponibilité du LLM, du navigateur ou de la cible doit produire un état structuré et compréhensible. Aucun composant ne doit simuler un succès pour masquer une dépendance indisponible.

### ENF-008 — Observabilité

Les journaux doivent permettre de corréler une campagne, une génération et une exécution sans enregistrer de secrets. Des métriques minimales doivent couvrir les durées, échecs et causes de blocage.

## 9. Modèle conceptuel

### Projet de test

Regroupe une cible, des profils autorisés, des campagnes et une politique de sécurité.

### Profil LLM

Décrit un adaptateur, un endpoint, un modèle, des paramètres et une référence de secret.

### Campagne

Regroupe plusieurs scénarios poursuivant un objectif fonctionnel commun.

### Scénario

Versionne les préconditions, données, étapes, assertions, hypothèses et approbations.

### Étape

Décrit une action ou une vérification indépendamment de sa traduction Playwright.

### Capture et observation

Conservent une preuve visuelle ou structurée et leur relation avec une étape.

### Artefact généré

Associe un scénario approuvé à une version de code, un générateur et un modèle.

### Exécution

Associe un artefact immuable à un environnement, un résultat, des journaux et des preuves.

## 10. Architecture logique proposée

Les composants suivants constituent une hypothèse d’architecture, à confirmer par des tranches verticales TDD :

1. **Interface web** : dialogue, captures, édition et approbation des scénarios.
2. **API applicative** : expose les cas d’utilisation sans dépendre d’un fournisseur LLM.
3. **Domaine** : projets, campagnes, scénarios versionnés, approbations et exécutions.
4. **Adaptateurs LLM** : traduisent un contrat interne vers Ollama ou une API compatible OpenAI, puis vers d’autres fournisseurs si nécessaire.
5. **Moteur d’exploration** : pilote Playwright, collecte DOM, captures et traces.
6. **Générateur** : transforme uniquement un scénario approuvé et enrichi en code source.
7. **Exécuteur isolé** : lance l’artefact généré avec des limites explicites.
8. **Stockage** : sépare métadonnées, secrets et artefacts volumineux.

Le LLM propose et interprète ; le domaine valide les transitions ; le moteur de sécurité autorise les destinations et actions ; Playwright exécute. Aucun texte produit par le LLM ne doit contourner ces contrôles.

## 11. MVP proposé

Le MVP doit prouver une seule tranche verticale complète :

1. créer un projet ciblant une application web de démonstration ;
2. sélectionner un profil LLM configurable ;
3. décrire un scénario simple ;
4. joindre une capture ;
5. obtenir puis éditer un scénario structuré ;
6. approuver ce scénario ;
7. explorer un parcours de deux ou trois actions non destructives ;
8. générer un test Playwright ;
9. exécuter ce test ;
10. afficher le résultat et permettre le téléchargement des artefacts.

Le MVP ne doit pas chercher à résoudre tous les sites, CAPTCHAs, authentifications multifacteurs ou interfaces graphiques complexes.

## 12. Critères d’acceptation du MVP

Le MVP est acceptable lorsque les critères suivants sont vérifiés par des tests automatisés et une démonstration réelle :

1. Un utilisateur peut créer un projet et un scénario sans éditer directement un fichier.
2. Une capture valide est rattachée au scénario ; un format invalide est refusé explicitement.
3. Un LLM simulé dans les tests et un LLM réel disponible utilisent le même contrat applicatif.
4. Une réponse LLM invalide ne peut pas créer un scénario approuvé.
5. Le scénario reste modifiable avant approbation et devient immuable après approbation.
6. L’exploration refuse une action absente du scénario approuvé.
7. Une destination interdite est bloquée avant le lancement du navigateur.
8. Le code généré ne contient ni référence éphémère de navigateur ni secret.
9. Le test Playwright généré réussit sur l’application de démonstration.
10. Une régression volontaire de l’application de démonstration fait échouer le test avec une preuve exploitable.
11. Le résultat distingue clairement exploration, génération et exécution.
12. L’export contient le scénario, le code, les versions et les commandes de reproduction.

## 13. Hors périmètre initial

- diagnostic CPU, GPU ou système ;
- entraînement ou fine-tuning d’un LLM ;
- résolution de CAPTCHA ou contournement d’authentification ;
- exploration autonome sans objectif ni limites approuvés ;
- tests natifs mobiles ou desktop ;
- tests de charge et de performance ;
- correction automatique de l’application cible ;
- publication automatique dans un dépôt sans autorisation dédiée ;
- prise en charge simultanée de plusieurs frameworks de test dans le MVP.

## 14. Questions ouvertes

Les décisions suivantes doivent être prises avant ou pendant les premières tranches TDD :

1. Le code Playwright du MVP sera-t-il généré en Python ou en TypeScript ?
2. Quels protocoles LLM seront officiellement supportés au MVP : Ollama, API compatible OpenAI, ou les deux ?
3. Quel mécanisme d’authentification utilisera l’application web ?
4. Quel stockage persistant sera retenu pour les métadonnées et les artefacts ?
5. Quelle politique autorisera les réseaux privés et environnements de développement ?
6. Quelle durée de conservation appliquer aux captures, prompts et traces ?
7. Quel mécanisme d’isolation sera exigé pour exécuter le code généré ?
8. L’intégration Git sera-t-elle incluse après l’export local ou reportée à une version ultérieure ?
9. Quelle application de démonstration servira de référence E2E au MVP ?

## 15. Stratégie de validation et de développement

Chaque exigence implémentée doit suivre une tranche verticale :

1. formaliser un critère d’acceptation observable ;
2. écrire le test automatisé correspondant et constater son échec ;
3. implémenter le minimum nécessaire ;
4. faire passer le test ciblé ;
5. exécuter la suite complète ;
6. exercer le véritable point d’entrée concerné ;
7. documenter séparément les dépendances externes indisponibles ;
8. effectuer une revue de sécurité pour toute fonction de navigation, de secret ou d’exécution.

Les tests doivent couvrir au minimum :

- le domaine sans LLM ni navigateur réels ;
- les contrats d’adaptateurs ;
- les erreurs et données non fiables ;
- les politiques de destination ;
- la génération déterministe à partir d’un scénario figé ;
- l’exécution E2E sur une cible de démonstration contrôlée.

## 16. Traçabilité des besoins initiaux

Cette spécification repose sur les besoins explicitement exprimés :

- développer une application web ;
- faire interagir l’utilisateur avec un LLM configurable ;
- définir des jeux de tests fonctionnels à partir des indications de l’utilisateur et de captures d’écran ;
- utiliser une boucle d’observation et d’interaction avec l’interface ;
- générer du code de test réutilisable et maintenable ;
- séparer strictement ce produit du diagnostic matériel d’AI Tester.

Les sections de sécurité, de versionnement, de validation humaine et d’isolation sont des exigences dérivées nécessaires pour rendre cette capacité contrôlable, vérifiable et exploitable en production. Elles devront être confirmées lors de la validation du cadrage.
