# TP n°2 Administration Système

## Variables d'environnement

1- Afin de trouver l'ensemble des dossiers bash contenant les commandes tapés par l'utilisateur, on cherche à afficher le contenu de la variable d'environnement *PATH*. Dans un premier temps, il est important de noter la différence entre variable d'environnement et variable local. Une variable est dite locale lorsqu'elle n'existe que dans le terminale (ou bash) courrant. A l'inverse, une variable d'environnement, comme son nom l'indique existe au travers de l'ensemble des bashs instanciés par l'utilisateur, quelques soit leur nombre.

Par la suite, pour revenir à la question d'origine, on tappe la commande : *printenv PATH* . Le résultat affiché à l'écran est :

*/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin*

2- La variable d'environnement associée au répertoire personnel est *HOME* .

Commandes :

* *cd $HOME* => retourne dans repertoire perso
* *pwd* => permet de s'en assurer en affichant l'emplacement du répertoire courant

3- Les variables explicitées sont :
* *HOME* => contient la location du répertoire personnel de l'utilisateur
* *LANG* => définie la langue par défaut
* *PWD* => pointe vers le répertoire courant (sans argument, equivalent à *pwd*)
* *OLDPWD* => contient le répertoire précèdent la dernière commande *cd* ( équivalent à *cd -*)
* *SHELL* => contient la localisation de l'interpréteur de commande
* *_* => contient la dernière variable d'environnement éxécutée

**Note** : *printenv | grep __VAR__* => recherche une variable (locale/d'environnement)
qui contient la chaîne de caractère **VAR**. Par exemple *__MYVAR__ = toto* .

4- Commandes :
* Création d'une variables locale : *MY_VAR="abc"*
* Vérification de son existance : *printenv | grep MY_VAR*

5- On observe que, après avoir tapé la commande bash (bash est un interpréteur de langage de commande sh-compatible qui éxecute les commandes depuis l'entrée standard ou depuis un fichier), la variable locale MY_VAR n'existe pas, en accord avec l'explication explicitée en 1- .

6- En revanche une variable d'environnement existe même après avoir taper la commande *bash*. Pour créer une variable d'environnement, on tappe la commande *export MY_VAR2=tototo* ou *declare -x MY_VAR=tototo* .

7-Il est possible de créer aussi des variables d'environnement avec un espace, en procédant de la manière suivante :

* *export NOMS="tom ludo"* => création de la variable d'environnement avec un espace
* *printenv | grep NOMS* => vérification de l'existance de cette même variable

8- Il possible, au travers de la commande *echo*, d'afficher des chaînes de caractère concaténée, et même de concaténer des chaînes de caractères avec des variables contenant des chaînes de caractères. Par exemple : 

* *echo bonjour a vous deux, $NOMS* => affiche "bonjour a vous deux tom ludo"

9- Pour supprimer une variable, on utilise la commande *unset*. De ce fait, on note, par exemple : *unset NOM* . Il est importaant de noter que la création d'une variable "vide" et la suppression de cette dernière est totalement différent du point de vue du système. La création d'une variable engendre un lien entre cette dernière et une case mémoire : la variable crée prend une place respective dans la mémoire du système (même si valeur est vide). a suppression d'une variable, quant à elle, engendre la libération d'une case mémoire du sytème (en soit, de la "place" est libéré en mémoire du système).

10- Commande:

* *echo '$HOME'=$HOME* => permet d'afficher $HOME =/home/tomludo (répertoire personnel)

## Scripts

### Exercice n°2

Enoncé : Écrivez un script "testpwd.sh" qui demande de saisir un mot de passe et vérifie s’il correspond ou non aucontenu d’une variable *PASSWORD* dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’aﬀicher.

**Script :**

#!/bin/bash                       #permet de commencer un script bash

read -s -p  'Entre mdp:' mdp      #demande à l'utilisateur de rentrer un mot de passe

if [ $mdp = "password" ]; then    #vérification si le mot de passe est bon

	echo "mdp correct"
  
else

	echo "mdp incorrect" 
  
fi    #permet de fermer une boucle "if"


### Exercice n°3

Enoncé : Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel. Il aﬀichera un message d’erreur dans le cas contraire.


**Script :**

#!/bin/bash

function is_number

{       

	re='^[+-]?[0-9]+([.][0-9]+)?$'     #identification de la signature d'un nombre réel

	if  [[ $1 =~  $re ]] ; then        #test si le premier argument de la fonction est un réel
  
		echo  "reel"
    
		return 1
    
	else
  
		echo "error"
    
		return 0
    
	fi
  
}

read -p 'Saisir un nombre réel' a     #saisi du nombre par l'utilisteur

echo "  on a $( is_number $a ) "


### Exercice n°4

Enoncé : Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il aﬀiche le message : ”Utilisation :nom_du_scriptnom_utilisateur”,où *nom_du_scriptest* le nom de votre script récupéré automatiquement (si vous changez le nom de votrescript, le message doit changer automatiquement).


**Script :**

#!/bin/bash

ctrl_usr () {


$(cut -f1 -d: /etc/passwd)

for us in $(ls)

do

	if  [[ $1 = $us ]] ; then
  
		echo "usr exist"
    
		return 1
    
	else 
  
		echo "usr don't exist"
    
		return 0
    
	fi
  
done

}
$( ctrl_usr $us)



