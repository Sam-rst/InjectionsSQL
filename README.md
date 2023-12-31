# InjectionsSQL
Que sont les INJECTIONS SQL ? Sont-elles si dangereuses que ça ? Présentent-elles un risque pour le base de données ? Pour le serveur ? C'est ce que nous verrons à travers cette présentation.

## Contexte actuel des site internet :

La plupart des applications web reposent sur l'utilisation d'une ou plusieurs bases de données pour stocker et gérer les informations en temps réel.

Lorsqu'un utilisateur envoie des requêtes, l'application interroge la base de données pour construire la réponse. Cependant, si les données fournies par l'utilisateur sont utilisées pour créer la requête à la base de données, un attaquant peut manipuler ces informations dans le but de détourner la requête pour des actions non prévues par le développeur initial. Cela ouvre la porte à une attaque par injection SQL, souvent abrégée en SQLi.

L'injection SQL cible spécifiquement les bases de données relationnelles telles que MySQL, Oracle Database, ou Microsoft SQL Server. En revanche, les injections visant les bases de données non relationnelles, comme MongoDB ou CouchDB, sont appelées injections NoSQL.

Cet article offre un aperçu des principes, des impacts et des méthodes d'exploitation des injections SQL. Il présente également des bonnes pratiques de sécurité et des mesures à mettre en place pour prévenir les risques liés à de telles attaques.

## Qu’est ce qu’une injection de code ?

Une injection de code se réfère à une méthode d'exploitation de vulnérabilités dans une application. Elle implique l'introduction non autorisée de code dans le système, visant à altérer le cours normal de l'exécution de l'application. Ces attaques sont généralement non anticipés par le système de sécurité et peuvent compromettre la stabilité et l'intégrité de l'application. Certains types d'injections de code visent à obtenir des privilèges élevés, tandis que d'autres cherchent à installer des logiciels malveillants.

## Comment fonctionne les attaques : 

La quête pour identifier une éventuelle vulnérabilité aux attaques par injection SQL (SQLi) commence par l'exploration du formulaire de connexion. L'objectif est de déceler tout paramètre vulnérable qui pourrait être exploité pour détourner la logique de l'application web et contourner l'authentification.

Pour ce faire, une approche consiste à ajouter l'un des payloads suivant(‘ ,  ” , # , ; , ) ) après le nom d'utilisateur et observer si cela entraîne des erreurs ou modifie le comportement de la page. La requête SQL envoyée à la base de données serait ainsi altérée. Le guillemet introduit initialement peut engendrer un nombre impair de guillemets, conduisant à une erreur de syntaxe. Une stratégie pour contourner cela serait de commenter et d'insérer le reste de la requête dans le contexte de notre injection, formant ainsi une requête fonctionnelle. Alternativement, une autre option serait d'utiliser un nombre pair de guillemets dans notre requête injectée, assurant ainsi le bon fonctionnement de la requête finale.

### Les différentes type de vulnérabilités : 

Effectivement, il y a diverses faiblesses liées à l'injection, telles que les vulnérabilités de type XSS, les manipulations d'en-têtes HTTP, les injections de code et les injections de commandes. Néanmoins, parmi ces vulnérabilités, l'injection SQL demeure la plus célèbre, redoutable, et est souvent privilégiée par les attaquants.

## Comment s’en défendre : 
Dans cette section, nous allons explorer la manière d'éviter ces types de vulnérabilités dans notre code et comment les corriger lorsqu'elles sont identifiées.

Garantir une gestion efficace des privilèges des utilisateurs est essentiel. Les systèmes de gestion de bases de données (SGBD) offrent la possibilité de créer des utilisateurs dotés de permissions très précises. Il est crucial de vérifier que l'utilisateur interrogeant la base de données dispose uniquement des autorisations minimales nécessaires.

L'utilisation de requêtes préparées constitue une autre mesure de sécurité. Les requêtes paramétrées intègrent des espaces réservés destinés aux données d'entrée. Ces espaces réservés sont ensuite échappés et transmis par les pilotes. Au lieu de transmettre directement les données dans la requête SQL, nous faisons usage d'espaces réservés que nous remplissons ensuite à l'aide de fonctions PHP.

Concrètement, la requête est ajustée pour incorporer deux espaces réservés, symbolisés par des « ? », où les informations du nom d'utilisateur et du mot de passe seront insérées. Ensuite, nous associons le nom d'utilisateur et le mot de passe à la requête en utilisant la fonction mysqli_stmt_bind_param(). Cette procédure permet d'éviter les problèmes liés aux guillemets et de placer les valeurs de manière sécurisée dans la requête.
