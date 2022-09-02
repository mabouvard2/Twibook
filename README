# Twibook en quelques mots
Twibook est un réseau social pour les fans de grosses cylindrées. Nous avons pour ambition de réunir les amateurs de beaux bolides à un même endroit afin qu'ils puissent échanger au sujet de leur passion avec des posts pouvant contenir du texte et une photo. Le réseau social est inspiré de Facebook et de Twitter. Ainsi, les utilisateurs pourront ajouter des posts à leur timeline et les autres utilisateurs auront la possibilité de les commenter. 

## Contexte de création du site web

### Contraintes techniques imposées

- Une limite de 1 photo par post a été fixée afin d'éviter d'avoir des photos illisibles
- Lorsqu'un utilisateur visionne la timeline de Twibook, les posts sont "réduits". Cela signifie que seul le post est visible ainsi que le premier commentaire publié sous le post
- Lorsqu'un utilisateur clique sur un post, celui-ci se "déplie". Cela signifie que l'ensemble des commentaires seront alors visibles en plus du contenu du post.

Voici un sketch montrant ce que l'on appelle un post "réduit" : 
![Post réduit](Documentation/Images/Post réduit.PNG)

Voici un sketch montrant ce que l'on appelle un post "déplié" : 
![Post déplié](Documentation/Images/Post déplié.PNG)

Voici un figma montrant les sketchs colorés et un peu plus élaborés : https://www.figma.com/file/hE6qYulNAKEUwWXrhdnJTx/Untitled?node-id=0%3A1

### Techologies utilisées

Dans le cadre du cours de "Client-Serveur" et de "Multiplateformes", seront développés : 
- Le back en JAVA - Spring Boot sous la forme d'une API
- La couche de persistance des données sera gérée par une base de données MongoDB
- Le front sera réalisé en Ionic Angular TS

## Partie client/serveur

### Architecture globale No SQL

Après avoir réfléchi aux différentes contraintes techniques que nous nous imposons pour la réalisation du projet, nous avons retranscrit cela sous la forme d'un diagramme UML. Les contraintes techniques sont explicitées grâce aux cardinalités du diagramme :

![Diagramme UML pour l'architecture de la MongoDB](Documentation/Images/Diagramme UML préliminaire.png)

Nous avons décidé de rajouter de l'incoporation dans les objets suivants : 
- Comment : Permet d'afficher le pseudo et l'utilisateur et son avatar sans avoir à faire une sous requête pour récupérer ces attributs
- Post : Permet d'afficher le premier commentaire du post sans voir à faire une sous requête pour le récupérer

Nous avons décidé d'utiliser le référencement pour séparer les posts des commentaires ainsi que pour séparer les users des posts. Cela nécessitera donc de travailler avec les jointures pour requêter les données mais cela évitera d'avoir des résultats de requêtes trop importants (un post peut potentiellement avoir un grand nombre de commentaires qui lui sont associé).

Pour la relation entre un utilisateur et ses voitures, nous auront une relation OneToFew car un utilisateur ne possédera jamais un très grand nombre de voitures. Nous avons donc choisi d'utiliser l'incorporation des voitures dans le document "user".

### Patron NoSQL implémenté

Nous avons également implémenté le patron NoSQL "Schema Versioning". Ce patron permet d'ajouter un numéro de version aux différents documents de la base. Ce numéro de version permettra de modifier les documents qui seront dans un numéro de version inférieur de la version courante dans le but qu'ils aient la même structure que les nouveaux documents insérés. Le patron facilite donc grandement les migrations en diminuant le temps d'exécution de celle-ci.

L'intégralité de nos documents ont donc un champ "schema_version" de type string qui contient la version actuelle du document.

### Requêtes de recherche implémentées

Nous avons implémenté la recherche selon : 
- L'adresse mail de l'utilisateur (utile lors de la phase de connexion)
- Le pseudo de l'utilisateur (utile lors de la phase de connexion)
- Les ids des différents composants (utile dans les différents cas de référencement)

### Diagramme de classes des entités JAVA

![Diagramme de classes pour l'architecture des entités](Documentation/Images/diagramme de classes entities java.drawio.png)

On retrouve les différents référencements (id des posts dans user et id des comments dans post)
On retrouve également l'incorporation des voitures dans la classe utilisateur


### Utilisation des indexes

Les champs email et pseudo ont été indexés avec un "Single Field Index" pour les utilisateurs car :
- Lors de la connexion, c'est le pseudo de l'utilisateur qui est utilisé pour récupérer son mot de passe hashé
- Lors de l'inscription, il faur vérifier que l'email de l'utilisateur n'est pas déjà utilisée


### Pipeline d'agrégation

Implémentation d'une pipeline d'agrégation sur les posts à partir de leur date de publication ($group, $project). Disponible dans
la classe PostController, PostService, PostRepository et PostRepositoryV2.


## Partie multiplateformes

### Packages utilisés

Pour la réalisation du projet, nous avons utilisé différents packages. Voici une liste non-exaustives des principaux packages utilisés : 
- @capacitor/camera : Permet de gérer la prise de photos mais aussi de récupérer une photo existante dans les fichiers de l'appareil
- @angular/router : Permet de gérer l'accès aux pages en fonction de si l'utilisateur est connecté ou non
- bcrypt.js : Permet de hasher le mot de passe de l'utilisateur et de le comparer pour la connexion de celui-ci

### Arborescence du projet

L'arborescence du projet ce décompose en plusieurs dossiers contenant les différentes classes de l'application Angular : 
- Auth : Contient le service permettant d'accorder ou non l'accès aux différentes pages
- Model : Contient les classes métier de l'application 
- Services : Contient les différents services qui seront injectés dans les constructeurs des différentes pages (controlleur, stub, apiclient, camera ...)
- Views: Contient les différentes pages de l'application
- Views/Components : Contient les composants graphiques utilisés par les views

### UI/UX

Nous avons voulu donner un style "tuning" à notre réseau social puisqu'il se veut centré les amateurs de grosses cylindrées et de tuning. Voici quelques captures d'écran de l'application : 

Page de connexion : 

![Post déplié](Documentation/Images/connexion.png)

Page d'inscription :

![Post déplié](Documentation/Images/inscription.png)


Page de la timeline : 

![Post déplié](Documentation/Images/timeline1.png)


Exemple de post lorsqu'il est plié : 

![Post déplié](Documentation/Images/post-plié.png)

Exemple de post lorsqu'il est déplié :

![Post déplié](Documentation/Images/post-déplié.png)


### Device API

Nous utilisons l'appareil photo du téléphone ainsi que son stockage pour pouvoir publier un post. L'utilisateur peut prendre une photo et l'intégrer à son post. La photo est par la suite convertie en base64 pour être enregistrée dans la base de données.
Pour cela, nous avons créé un service qui utilise le package "@capacitor/camera".

### Api HTTP

Notre application contacte l'API REST Java Spring Boot pour pouvoir effctuer les opérations CRUD sur la base de données. La liaison entre les deux est garantie par le service "Api-Client". Ce service crée les requêtes sur les différents endpoints de l'API et retourne des "Observable" qui pourront par la suite être consommés par le front.

L'opération DELETE est bien implémentée mais non-utilisée par le front car nous n'en avons pas l'utilité pour le moment.

Nous avons également le moyen de changer la persistence pour un stub grâce à l'injection de dépendances pour réaliser nos tests sur le front.

### Data access/storage

Utilisation du Web-basedstorage pour sauvegarder l'utilisateur connecté. Un nouvel item est donc créé lors de la connexion dans le "app-controlleur" et est détruit lors de la déconnexion dans le "app-controlleur". Le service "auth.guard" consomme cet item pour savoir si l'utilisateur est connecté ou non.

### Ce que nous aurions voulu implémenter avec plus de temps

Nous n'avons pas le temps nécessaire pour implémenter toutes les fonctionalités souhaitées.Voici une liste des fonctionalités qui pourrait apparaître dans une prochaine mise à jour : 
- Possibilité pour l'utilisateur d'ajouter une photo de profile et de modifier son profile
- Possibilité de cliquer sur un utilisateur pour voir la liste de ses voitures et son profile
- Possibilité de modifier/supprimer un post






