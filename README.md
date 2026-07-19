# SentinelFonctionnelle

SentinelFonctionnelle est un outillage web destiné à transformer des indications utilisateur et des captures d’écran en scénarios de tests fonctionnels exécutables, grâce à une interaction guidée avec un LLM configurable.

Le produit sépare explicitement deux activités :

1. **la conception**, interactive, pendant laquelle l’utilisateur dialogue avec un LLM, structure ses tests et les valide individuellement ;
2. **l’exécution**, automatisable, qui lance du code Playwright généré ou personnalisé et ne sollicite un LLM d’exécution que dans des cas exceptionnels explicitement autorisés.

## Principes directeurs

- organisation hiérarchique des suites et des tests ;
- description synthétique et factuelle mise à jour lors de l’édition d’un test ;
- validation individuelle des tests pendant leur conception ;
- profils LLM de conception et d’exécution distincts ;
- tests exécutables sans LLM dans le cas nominal ;
- génération de code Playwright lisible, déterministe et personnalisable ;
- utilisation de sélecteurs stables plutôt que de coordonnées visuelles fragiles ;
- séparation stricte entre secrets, spécifications, preuves et code généré ;
- déploiement conteneurisé simple ;
- intégration native aux chaînes GitHub Actions et GitLab CI par une CLI, des codes de sortie standards et des rapports JUnit ;
- développement du produit en TDD.

## Documentation

La spécification fonctionnelle, le MVP et l’architecture logicielle sont décrits dans :

- [`docs/SPECIFICATIONS.md`](docs/SPECIFICATIONS.md)

## État du projet

Le dépôt contient actuellement les spécifications initiales. L’architecture exécutable et la première tranche verticale restent à implémenter en TDD.

## Licence

Ce projet est distribué sous licence MIT. Consultez [`LICENSE`](LICENSE).
