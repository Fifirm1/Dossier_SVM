# Classification des postes des basketteurs universitaires

L'objectif de ce dossier est de classer des joueurs de basket universitaire selon leur poste sur le terrain en fonction de leurs statistiques.
La base de données initiales a été trouvé sur Kaggle et regroupe les statistiques de 25 719 joueurs de basket universitaire entre 2009 et 2021 selon 8 postes dfférents.

## Analyse des données

### Suppression des variables inutiles
Nous avons supprimer toutes les variables qui ne sont pas utiles ou qui n'apportent pas d'informations pour classer les joueurs selon leur poste. En effet, des variables comme l'année, le numéro du joueur ou l'identifiant du joueur ne seront pas utile et n'apporteront aucune information pour répondre à notre problématique. 

Ainsi, les variables 'yr'(niveau d'études du joueur), 'num' (numéro du joueur), 'year'(année), 'pid' (idenatifiant du joueur) ,'type'(variable prenant uniquement la valeur 1),'pick' (place du joueur lors de la draft nba). De plus, une variabe sans nom était présente dans la base de données, elle a aussi été supprimé puisqu'aucune explication sur la variable n'était indiqué dans la base de données.

### Recodage de la variable 'ht'
Nous avons recoder la variable 'ht'(taille du joueur). En effet, les tailles étant inscrites en pied (mesure de la taille aux Etats-Unis), le format de cette variable c'est retrouvé transformer en date lors de l'importation (Un joueur mesurant 1m80 sera inscrit dans la base de données 5"11 (5 pieds et 11 pouces) et sera importé sous le format 11 Mai sur python). 

Ainsi, dans un tableur excel, nous avons récupéré toutes les valeurs que prenait la variable 'ht' et nous l'avons recodé pour que les tailles soient indiquées en centimètres au lieu des pieds et que toutes les valeurs nulles ou incohérentes soient recodées en NA (Voir détail dans le tableur excel "Recodage ht").

### Suppression des valeurs manquantes
Nous avons supprimer toutes les valeurs manquantes. En effet, certaines variables comme 'REC Rank' et 'dunksmade/(dunksmade+dunksmiss)' ont beaucoup de valeurs manquantes, nous allons donc supprimer toutes les observations pour lesquelles au moins une variable possède une valeur manquante. 

Le Rec Rank est une note attribué par la chaine de télévision ESPN sur les joueurs de basket universitaire afin réaliser un classement des meilleurs jeunes joueurs qui ont le plus de chances de devenir des stars de la NBA. Cette note n'étant attribué qu'aux joueurs les plus prometteurs, une partie des joueurs de notre base de données n'ont pas de notes et vont donc être supprimés de notre analyse.

La variable 'dunksmade/(dunksmade+dunksmiss)' comptabilise le pourcentage de dunks réussis par un joueur (Au basket, un dunk consiste à rentrer le ballon directement dans le panier en vous accrochant à l’arceau). Cependant, certains joueurs en université ne peuvent pas dunker ou ne tente pas de dunk de la saison, ils n'auront donc des valeurs manquantes dans cette variable et seront supprimés de notre analyse. 

Ainsi, après suppression des valeurs manquantes, notre base de données est composée de 11 170 joueurs pour 59 variables.

### Statistiques descriptives
Nous avons ensuite regarder les statistiques descriptives de notre base de données pour avoir un premier aperçu de nos variables. Ainsi, nous avons pu constater que notre base de données découpait les joueurs en 8 postes distincts et que le poste C étant celui le plus représenté dans notre base avec 2 607 joueurs évoluant à ce poste.

### Etudes des valeurs atypiques
Nous avons ensuite regardé si nos variables explicatives quantitatives avaient des valeurs atypiques. Pour cela, nous avons utilisé la fonction mis à disposition par François pour détecter les points atypiques. Cependant, un nombre important de variables quantitatives (17 variables sur 54) avait des valeurs potentiellement atypiques à la fois supérieures et inférieures à la moyenne et la fonction de François n'indiquait pas si les valeurs atypiques étaient les valeurs au dessus de la moyenne, en dessous de la moyenne ou les deux en même temps. 

Nous avons donc modifié les fonctions données par François afin de créer une fonction qui ne détecte que les points atypiques supérieures à la moyenne et une autre fonction qui ne détecte que les points atypiques inférieures à la moyenne. 

Pour comprendre comment nous avons modifié la fonction initiale, il faut d'abord savoir comment fonctionnait cette fonction. Elle calculait l'écart en valeur absolue entre les observations et la moyenne, ce qui permettait de prendre en compte aussi bien les valeurs supérieures qu'inférieures à la moyenne, puis triait ces valeurs par ordre décroissant et vérifiait une par une l'atypicité des observations et s'arretait dès qu'il tombait sur une valeur non atypiques. En effet, les valeurs étant triés de la plus importante à la plus faible, dès qu'il trouvait une valeur non atypiques, les suivantes étaient aussi non atypiques.

Ainsi, en ne passant pas en valeur absolue l'écart entre les observations et la moyenne, nous allons uniqumement vérifier l'atypicité des observations supérieur à la moyenne puisque le tri par ordre décroissant va placer en dernière les valeurs potentiellement atypiques inférieures à la moyenne puisqu'elles auront une différence négative. Cette nouvelle fonction nous donnera donc uniquement les valeur atypiques supérieures à la moyenne.

Et, en ne passant pas en valeur absolue l'écart entre les observations et la moyenne puis en triant les valeur par ordre croissant, nous aurons les valeurs potentiellement atypiques inférieures à la moyenne qui seront vérifier en première. Cependant, cette différence étant négative, nous allons ensuite les passer en valeur absolue pour que la fonction puisse s'effectuer correctement. Ainsi, la fonction s'arretera dès qu'elle tombera sur une valeur non atypiques et ne prendra donc pas en compte les valeurs potentiellement atypiques supérieures à la moyenne qui sont placer en dernière. Cette nouvelle fonction nous donnera donc uniquement les valeur atypiques inférieures à la moyenne.

Prenons l'exemple de la variable "Ortg" qui correspond à l'offensive rating, une statistique avancée qui calcule l'impact d'un joueur sur l'attaque de son équipe (plus la statistique est élevée, plus le joueur impacte positivement l'attaque de son équipe). Le boxplot nous montre que la variable possède des individus potentiellement atypiques supérieures et inférieures à la moyenne.
![image](https://user-images.githubusercontent.com/116641100/217602324-8d4ab105-fdb1-46d4-994e-9a8be84d1bee.png)

Lorsque nous utilisons la fontion mis à disposition par François, nous obtenons 19 valeurs atypiques avec un seuil de 44,8. Seulement, nous ne savons pas quelle partie des valeurs atypiques correspondent aux valeurs supérieures ou aux valeurs inférieures à la moyenne. 

Avec les deux fonctions que j'ai modifié à partir de la fonction initiale, nous obtenons 2 valeurs atypiques supérieures à la moyenne avec un seuil à 159,6 et 17 valeurs atypiques inférieures à la moyenne avec un seuil à 44,8.

Ainsi, nous pouvons supprimer tous les individus ayant un Ortg supérieur à 159,6 et inférieur à 44,8.

Grâce à ces trois fonctions, nous avons pu supprimer toutes les observations possédant des valeurs atypiques et notre base de données est maintenant constitués de 10 245 joueurs pour 59 variables.

### Etude des corrélations
Enfin, nous avons étudier les corrélations entre nos variables pour voir si certaines variables donnait des informations redondantes dans notre analyse.
![image](https://user-images.githubusercontent.com/116641100/217606284-23f36948-eceb-4f31-88fc-9bc06110cd59.png)

Afin de réaliser un premier tri dans nos variables, nous allons utiliser une méthode de régression pénalisée Elastic-Net. Cette méthode nous permet de supprimer les variables 'eFG', 'twoPM', 'bpm' de notre base de données.

Cependant, après réalisation de ce tri, il reste toujours un nombre important de variables explcatives fortement corrélées entre elles.
![téléchargement](https://user-images.githubusercontent.com/116641100/217613219-3670e23d-651a-492c-ba8d-b4d5a2ec2a93.png)

Ainsi, nous remarquons que les couples de variables FTM & FTA, TPM & TPA, rimmade & rimmade+rimmiss, midmade & midmade+midmiss, dunksmade & dunksmiss+dunksmade sont fortement corrélées entre elles puisqu'elles comptabilise le nombre d'essai d'un type de shoot et le nombre de succès de ce même type de shoot. 

Sachant que nous avons les pourcentages de réussite de chacun de ces types de shoot, nous allons supprimer l'ensemble de ces variables puisque leur information est déjà comprise dans la variable en pourcentage.

Le pourcentage de minutes joués par un joueur (Min_per) est fortement corrélés aux nombres de stops effectués par ce dernier ainsi qu'à son nombre de matchs joués. Etant donné que les variables stops et matchs joués (MP) ne sont pas corrélés entre elles, nous allons simplement supprimer la variable Min_per de notre base de données.

Les couples de variables obpm & ogbpm ainsi que dbpm & dgbpm mesure sensiblement la même information et sont donc fortement corrélés. Le calcul des variables obpm et dbpm étant nettement plus simple et captant la même information, nous allons supprimer les variables ogbpm et dgbpm de notre base de données.

Les variables dreb et treb sont fortement corrélés. Etant donné que nous avons la différence entre les rebonds offensifs et défensifs, le nombre total de rebond nous donne une information redondante. Nous allons donc supprimer la variable treb de notre base de données.

Les variables AST_per et ast sont aussi fortement corrélés. Ces deux variables nous donne une information similaire, le nombre de passes décisives effectués par un joueur. Nous allons donc garder les valeurs brutes de passes décisives au lieu du ratio de passes décisives d'un joueur par rapport au nombre de passes décisives totales de l'équipe. La variable AST_per sera donc supprimée de notre base de données.

La variable adjoe est fortement corrélés à la variable ogbpm. Cependant, comme nous supprimons déjà la variable ogbpm de notre base de données nous pouvons garder la variable adjoe.

![téléchargement (1)](https://user-images.githubusercontent.com/116641100/217615218-55a2b66f-0305-4cd7-a525-3e8897ef1188.png)
Après avoir supprimé ces variables de notre base de données, nous observons qu'il n'y a plus de problème de multicolinéarité dans notre base de données. Nous allons donc passer à la modélisation avec une base de données composée de 10 245 joueurs pour 41 variables dont 3 ne sont pas utilisées car elle donne le nom, l'équipe et la conférence du joueur et 1 correspond à la variable à expliquer. Ainsi, notre base comprend 37 variables explicatives.

## Modélisation

### Séparation du jeu de données en Train/Test
Tout d'abord, nous séparons notre base de données en deux bases distinctes : une base d'entrainement comprenant 8 196 observations et une base de validation comprenant 1 903 observations. Puis nos standardisons nos variables afin qu'elles soient toutes sur la même échelle.

### Classification Multiclasse
Afin de réaliser une classification multiclasse, nous allons utiliser un SVC avec la méthode OVO (One Versus One) et avec la méthode OVR (One Versus Rest). 

#### Classification One Versus One (OVO)
Avec la méthode OVO, nous obtenons la matrice de confusion suivante :
![Capture d’écran 2023-02-09 à 17 07 55](https://user-images.githubusercontent.com/116641100/217869249-448d565a-3b98-46ee-88b0-ced7837e9019.png)

Pour les joueurs jouant purement à un poste extérieur (avec la lettre G dans leur poste), on remarque que le modèle prédit plutôt bien leur poste puisque le taux de précision est compris entre 80 et 90%.

De même pour les joueurs C qui ont une bonne prévision avec 92% de bonnes prédictions.

Les joueurs Pure PG sont moins bien prédits que les autres G (82% de bonnes prédictions), cela est surement du au faible nombre de Pure PG dans notre base de données qui vont fausser notre analyse. En effet 18% des Pure PG sont classés en Scoring PG alors que seulement 2% des Scoring PG sont classés en Pure PG.

Il semble plus difficile de prédire des joueurs ayant les postes Stretch 4 et PF/C car leurs taux de prédictions est de 65% pour les premiers et de 72% pour les deuxièmes.Cela peut s'expliquer par le fait que ces postes sont très proches en termes d'impact sur le jeu et sont composés de joueurs très polyvalents qui seront donc plus difficiles à catégoriser.

En effet, on remarque que 14% des Stretch 4 sont prédits en Wing F et 20% en PF/C. De même avec les PF/C qui sont prédit à 12% comme des Strecth 4, à 7% comme des Wing F et à 10% comme des C. Enfin les Wing F sont plutôt bien prédit avec un taux de précision de 80% mais 10% des Wing F sont prédits comme des Wing G ou des PF/C par notre modèl

#### Classification One Versus Rest (OVR)
Nous allons maintenant regarder le matrice de confusion avec la méthode OVR : 
![Capture d’écran 2023-02-09 à 17 08 29](https://user-images.githubusercontent.com/116641100/217869870-4c8eb563-ab91-4e49-985c-a4d2bff8f21c.png)

La méthode OVR nous donne des prévisions nettement moins bonnes que la méthode OVO.
										
En effet, les Wing F et les Stretch 4 sont correctement classés qu'avec respectivement 9% et 20% de précision.

De plus, tous les Pure PG sont classés comme des Scoring PG et les Combo G ne sont classés correctement qu'une fois sur deux.
On remarque une difficulté pour notre modèle à faire la différence entre un Combo G et un Wing G (32%), entre un Scoring PG et un Combo G (28%), entre un Wing F et un Wing G ou un PF/C (62% et 19%), entre un Stretch 4 et un Wing G ou un PF/C (30% et 40%) et entre en PF/C et un C (18%).

Ce résultat montre que, comme dit auparavant, la disticntion entre Wing G, Wing F, Stretch 4 et PF/C est assez fine puisque les joueurs composant ces classes sont très polyvalents et sont donc généralement capables de joués à l'ensemble de ces différents postes. En effet, les postes n'étant pas fixes au basket, les joueurs jouant à ces postes vont changer de catégorie au cours de la saison en fonction des différentes oppositions, ce qui va compliquer notre analyse.

#### Réseau de neurones
Nous allons maintenant réaliser un modèle de réseau de neurones afin de voir s'il est possible d'améliorer notre classification.

Cependant, lorsque nous avons voulu effectuer le réseau de neurones, celui-ci n'a pas abouti et les résultats que nous obtenions étant étranges. En effet, notre learning curve n'est pas utilisable car elle ne montre pas de signes d'apprentissages de la part de notre réseau de neurones. 
![image](https://user-images.githubusercontent.com/116641100/217873531-0e6fdd47-67c7-437a-971c-f396037a4729.png)

Malgré les différents essais en changeant les valeurs des hyper-paramètres, en changeant le nombre d'epochs ou encore en ne cherchant pas à utiliser un GridSearch pour optimiser les hyperparamètres et en construisant une réseau de neurones simples, nous n'arrivons pas à obtenir un résultat satisfaisant.

Ainsi, nous avons décider de ne pas utliser de réseau de neurones pour notre classification puisque ceux-ci ne nous donnent pas de résultats utilisables.

#### Concluson classification multiclasse
Pour conclure sur notre classification multiclasse, nous pouvons dire que les résultats de la méthode OVO sont plutôt satisfaisants puisque la plupart des postes sont bien prédits et seules les postes assez similaires (Stretch 4 et PF/C) ont des résultats plus décevants mais tout de même acceptable avec environ 70% de bonnes prédictions.

De plus, la répartition des effectifs dans les différentes classes sont assez hétérogènes, ce qui peut poser des problèmes pour bien définir les différences entre les postes et donc bien les classer. En effet, notre base de données ne comporte que 114 joueus évoluant au poste de Pure PG contre 2 343 joueurs évoluant au poste de Wing G. 

Ainsi, cette répartition inégale à entrainer des problèmes dans ntre classifiction puisqu'aucun Pure PG n'est bien prédit dans la modélisation OVR contre 80% des Wing G dans cette même modélisation.
![Capture d’écran 2023-02-09 à 17 34 17](https://user-images.githubusercontent.com/116641100/217877837-27e31f2d-dac4-4e66-a9af-ce24549f5bc4.png)

### Classification binaire

#### Création de la variable binaire
Nous allons donc créer une nouvelle variable qui prend la valeur 1 lorsque le joueur va jouer à un poste dit 'extérieur 'et 2 lorsque celui-ci va jouer à un poste dit 'intérieur'.

Parmi les 8 postes inclus dans notre base de données, nous pouvons définir 5 postes correspondant aux postes extérieurs (Combo G, Pure PG, Scoring PG, Wing G et Wing F) et 3 postes correspondant  aux potes intérieurs (Stretch 4, PF/C et C). On notera cependant 2 postes que le qualifiera "d'hybrides" correspondant à des postes où les joueurs vont être considéré intérieurs et extérieurs en fonction des circonstances (Wing F étant plus axé extérieur mais pouvant jouer comme un intérieur sur certaines phases de jeu et Stretch 4 étant plus axé intérieur mais pouvant jouer comme un extérieur sur certaines phases de jeu).

Après avoir recodé notre variables, nous obtenons 5 636 joueurs que nous classons comme des extérieurs et prenant la valeur 0 et 3 879 joueurs que nous classons comme des intérieurs et prenant la valeur 1.
![Capture d’écran 2023-02-09 à 17 34 35](https://user-images.githubusercontent.com/116641100/217878120-59a1e8cf-6cf6-462d-9679-b47729c006d1.png)

#### Recherche du meilleur modèle
Pour cette classification binaire, nous allons utiliser 4 modèles différents que nous allons comparer afin de définir le modèle le plus approprié pour notre analyse. Ces 4 modèles sont : la régression logistique, le SVM, le SVM linéaire et le SGDClassifier.
![image](https://user-images.githubusercontent.com/116641100/217879444-d22f638a-6350-4a67-b851-0943aefe89b5.png)

![Capture d’écran 2023-02-09 à 17 38 38](https://user-images.githubusercontent.com/116641100/217879373-87ce5a31-327c-4b2d-83de-dd0f487d13fe.png)

Après comparaison de nos différents modèles, le SVM linéaire ressort comme étant le plus efficace puisqu'il à l'accuracy la plus grande et un écart-type très faible.

#### SVM Linéaire
##### Matrice de confusion
Après avoir optimiser les hyperparamètres de notre modèle et entrainer le modèle avec la base d'entrainement, nous avons réaliser la matrice de confusion pour voir la qualité de notre modèle :
![image](https://user-images.githubusercontent.com/116641100/217881518-8acd9552-3a42-493b-a443-51d32ec2a35d.png)

Le modèle ne commet quasiment aucune erreur de prévision, ce qui est logique puisque nous avons classé à la main les joueurs extérieurs et intérieurs et que la distinction entre ces deux classes est beaucoup plus simple qu'entre les 8 postes.
![Capture d’écran 2023-02-09 à 17 49 02](https://user-images.githubusercontent.com/116641100/217882036-a3735d9d-2db7-4637-91e0-f50f0f9a9544.png)

Le modèle prédit correctement 97,4% des joueurs dans notre base d'entrainement et 97,1% des joueurs dans notre base de validation. Ces deux valeurs étant très élevées et surtout très proches, nous pouvons penser que notre modèle à un problème de sur ajustement. Cependant, notre base étant composé de plus de 10 000 joueurs diviser en seulement deux classes et la différence statistiques entre les postes intérieurs et extérieurs au basket étant assez simple à observer, notre modèle à pu facilement classer les joueurs et donc possède un taux de bonne classificaton très élevée sans pour autant souffrir de sur ajustement.

##### Importance des variables
Enfin, nous pouvons nous intéresser à l'importance des variables dans la classification.
![image](https://user-images.githubusercontent.com/116641100/217883399-66c9adc3-3390-476f-805c-5be05c401779.png)

Nous remarquons que la taille (ht) est la variable qui influence le plus la classe 1. En effet, au basket, le principal facteur qui décide si un 
joeur va jouer à un poste intérieur ou extérieur est la taille puisqu'un joueur de grande taille aura plus de faciliter à marquer près du panier alors qu'un joueur plus petit va être plus à l'aise pour dribbler et va donc jouer à un poste extérieur.

Ensuite, les variables blk_per, TS_per et ORB_per, qui représente respectivement le ratio de contres d'un joueur par rapport au nombre de contres total de l'équipe, le pourcentage de tirs réussis par le joueur et le ratio de rebond offensif pris par un joueur par rapport au nombre de rebond offensif pris par l'ensemble de l'équipe, vont aussi fortement impacter notre modèle. Encore une fois, ces 3 variables sont fortement liées au poste d'intérieur puisque ces joueurs jouant proche du panier, ils ont tendance à effectuer plus de contres, avoir une plus grande réussite au tirs et prendre plus de rebond.

A l'inverse, la variable TP_per, qui représente le ratio de 3 points marqués par un joueur par rapport au nombre de 3 points marqués par l'ensemble de l'équipe. Les joueurs intérieurs n'étant que peu souvent placé autour de la ligne à 3 points, il semble logique que la plupart des 3 points marqués par une équipe soit de l'oeuvre des extérieurs.

Aussi, les variales twoP_per, adjoe et stl_per, qui représente respectivement le ratio de 2 points marqués par un joueur par rapport au nombre de 2 points marqués par l'ensemble de l'équipe, l'efficacité offensive d'un joueur et le ratio d'interceptions d'un joueur par rapport au nombre d'interceptions total de l'équipe, vont aussi influencer notre modèle. Les joueurs extérieurs étant souvent des joueurs marquant beuacoup de points par rapport aux intérieurs, les variables twoP_per et adjoe vont donc logiquement impacter fortement le fait d'être un joueur extérieur. 

De plus, comme dit précèdement, les joueurs extérieurs vont plus souvent dribbler que les joueurs intérieurs, ce qui va logiquement entrainer un nombre d'interceptions plus important pour ces joueurs, du au fait qu'ils en ont plus souvent l'occasion.
![Capture d’écran 2023-02-09 à 18 21 06](https://user-images.githubusercontent.com/116641100/217889713-8f6845ed-cbb7-4ada-bc9c-62bf40b15bfb.png)

