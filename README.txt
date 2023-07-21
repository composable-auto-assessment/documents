Compte-rendu du stage d'OSCAR LADRIERE pour la FSI de l'UNIVERSITE PAUL SABATIER
Sous la tutelle de Millian Poquet, IRIT, du 12 Juin au 21 Juillet.
Titre du sujet de stage:
"Implémentation de composants robustes en Rust dans l'outil d'évaluation automatique Compasse"

On été réalisés, durant la période de stage, les éléments suivants:
1- Une modification au code source de Typst (un sucesseur spirituel à LaTeX), implémentant de l'écriture de fichier (non utilisée)
2- Une seconde modification au code source de Typst, implémentant l'écriture de façon minimale, pouvant être supportée facilement.
3- Un générateur de codes QR d'identification de feuille d'examen
4- Une *template* de document en Typst, modification de la template déja présente pour compasse.
5- Un *parser* capable de lire, analyser, et noter les documents remplis

On détaille un peu plus ces composants:
1 et 2: 
On a modifié le code typst pour implémenter de l'écriture façon *log* désordonné, dans le but de proposer ce code aux responsables de typst.
Ces derniers l'ont refusé, l'écriture de fichier n'étant pas quelque-chose qu'ils désiraient.
De plus, l'écriture désordonnée aurait pu causer des problèmes pour ecrire des données structurées.

On a ainsi réimplémenté l'écriture plus simplement sous forme d'un *dump* (coté code source),
avec un *template* en .typ permettant de construire et formatter nos données.

3:
On a un programme en rust que l'on peut appeller avec les documents à hasher, le nombre de page, et l'identifiant d'examen,
et qui:
 - fusionne les contenus des documents en s'assurant que le contenu fusionné est unique pour des documents unique (ie; pas de collision)
 - hashe ce contenu fusionné en utilisant le hasher SHAKE de Sha3, qui est moderne (2015), robuste, et dont la sortie est facilement ajustable.
 - génère pour chaque page du document, un code qr contentant ce hash, le numéro d'examen, et le numéro de page.

4:
On a modifié le code .typ original de compasse, les changements principaux sont les suivants:
 - Des fonctions de gestion de données permettant la construction des dictionnaires de données requis, et leur conversion au format JSON.
 - Des modifications au format original permettant d'inclure les données à insérer dans les dictionnaires.
On a aussi fait de légères modifications au format JSON, et créé deux formats de sortie JSON (un fichier de positions (nous permettant de lire le document), et un fichier de modèle (nous permettant de noter le document))

5:
On a un programme rust qui réalise les opérations suivantes:
 A- Lecture et analyse de l'image, produisant des informations de réponse (cases cochées, en l'occurence)
 B- Utilisation des informations de réponse pour calculer des notes

  On les décrit ci-après:
  A:
  On utilise OpenCV (et une bibliothèque rust fournissant des *bindings* pour pouvoir l'utiliser)
  On fait alors:
   - Lecture et traitement préliminaire du fichier
     - Lecture
     - Transformation en niveaux de gris
     - Application de filtre bilatéral (permettant de nettoyer le bruit de fond sans perdre les informations de forme cruciales à l'analyse du code QR)
   - Lecture du code QR, vérification que le fichier est bien formé, et extraction des informations de taille et orientation à partir du code QR.
     - vérification de la compatibilité entre données QR et données JSON.
   - Localisation des marqueurs à l'aide d'un localisateur de blobs
   - Application d'une transformation affine à l'image, permettant d'obtenir une image où les éléments positionnés peuvent ètres obtenus au pixel près.
   - Analyse des cases de réponse définis, et de leurs hisogrammes, pour détecter si ils ont étés cochés.

  B:
  On applique les scores définis à chaque élément de gradation, et on réunit les éléments de gradation de façon à produire une note et un numéro d'étudiant. On applique aussi des vérifications de format, s'assurant que nos réponses sont bien formées.


