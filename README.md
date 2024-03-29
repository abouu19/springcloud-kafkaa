﻿# springcloud-kafka
Dans ce rapport nous allons voir comment transmettre des messages asynchrones avec Kafka
1-lancer le server Kafka : 
Pour lancer le server Kafka, on va premièrement télécharger Kafka et puis se rendre dans le dossier Kafka qui contient le dossier \bin puis taper les deux commandes suivantes :
![image](https://user-images.githubusercontent.com/98331671/173159759-faf015da-add9-450f-8268-d7fdbe659b1a.png)
Deux fenêtres vont s’ouvrir qui correspondent aux lancements du server Kafka :
![image](https://user-images.githubusercontent.com/98331671/173159785-7e8e48b5-6a1b-4a1d-b741-25a8132d2ff2.png)

![image](https://user-images.githubusercontent.com/98331671/173159792-fd4e746e-aea5-4d1f-a792-bfb5c7645d2b.png)
Maintenant, si tout se passe bien notre server Kafka est lancé ! 
2- Le producer et le consumer : 
En gros le producer permet de d’envoyer des messages dans le canal du broker d’un topic et le consumer permet d’écouter le canal. On va ouvrir une fenêtre pour le producer et une autre pour le consumer par le biais des commandes suivant : 
![image](https://user-images.githubusercontent.com/98331671/173159837-ae812228-0d06-4f0f-be76-0341934018a0.png)
Là, on a lancé un consumer dans le topic R1, donc il va écouter tous les messages qui passe par ce canal. Et un producer qui va lui envoyer des messages dans ce topic R1.
Petit essai :
![image](https://user-images.githubusercontent.com/98331671/173159859-834ebfcf-00da-4ff3-8b65-079e5d0b1062.png)
3- Créer une application spring boot qui permet d’envoyer des messages sur le broker :
On va le faire avec spring cloud function.
Crayons d’abord une class PageEvent, dont nous enverrons après son instance dans le topic du broker
![image](https://user-images.githubusercontent.com/98331671/173159886-598c7aac-94e0-4be2-9105-84c40da0468e.png)
Apres avoir créé la classe, nous allons créer une autre classe comme vous le voyez ci-dessous avec l’annotation @RestController. Dans la classe nous vous trouverez une méthode publish, qui en gros va créer une instance de la classe PageEvent et le retourner. Reste plus qu’a aller dans le navigateur et taper en respectant le chemin dans l’annotation @GetMapping () et lancer un consumer dans le topic que nous voulons envoyer.

![image](https://user-images.githubusercontent.com/98331671/173159913-c236cf3d-19b7-4841-8400-98a4b0cbd2cb.png)
Le résultat :
![image](https://user-images.githubusercontent.com/98331671/173159927-d4c9a308-8b52-4846-9224-f0500caa1976.png)
4- application spring boot consumer (qui écoute le broker)
Alors pour se faire, nous allons donc créer une par exemple une classe PageEventService avec l’annotation @Service, dans laquelle nous allons créer une méthode de type Consumer avec l’annotation @bean. Voir le code si dessous :
![image](https://user-images.githubusercontent.com/98331671/173159952-71682276-4a31-47b8-9792-e3eea821e9dd.png)
Mais pour que ça fonctionne avec notre topic R1 dans lequel nous voulons lire le message, alors dans le fichier application.properties, nous allons changer le nom du topic. Puis que par défaut le nom, c’est pageEventConsumer. Voir le code ci-dessous :
![image](https://user-images.githubusercontent.com/98331671/173159972-7eddfbc7-533b-48a2-9acc-4b4ab40f9a49.png)

Le résultat :
![image](https://user-images.githubusercontent.com/98331671/173159991-a6a6ec0a-039a-44c5-b6c5-5ee860f39483.png)
5-Producer Poller : 
Une application spring producer qui va envoyer par exemple chaque seconde des donnes sur le broker. Alors, pour se faire nous allons utiliser une fonction de type supplier. Voir le code ci-dessous :

![image](https://user-images.githubusercontent.com/98331671/173160009-0c73e201-9f51-4ec2-a6d0-94db52c99d45.png)
Pour que la méthode fonctionne, il faut changer le nom par défaut dans l’aplication.properties en choisissant notre topic par exemple R2 et ajouter la propriété a la : 
![image](https://user-images.githubusercontent.com/98331671/173160022-07f13c85-c1d0-48d3-886d-0b043e1574d5.png)
Le résultat :
![image](https://user-images.githubusercontent.com/98331671/173160049-76c95c31-8884-47c2-9112-85d9826cd740.png)
6-Consumer et Producer :
Maintenant une function qui va faire les deux, prend un input fait le traitement puit produit un output vers un autre topic. Alors pour se faire allons utiliser la méthode Function. Voir le code ci-dessous : 
![image](https://user-images.githubusercontent.com/98331671/173160067-8da0e84e-f8f7-4847-9ef4-7ab65a298ab3.png)
En gros, la méthode prend entre un input de type PageEvent, puis modifie le nom et l’user et retourne l’objet modifié.
Et bien sûr pour que ça fonctionne, on modifie les des topics par défaut et ajouter pageEventFunction a la fin :
![image](https://user-images.githubusercontent.com/98331671/173160100-b9127b79-d23c-461b-863f-b31f43d5f090.png)
![image](https://user-images.githubusercontent.com/98331671/173160108-db4e33c7-1498-49f6-a40d-ab1e3c452bc3.png)

L’entrée sur le topic R1 et la sortie sur le topic R3.
Le résultat : 
![image](https://user-images.githubusercontent.com/98331671/173160126-83ec8400-80fd-4cf2-9e81-0f7f82223029.png)
7- Kafka Stream :
![image](https://user-images.githubusercontent.com/98331671/173160148-0e2c4fe2-c52b-418c-8062-4e09be045d63.png)
![image](https://user-images.githubusercontent.com/98331671/173160159-0cd09633-68f9-4eab-980c-eb973b0c6f9e.png)
Le résultat :
![image](https://user-images.githubusercontent.com/98331671/173160177-22d9e766-2f1f-42c9-817f-aabd2a25039a.png)













