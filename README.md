# SentinelFonctionnelle

SentinelFonctionnelle est une application web destinée à transformer des indications utilisateur et des captures d’écran en scénarios de tests fonctionnels exécutables, grâce à un LLM configurable.

Le produit doit accompagner l’utilisateur depuis la description d’un parcours fonctionnel jusqu’à la génération d’un code de test réutilisable et maintenable, puis permettre d’en vérifier l’exécution.

## Périmètre

SentinelFonctionnelle concerne exclusivement la conception et la génération de tests fonctionnels d’interfaces web.

Il ne s’agit pas d’un outil de diagnostic matériel : la détection des CPU/GPU, `rocm-smi`, `nvidia-smi` et les diagnostics d’infrastructure appartiennent au projet séparé **AI Tester**.

## Principes directeurs

- interaction guidée entre l’utilisateur et un LLM configurable ;
- définition des tests à partir d’objectifs métier et de captures d’écran ;
- boucle d’exploration « observer, raisonner, agir » ;
- validation humaine avant la génération ou l’exécution d’actions sensibles ;
- génération prioritaire de tests Playwright lisibles et déterministes ;
- utilisation de sélecteurs stables plutôt que de coordonnées visuelles fragiles ;
- séparation stricte entre secrets, spécifications, preuves et code généré ;
- développement du produit en TDD.

## Documentation

La spécification initiale consolidée se trouve dans :

- [`docs/SPECIFICATIONS.md`](docs/SPECIFICATIONS.md)

## État du projet

Le dépôt contient actuellement les spécifications initiales. L’architecture exécutable et la première tranche verticale restent à implémenter en TDD.

## Licence

Ce projet est distribué sous licence MIT. Consultez [`LICENSE`](LICENSE).
