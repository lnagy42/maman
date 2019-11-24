# OBJECTIFS
* Comprendre l'organisation de fichiers linux et les utilités des repertoires importants dans le systeme de fichier.
* Apprendre a vous reperez et a maitriser votre environnement shell
* Savoir agir dans votre environnement shell
* Referencer des fichiers sous differents noms
* Cheat Sheet des commandes basiques pour multifichiers sur bash

# I/ Explication filesystem + points importants a savoir
    
## 1. termes a savoir

        Filesystem : designe l'organisation hierarchique des fichiers au sein d'un systeme d'exploitation

Ces fichiers sont organisés en ce qu'on appelle une arborescence inversée.
On part du dossier racine `/` pour descendre dans les branches de repertoires et sous repertoires.
WARNING : `/` est aussi utilisé dans les noms de fichiers ou dossiers, il indique le chemin d'accès au fichier. ex : `/usr/bin` indique que le repertoire `bin` est dans le repertoire `usr` qui est lui même dans le repertoire `/`.
Detail : les espaces dans les noms de fichiers peuvent entrainer lors de certaines commandes, de mauvaises interpretations de la commande en confondant une partie du nom avec un argument ou une option. Preferez des noms sans espace ou mettez des guillemets autours du nom.

Notes : termes importants de descriptions de contenu fichiers ou repertoires
- statique : le contenu ne change que s'il est modifié
- dynamique ou variable : le contenu est modifier par des processus actifs
- persistant : le contenu ne change pas meme au demarrage de l'ordinateur (fichiers de configurations)
- execution : le contenu est specifique au processus ou au systeme, supprime lors d'un redemarrage

    2. directories importants

/usr : ontient des fichiers exécutables et annexes pour de nombreux logiciels 
    considérés comme faisant partie de la distribution concernée.
    /usr/bin : commande user ??
    /usr/sbin : commandes adminsys ??
    /usr/local : logiciels personnalisé localement ??

/etc : Fichiers de config du systeme

/var : Donnees variables du systeme qui doivent persister au demarrage et
 les fichiers qui changent dynamiquement genre cache, dylibs ...

/run : donnees d'executions des processus démarré depuis le dernier demarrage. 
    Contenu du dir recréer au demarrage. Contient les fichiers d'identifications 
    et de verrouillage des processus

/home : Tous les repertoires personnels de lusr
/root : Repertoire personnel du superusr root
/tmp : Pour les fichiers temporaires. Delete files non modifiés/ouverts 10 apres.
    /var/tmp les supprimes 30j apres
/boot : Fichiers necessaires au demarrage
/dev : Fichiers periphériques speciaux (drivers). Utilisé par l'ordi pour acceder au materiel

**Notes** : les repertoires suivants sont des liens symboliques vers les repertoires correspondants :
    - /bin et /usr/bin
    - /sbin et /usr/sbin
    - /lib et /usr/lib
    - /lib64 et /usr/lib64

## II/ Tout savoir sur les paths

### 1. chemin relatif vs absolu
> **ATTENTION AUX FAUTES DE FRAPPES**

    Répertoire initial : correspond à l'emplacement initial des processus systèmes

    Chemin absolu : correspond à l'emplacement exact des fichiers dans la hierarchie systeme en commencant par la racine `/`. ex : /usr/bin ou /var/log

    Chemin relatif : correspond à l'emplacement d'un fichier depuis l'endroit où l'usr se situe.
    ex : l'utilisateur est dans son dossier /home qui contient le dossier Bureau où se situe le fichier photo.jpg. Le chemin pour accéder à la photo est Bureau/photo.jpg et non pas /home/Bureau/photo.jpg qui lui est le chemin absolu.

    ATTENTION : l'ensemble des linux ont un systeme de fichier sensible a la casse mais ce n'est pas le cas de microsoft ou d'apple.


### 2. les commandes de navigation

* `pwd` : permet de se localiser dans le systeme de fichier

* `ls` : liste le contenu du repertoire specifié si rien n'est specifié, liste le contenu du répertoire actuel.
Options principales :

    * `ls -R` : liste tous les repertoires et sous repertoires
    * `ls -l` : liste tous les fichiers avec plus de details sur eux (date de creations, tailles, appartenance ...)
    * `ls -a` : liste tous les fichiers meme les fichiers cachés (.nomdefichier)

> Remarque : le repertoire `.` deisgne notre repertoire actuel et `..` designe notre répertoire parent.

* `cd` : permet de se deplacer dans le systeme de fichier à partir de notre emplaement actuel. Si rien n'est spécifié, cela nous deplacera dans notre *répertoire initial*
    * `cd ..` : nous deplace dans le repertoire parent au notre.
    * `cd - ` : nous deplace dans le repertoire précédent
    * `cd . ` : ne nous deplace pas. le `.` signifie que c'est notre repertoire actuel

* `touch` : 
    * permet de mettre a jour l'horodatage d'un fichier sans avoir a le modifier. Pratique pour des fichiers vides qui peuvent servir d'exercices par exemple.
    * permet de créer des fichiers

# III/ Gestions de fichiers
# 1. les commandes de gestions de fichiers

* `mkdir` : créer un ou plusieurs répertoires
    > `mkdir Projet1 Projet2`

    Elle échoue lorsque le repertoire en argument existe deja ou si on essaye de créer un sous repertoire dans un repertoire qui n'existe pas. L'option `-p` permet de creer les repertoires parents manquants.
**ATTENTION AUX FAUTES DE FRAPPES**
* `cp` : permet de copier un fichier dans un emplacement specifié. Attention, si le nom de fichier existe deja, `cp` le remplace definitivement.
Par defaut, `cp` ne copie par les repertoires.
    * `cp -r` : copie les repertoires
    * `cp -f` : force copie le fichier en ignorant les erreurs (a utiliser avec précaution)
    * `cp -r` : copie un repertoire et son contenu dans un autre repertoire
* `mv` : permet de deplacer des fichiers d'un emplacement a un autre. Si on deplace le fichier au meme endroit avec un nom different, cela renommera le fichier avec le dernier argument. 
```
$ mv Projet/projet1.c Projet/projet2.c
$ ls Projet
 projet2.c
```
* `rm` : permet de supprimer un fichier. Par defaut, `rm` ne supprime pas les repertoires.
**ATTENTION : `rm` supprime definitivement les fichiers ou repertoire. Il n'y a pas de retour arriere.**
    * `rm -r` : permet de supprimer des repertoires et leur contenu
    * `rm -ri` : permet de demander une confirmation de suppression pour chaque contenu
    * `rm -rf` : permet de forcer la suppression de chaque sous dossier et son contenu sans demander la permission.
    **ATTENTION : Dans `rm -rif`, l'option `-f` est prioritaire et supprimera tout sans confirmation**

* `rmdir` : permet de supprimer un dossier vide

### 2. liens symboliques / liens materiels

 **1/ Lien materiels** :
    
        Permet de donner plusieurs noms/chemin d'accès, à un même fichier en pointant sur un numéro de fichier, (en interne Linux enregistre les fichiers sur la base d'un numéro appelé "inode" et pas sur la base d'un nom). Un fichier peut donc avoir plusieurs noms,et existera tant qu'il reste au moins un fichier lié au numero du fichier original.
        Toute modification faite au fichier original entraine une modification de tous les fichiers liés.
       
* Limites : 
    * Contrairement aux liens symboliques, ils ne peuvent pointer que vers un autre élément du même système de fichiers. `df` permet de voir les differents repertoires sur les differents systemes de fichier.
    * Ils ne peuvent pas pointer sur un repertoire

Creation du lien symbolique :
```
$ ln text1.txt /tmp/txt2.txt
$ ls -l
-rw-rw-r-- 2 cedric cedric 27 2011-11-11 08:22 text1.txt
-rw-rw-r-- 2 cedric cedric 27 2011-11-11 08:22 tmp/txt2.txt
```
Le numero 2 derriere les fichiers indique qu'ils ont 2 liens physiques.
```
$ ls -li
667473 -rw-rw-r-- 2 cedric cedric 27 2011-11-11 08:22 text1.txt
667473 -rw-rw-r-- 2 cedric cedric 27 2011-11-11 08:22 tmp/txt2.txt
```
`ls -li` permet de verifier que le numero d'inode est le meme pour chaque fichier et qu'ils sont liés en dur si c'est le cas.


**2/ Lien symboliques** :
        
        Permet d'attribuer un autre chemin d'accès à un type de fichier special qui pointe sur un nom de fichier existant. Contrairement aux liens materiels, ils peuvent lier des fichiers sur deux systemes de fichier differents.
        Ils peuvent egalement pointer sur un repertoire.
    
* Limites :
    * Si le fichier original est supprimé, le lien est detruit et indique "lien symbolique non resolu" si on le `ls -l`


# IV./ CheatSheet Filtrage et gestion d'extensions
Pour pouvoir agir sur plusieurs fichiers en meme temps, il existe plusieurs fonctionnalités dans le bash permettant differentes actions sur les fichiers. 

### 1. Le "globbing"

        Ou globalisation est une fonction de recherche du shell qui utilise les metacaracteres qui s'étendent pour correspondre a des ensembles de noms de fichiers.
Filtrage par motif : 

* Metacaracteres :
    * **`*`** : permet de representer soit un caractere, soit aucun caractere, soit un ensemble de caractere dans une string.
    * **`?`** : permet de representer un seul caractere
* Classes de metacaracteres :

| Motif       | Correspondance                                                                    |
|-------------|-----------------------------------------------------------------------------------|
| [abc...]    | N'importe quel caractere de la classe entre crochets                              |
| [!abc...]   | Tout caracteres *non* compris dans la classe entre crochets                       |
| [^abc...]   | Tout caracteres *non* compris dans la classe entre crochets                       |
| [[:alpha:]] | Tout caractere alphabetique                                                       |
| [[:lower:]] | Tout caractere minuscule                                                          |
| [[:upper:]] | Tout caractere majuscule                                                          |
| [[:alnum:]] | Tout caractere alphabetique ou numerique                                          |
| [[:punct:]] | Tout caractere imprimable n'etant ni un espace ni un alphanumerique               |
| [[:digit:]] | Tout caractere entre 0 et 9                                                       |
| [[:space:]] | Tout caractere d'espacement unique. Donc espace, retour chariot, tabulations etc. |


# Conclusion

Vous avez appris le fonctionnement global d'un systeme de fichier et comment creer, manipuler et supprimer les fichiers et repertoires a l'interieur du systeme de fichier.
Vous avez egalement apris a creer des liens entre les noms.