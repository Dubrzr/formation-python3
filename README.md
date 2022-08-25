# Intro

## Histoire de python

* python2, python3 (2008), python3.X

Créé par Guido van Rossum (pays bas) en 1991, il devient "Benevolent Dictator for Life" sur le projet, employé de 2005-2012 par Google pour bosser sur Python (puis passe à dropbox de 2013 à 2019, puis depuis 2020 à M$)


## Théorie sur les versions, les packages et les environnements python

* apt install python3" "python3 --version"
* pypi & pip (installation de pip "apt install python3-pip" ou https://pip.pypa.io/en/stable/installation/)

### Environnements virtuels

Si tu utilises le pip/poetry de ta machine pour installer des packages, alors ils seront disponibles pour tout le systèmes/tous les users, cependant si sur cette machine il y a plusieurs projets python, il vaut mieux créer un environnement virtuel par projet et y installer les packages de chaque projet :

source /home/$USER/tmp/venv/bin/activate 
pip install requests
pip install --proxy http://proxym-inter.aphp.fr:8080 requests

  * conda (venv+packages système)

#### virtualenv venv

Création d'un virtualenv (ou venv pour les intimes) : python3 -m venv /home/$USER/mon/path/venv (possiblement besoin de apt install python3-venv -> c'est une lib standard de python pas installée par défaut)

"Rentrer" dans un venv : source /home/$USER/mon/path/venv/bin/activate

"Sortir" d'un venv : deactivate

Une fois rentré dans un venv, tu peux y installer des packages (avec pip/poetry), on peut checker aussi quelles packages sont déjà présents avec pip freeze, d'ailleurs même si tu utilises poetry tu dois pouvoir lister les packages existants avec pip freeze

Note: il y a d'autres packages pour créer des env. virtuels, virtualenv est très populaire par ex., cf https://stackoverflow.com/a/41573588/3157230) mais la lib standard venv est suffisante

#### Conda

Conda permet aussi de gérer des environnements virtuel avec conda create --name myenv

Les différences entre venv et conda :

* venv ne s'occupe de gérer que des packages python, si un package python a une dépendance sur un package système, alors il faut installer le package système sur le système avec sudo/root
* conda comme venv permet de gérer des packages python, mais permet aussi de gérer des packages système dans l'environnement virtuel
* conda a son propre repository officiel alimenté uniquement par conda, et met à dispo des repositories public (conda-forge) que les gens peuvent alimenter. Les packages conda sont différents car ils contiennent toutes leurs dépendances (python et système)
* conda a un système de résolution de packages ultra complexe qui résout à la fois les dépendances python et les dépendances système -> d'où la réputation de conda pour sa lenteur extrème à la résolution des conflits de versions 
* on ne choisira conda par exemple que pour des cas rares d'utilisateurs qui travaillent sur le même serveur ou qui n'ont pas le droit d'installer des packages système

# Environnement de développement

## Setup de Pycharm

1. Setup de Pycharm - debug dans pycharm - l'onglet tools proposer d'ajouter une rentrée sur les bureau ubuntu
2. main et PEP8

if __name__ == "__main__":
    print("Hello world!")

Attention à l'indentation, utiliser 2 ou 4 spaces pas de tab (rester cohérent à travers toute les développements), python a une coding style : https://peps.python.org/pep-0008/

# Le langage 

## Fonctions python

def ma_fonction(arguments):
    truc_muche

Attention au naming, pas de CamelCase pour les variables et fonctions : seulement du snake_case, exemple :

nom_de_variable
nom_de_fonction


## Modules python

Considérer l'archi suivante :

- main.py
- a/__init__.py (vide)
- a/utils.py
- b/truc.py

Avec main.py contient : 

from a.utils import hello_world
from b.truc import bidule

print(hello_world('Coucou') + bidule())

Avec a/utils.py contient :

def hello_world(arg):
    return arg + ": Hello world"

Avec b/truc.py contient : 

def bidule():
    return 'Wesh'


Attention au naming, toujours du snake case, utiliser le underscore vraiment que si ça améliore clairement la lisibilité, sinon faire sans.

modulename
module_name

## Syntaxe list, dict et arguments de fonction

a = [1,2,3,4,5]
c = ['a','a',"a","a",a]

d = """
igjeirjgierjd
ger
gerg
ergerg
gezgzeg
"""

b = {
	'a': 1,
	'b': 2,
	'c': 3,
    'd': 4,
    'e': 5,
}

z,b,c,d,e = *a

def fun(a,b,c,*args, d=42,e=None, **kwargs):
    print(args)
    print(kwargs)
    print(a,b,c,d,e, args)
    print(a,b,c,d,e, *args)

fun(1,2)
fun(1,2,3)
fun(1,2,3,e=7,d=9)
fun(1,2,3,*a)
fun(**b)


## For, comprehension lists, cast

a = [1,2,3,4,5]

for element in a:
    print("J'ai trouvé " + str(element) + "! Amazing!")

for index, element in enumerate(a):
    print("J'ai trouvé " + str(element) + " à l'indexe " + str(index))


b = {
	'a': 1,
	'b': 2,
	'c': 3,
    'd': 4,
    'e': 5
}

for k in sorted(b.keys()):
    print(b[k])

!for key, value in b.items():
    print(key, value)

for idx, key in enumerate(b):
    val = b[key]
    print("Pour la clé numero " + str(idx) + " qui vaut " + key + " j'ai trouvé la valeur " + str(val))

result = [k + str(v) for k, v in b.items()]
result = [k for k in b.keys()]
result = [str(v) for v in b.values()]

result = [(k, str(v)) for k, v in b.items()]

result = {k + 'a': v + 10 for k, v in b.items()}



## print format (depuis python3.6), single quotes, double quote, triple quotes, et join

x = "une string"
y = {"latitude": 45, "longitude": 2}
z = ['ah', 'oui', "j'aime"]

print("x vaut {x}")
print(f"x vaut {x}")
print("lat={0} lon={1}".format(y['latitude'], y['longitude']))
print("lat={latitude} lon={longitude}".format(**y))


## Classes et asserts

class MaClasseALaClasse:
    pass

obj = MaClasseALaClasse()

class MaClasseALaClasse(CLasseMere):
    def k(self):
        print(self.__dict__)
    def __init__(self, *args, **kwargs):
        for idx, elem in enumerate(args):
            setattr(self, str(idx), elem)
        for idx, key in enumerate(kwargs):
            setattr(self, key, kwargs[key])
        super().__init__(self, *args, **kwargs)

    def describe_yourself(self):
        for attribute, value in vars(self).items(): # OU in self.__dict__.items()
            print(f"self.{attribute} = {value}")
    def static_method():
    	return 3
    def __repr__(self):
    	return self.__dict__

obj = MaClasseALaClasse()
obj.describe_yourself()

obj = MaClasseALaClasse(1,2,3, a=5, b=6, c=7)
obj.describe_yourself()

assert obj.a == 5


Attention au naming, toujours CamelCase pour les noms de classe


## type hints + fonctions imbriquées

def greeting(name: str) -> str:
    return 'Hello ' + name

from typing import List, Callable
def greetings(*names: str, **more_names: int) -> Callable:
    def imbriquee():
        print(names)
        print(more_names)
    return imbriquee

greetings("flouze", "thunes", "money", h=45, b=87, zzza=987654)()

def greetings() -> Callable:
    def imbriquee(*aaa, **bbb):
        print(aaa)
        print(bbb)
    return imbriquee

greetings()("flouze", "thunes", "money", h=45, b=87, zzza=987654)



type(b)
<class 'dict'>



## Lire des stacktraces

## Débug avec Pycharm

## decorateurs


. Bibliothèque Standard

json, date, 

. Bibliothèques très connues et utilisées
requests, sqlalchemy & alembic, celery, pandas, tensorflow...

. Framework web
Django & DjangoRestFramework, Flask, FastAPI
