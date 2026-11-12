# TP3 — Réponses

**Binôme :** Dadi Wiem

> Répondez sous chaque question. Les mesures (durées, pourcentages) sont celles de **votre** machine.

## Partie 1 — Pyramide des tests

**Q1.** Pour chacun des dossiers `tests/unit`, `tests/component`, `tests/integration` et `tests/e2e`, indiquez : le niveau de la pyramide, l'outil utilisé, et si la stack Docker doit tourner pour l'exécuter.
**Q2.** Pourquoi la pyramide des tests est-elle large à la base et étroite au sommet ? Donnez au moins deux critères.

## Partie 2 — Tests unitaires

**Q3.** Combien de tests sont exécutés et combien échouent ? Recopiez le message d'échec et expliquez ce que signifient `Expected` et `Received`.
Au premier lancement, 17 tests ont été exécutés : 16 ont réussi et 1 a échoué.

- Expected : résultat attendu par le test.
- Received : résultat réellement retourné par la fonction.
  L'échec indique que le résultat obtenu ne correspond pas au résultat attendu.

**Q4.** Quelle était la cause du bug dans `formatDuration` ? Expliquez pourquoi la correction fournie le résout.
Le problème venait de l'affichage des minutes restantes : même lorsqu'elles étaient égales à zéro, elles étaient affichées.

La correction consiste à ajouter une condition qui vérifie si les minutes restantes sont nulles. Si oui, seule l'heure est affichée, sinon les heures et les minutes sont affichées.

**Q5.** Avant d'ajouter des tests : relevez la couverture (lignes, branches, fonctions) de `lib/cours.ts`. D'après le rapport HTML, quelles lignes ou branches ne sont pas couvertes ?

- Lignes : 93,33 % (14/15).
- Branches : 78,57 % (11/14).
- Fonctions : 85,71 % (6/7).
  La ligne 48, qui effectue la recherche dans le titre et les topics, n'était pas exécutée. Certaines branches conditionnelles n'étaient pas testées.

**Q6.** Après l'ajout des deux tests fournis : quelle est la nouvelle couverture de `lib/cours.ts` ? Quelles lignes ou branches ces tests exécutent-ils qui ne l'étaient pas ? Reste-t-il quelque chose de non couvert, et pourquoi ?
-> Après l'ajout des deux tests, la couverture est :

- Statements : 94,11 % (16/17).
- Branches : 100 % (14/14).
- Functions : 85,71 % (6/7).
- Lines : 100 % (15/15).
  Les nouveaux tests exécutent notamment la ligne 48, qui effectue la recherche dans le titre et les topics, et couvrent toutes les branches conditionnelles.

Il reste une instruction et une fonction non couvertes. La couverture n'est donc pas encore de 100 % pour tous les indicateurs.

**Q7.** Quelle différence entre couverture de lignes et couverture de branches ? Un code couvert à 100 % est-il forcément sans bug ? Justifiez avec un exemple tiré de ce TP.
-> - Couverture de lignes : mesure le pourcentage de lignes de code exécutées par les tests.

- Couverture de branches : mesure si tous les chemins possibles des conditions (if/else, etc.) ont été testés.
  Non, un code couvert à 100 % n'est pas forcément sans bug.

Exemple du TP : dans filterCours, les tests peuvent exécuter toutes les lignes et toutes les branches, mais ne pas détecter un bug si la recherche ne gère pas correctement certains caractères spéciaux ou certains cas particuliers.

La couverture à 100 % garantit l'exécution du code, pas son bon fonctionnement.

## Partie 3 — Tests de composant

**Q8.** Que vérifie `CoursCard.test.tsx` qu'un test unitaire de `formatDuration` ne vérifie pas ? Quel outil simule le navigateur, et dans quel fichier est-il configuré ?
CoursCard.test.tsx vérifie le comportement et l'affichage de la composante React : titre, lien, durée, niveau et mots-clés.

Contrairement au test unitaire de formatDuration, il vérifie l'intégration de ces éléments dans l'interface.

L'outil utilisé est JSDOM, configuré dans vitest.config.ts. Il simule un environnement navigateur.

**Q9.** Pourquoi les tests recherchent-ils les éléments par rôle ou par texte (`getByRole`, `getByText`) plutôt que par classe CSS ?
Parce qu'ils permettent de tester les éléments selon leur rôle et leur contenu visible par l'utilisateur, plutôt que leur apparence.

Les classes CSS peuvent changer lors d'une modification du design sans modifier le comportement de l'application.

**Q10.** Pourquoi n'y a-t-il pas de test de composant pour `app/page.tsx` ? Quel niveau de test le couvre ?
app/page.tsx est une page complète qui récupère les données depuis Strapi et utilise plusieurs composants.

Elle est donc couverte par les tests E2E (End-to-End), qui vérifient le fonctionnement de l'application dans un navigateur réel, plutôt que par un simple test de composant.

## Partie 4 — Tests d'intégration

**Q11.** Que vérifient les tests d'intégration qui n'est vérifié par aucun test unitaire ou de composant ?
Ils vérifient que les fonctions de frontend/lib/api.ts communiquent correctement avec le vrai serveur Strapi : récupération des cours, tri, recherche par identifiant et gestion des identifiants inexistants.

**Q12.** Strapi arrêté : quel résultat et quel message obtenez-vous ? Pourquoi les tests unitaires passent-ils toujours ? À quoi sert `--no-deps` ?
Résultat : les 4 tests d'intégration échouent.

Message : TypeError: fetch failed, avec getaddrinfo ENOTFOUND strapi.

Cause : le serveur Strapi est arrêté et son nom strapi ne peut pas être résolu dans le réseau Docker.

Les tests unitaires passent toujours car ils testent les fonctions indépendamment de Strapi, sans avoir besoin d'un serveur réel.

--no-deps empêche Docker Compose de démarrer automatiquement les services déclarés dans depends_on, notamment Strapi.

**Q13.** Un test d'intégration en échec signale-t-il forcément un bug dans le code ? Citez deux autres causes possibles.
Non. Un échec ne signifie pas forcément qu'il existe un bug dans le code.

Deux autres causes possibles :

1. Un problème d'infrastructure : serveur Strapi arrêté ou indisponible.
2. Un problème de configuration réseau : mauvaise URL, mauvais port ou problème de connexion entre les conteneurs.

## Partie 5 — Tests de bout en bout

**Q14.** Quel scénario échoue ? Que montrent la capture d'écran et la trace ? Quel lien avec la question Q3 ? Pourquoi échoue-t-il alors que vous avez déjà corrigé le bug en partie 2 ?
Lors de la première exécution, le scénario suivant échouait :

la fiche du cours d'introduction affiche sa durée

La capture d'écran et la trace permettent de visualiser le comportement de l'application dans le navigateur, l'action qui provoque l'échec et le message d'erreur.

Ce bug est lié à la Q3 : le formatage incorrect de la durée (affichage de 1 h 0 au lieu de 1 h).

Le test échouait parce que le conteneur frontend utilisait une ancienne image Docker construite avant la correction

**Q15.** Le même bug est détecté par un test unitaire et par un test E2E. Lequel permet de trouver la cause le plus vite ? Lequel prouve que l'utilisateur est touché ?
Test unitaire : permet de trouver la cause du bug plus rapidement, car il teste directement la fonction responsable (formatDuration()), sans dépendre du navigateur ou des autres services.

Test E2E : prouve que le bug affecte réellement le parcours de l'utilisateur dans l'application complète, depuis le navigateur.

**Q16.** Complétez le tableau avec vos mesures :
Niveau | Nombre de tests | Durée d'exécution | Besoin de la stack ?
Unitaire + composant 19 5,97s Non
Intégration 4 2.18s Oui
E2E 4 7,5s Oui

**Q17.** Qu'est-ce qu'un test « flaky » (instable) ? Quels mécanismes de Playwright utilisés ici (attente automatique, assertions `expect(...).toHaveCount`, etc.) réduisent ce risque ?
Un test flaky est un test instable qui peut réussir ou échouer lors de plusieurs exécutions avec le même code, sans modification de celui-ci.

Cela peut arriver à cause :

- D'un chargement lent de la page.
- D'un problème réseau temporaire.
- D'une différence de temps d'exécution entre les machines.
  Playwright réduit ce risque grâce à plusieurs mécanismes :

1. Attente automatique (Auto-waiting) : Playwright attend automatiquement qu'un élément soit prêt avant d'effectuer une action.
2. Assertions avec réessai : expect(...).toHaveCount(6) attend que le nombre d'éléments attendu soit atteint avant de déclarer l'échec.
3. Attentes intelligentes : évitent d'utiliser systématiquement des délais fixes comme waitForTimeout(5000).

## Partie 6 — SonarQube

**Q18.** Collez votre `sonar-project.properties` complété et expliquez chaque valeur ajoutée. Pourquoi exclut-on `app/**` et `lib/api.ts` du calcul de couverture ?
**Pourquoi exclure `app/**`et`lib/api.ts` du calcul de couverture ?\*\*

Le dossier `app/**` est exclu car ses pages sont principalement vérifiées par les tests End-to-End (E2E) avec Playwright, qui simulent les interactions réelles d'un utilisateur dans le navigateur.

Le fichier `lib/api.ts` est exclu car ses fonctions de communication avec le backend Strapi sont vérifiées par les tests d'intégration, qui utilisent le véritable backend.

Ces exclusions évitent de pénaliser artificiellement le taux de couverture des tests unitaires Vitest. Les fichiers restent analysés par SonarQube : seule leur contribution au calcul de couverture est exclue.

**Q19.** Analyse avec le Quality Gate TP3 : statut du gate et conditions en échec ; nombre de bugs, vulnérabilités, security hotspots et code smells ; couverture ; taux de duplication.
Les 19 tests unitaires et composants ont réussi.

Couverture mesurée par Vitest :

- Statements : 92,30 %.
- Branches : 100 %.
- Functions : 85,71 %.
- Lines : 95,83 %.
  Le rapport SonarQube n'a pas pu être envoyé au serveur (UnknownHostException). Le statut du Quality Gate, les conditions en échec, les bugs, vulnérabilités, Security Hotspots, Code Smells, la couverture SonarQube et le taux de duplication restent donc à vérifier.

Conditions du Quality Gate :

- Coverage ≥ 90 %.
- Vulnerabilities = 0.

**Q20.** Expliquez la différence entre bug, vulnérabilité, security hotspot et code smell, avec un exemple tiré de votre rapport quand il en existe.
Bug : erreur pouvant provoquer un dysfonctionnement du programme. Exemple : condition logique incorrecte.

Vulnerability : faille de sécurité exploitable. Exemple : injection SQL.

Security Hotspot : code présentant un risque potentiel nécessitant une vérification humaine. Exemple : utilisation de code dynamique.

Code Smell : mauvaise pratique qui complique la maintenance. Exemple : fonction trop longue ou code dupliqué.

**Q21.** Ouvrez la vulnérabilité : quelle règle est en cause et quel est le risque ? Expliquez la correction fournie : d'où vient maintenant le mot de passe, et par quel chemin arrive-t-il jusqu'au code ? Pourquoi un secret dans le code est-il dangereux, même dans un dépôt privé ?
La vulnérabilité concerne un mot de passe stocké directement dans le code (Hard-coded Credentials).

- Risque : une personne ayant accès au dépôt peut récupérer le mot de passe et l'utiliser pour accéder au service.
- Correction : le mot de passe est déplacé dans une variable d'environnement, généralement définie dans .env.
- Chemin : fichier .env → Docker Compose (environment) → variable d'environnement du conteneur → code qui lit cette variable avec process.env.
- Pourquoi ? Même un dépôt privé peut être partagé, piraté ou exposé accidentellement. Un secret ne doit donc jamais être écrit directement dans le code.
  La règle exacte et le nom de la variable sont à confirmer dans le détail de la vulnérabilité

**Q22.** Quels code smells la correction fournie de `CoursList.tsx` fait-elle disparaître ? Pour chacun : règle, sévérité, explication, et ce qui, dans le nouveau code, règle le problème.
Les Code Smells sont des mauvaises pratiques qui compliquent la maintenance du code.

La correction de CoursList.tsx vise à supprimer les problèmes signalés par SonarQube en améliorant la structure et la lisibilité du composant.

Pour chaque problème, il faut relever :

- La règle SonarQube.
- Sa sévérité (Minor, Major, Critical…).
- L'explication du problème.
- La modification réalisée dans le nouveau code.
  Les 14 Code Smells affichés dans ton tableau de bord concernent le projet entier, pas nécessairement uniquement CoursList.tsx.

**Q23.** Où se trouve le code dupliqué ? Comment le supprimer ? (Ne le faites pas, décrivez seulement.)
Le code dupliqué correspond à des blocs de code identiques ou très similaires présents à plusieurs endroits.

Pour le supprimer, il faut extraire la partie commune dans une fonction ou un composant réutilisable, puis appeler cette fonction aux différents endroits.

Le rapport affiche actuellement 5,6 % de duplication.

**Q24.** Qu'est-ce que la dette technique dans SonarQube et comment est-elle exprimée ? Valeur avant et après vos corrections.
La dette technique représente le temps estimé nécessaire pour corriger les problèmes de maintenabilité du code.

SonarQube l'exprime en temps de correction estimé (minutes, heures ou jours).

La valeur avant et après correction doit être relevée dans Measures → Maintainability → Technical Debt.

**Q25.** Que signifie « New Code » dans SonarQube ? Quelles conditions du gate TP3 portent sur le nouveau code ? Pourquoi SonarQube recommande-t-il de juger le nouveau code plutôt que le code global ?
Le New Code représente le code ajouté ou modifié pendant une période de référence ou depuis une version de référence.

Les conditions du Quality Gate TP3 comprennent :

- Les conditions prédéfinies sur le New Code, conservées lors de la création du gate.
- Coverage sur Overall Code : au moins 90 %.
- Vulnerabilities sur Overall Code : 0.
  SonarQube recommande de contrôler le nouveau code pour éviter d'introduire de nouveaux problèmes sans bloquer systématiquement les projets à cause de leur ancien code.

**Q26.** Avec `sonar.qualitygate.wait=true`, que fait le scanner quand le Quality Gate est rouge ? Pourquoi est-ce utile dans un pipeline CI ?
Cette propriété oblige le scanner à attendre le résultat du Quality Gate après l'analyse.

- Si le Quality Gate est Passed, le scanner termine avec succès.
- S'il est Failed, le scanner retourne une erreur.
  C'est utile dans un pipeline CI/CD car cela permet de bloquer automatiquement la validation ou le déploiement d'un code qui ne respecte pas les critères de qualité.

## Partie 7 — Synthèse

**Q27.** Proposez l'ordre des étapes d'un pipeline CI pour ce projet (build, tests unitaires, intégration, E2E, analyse Sonar…) et justifiez-le. Quelles étapes doivent bloquer le merge d'une pull request ?
