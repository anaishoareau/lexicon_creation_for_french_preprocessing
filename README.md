# Création du lexique pour le package french_preprocessing

Ce repository permet la construction du lexique (fichier lexique.txt) utilisé 
par l'algorithme de lemmatisation du repository french_preprocessing.

#### ATTENTON : Installations nécessaires pour faire fonctionner les codes

Pour que les codes puissent fonctionner, il faut avoir installé java (JRE) : https://www.java.com/fr/download/ 
et adapté le path du fichier java.exe dans les codes (au niveau de l'initialisation du StanfordPOSTagger) du type :

```python
java_path = "C:/Program Files/Java/jre1.8.0_211/bin/java.exe".
```

## Contenu du fichier lexique.txt

Le fichier lexique.txt, contenu dans le dossier final_data, est écrit à la manière 
d'un dictionnaire de dictionnaires. Les clés du dictionnaire principal sont les mots 
de la langue française pouvant être lemmatisés par la méthode FrenchPreprocessing.lemmatize() 
du package french_preprocessing. Pour plus d'informations 
voir : https://github.com/anaishoareau/french_preprocessing.

A chaque clé-mot est associé un dictionnaire dont les clés sont des tags possibles du ce clé-mot 
parmi la liste suivante : ['v', 'nc', 'adj', 'c', 'npp', 'adv', 'det', 'pro', 'prep', 'i', 'ponct', 'cl', 'et'], 
et les valeurs des tags-clés sont les lemmes associés aux tags-clés. 

Extrait du contenu du fichier lexique.txt : 
{ [...] 'affouillées': {'adj': 'affouillé', 'v': 'affouiller'}, 'st-michel-de-fronsac': {'npp': 'St-Michel-de-Fronsac'}, 
'ferrites': {'nc': 'ferrite'} [...] }

## Construction de lexique.txt

Le fichier lexique.txt est issu de la synthétisation de trois lexiques : 
Lexique des formes fléchies du français (LEFFF), le Lexique 3.83, et le lexique utilisé par la librarie python spaCy.

Une nouvelle classification des tags est utilisée dans lexique.txt, créée à partir de l'uniformisation des tags 
des deux lexiques, LEFFF et Lexique 3.83, et des tags du StandfordPOSTagger.
Les données fournies par le lexique de spaCy sont taguées à l'aide du StandfordPOSTagger 
et les sorties sont réduites pour correspondre à la nouvelle classification.

Les tags après uniformisation sont réduits à : 'v' : verbe, 'nc' : nom commun, 'adj' : adjectif, 
'c' : conjonction, 'npp' : nom propre, 'adv' : adverbe, 'det' : déterminant, 'pro' : pronom, 
'prep' : préposition, 'i' : interjection, 'ponct' : ponctuation, 'cl' : clitique, 'et' : mot en langue étrangère.

Les fichiers du dossier initial_data : lexique_spacy.txt et lexique383.txt sont les données extraites 
respectivement du module spaCy (lemmatisation en français) et du fichier Lexique383.xlsb 
téléchargeable sur le site http://www.lexique.org/.

## Outils pour la motification du fichier lexique.txt

Les outils pour la modification du lexique afin de l'augmenter facilement, 
sans compromettre le fichier texte, sont dans le fichier lexique_tools.ipynb, 
mais également dans le package french_preprocessing dans le fichier lexique_tools.py.

#### Intialisation de la classe LexiqueTools :

```python 
lt = LexiqueTools()
```

#### Méthodes de la classe LexiqueTools :

- lt.in_lexique(word)

Prend une string en entrée, renvoie False si le mot n'est pas dans le dictionnaire, sinon
renvoie la valeur associée à la string dans le dictionnaire.

##### Exemple :

```python 
lt.in_lexique('mangé')
>>> {'v': 'manger', 'adj': 'mangé'}

lt.in_lexique('cemotnexistepas')
>>> False
```

- lt.lexique_rewrite()

Ne prend rien en argument et ne renvoie rien, sert à réécrire le lexique
lorsque des modifications ont eu lieu.

Doit être utilisée après les ajouts ou les suppressions de mots, 
sinon les changements ne sont pas pris en compte.

- lt.add_element(word, lemma, tag)

Prend en argument une string, son lemme associé, son tag et ne renvoie rien. 
Cette méthode ajoute le nouvel élément (word, lemma, tag) au dictionnaire, ou 
ne fait rien s'il y est déjà.

- lt.remove_element(word, tag)

Prend en argument le mot à supprimer et son tag, ne revoie rien.
Supprime tag associé au mot désiré ou ne fais rien si le tag n'existe 
pas dans le dictionnaire associé au mot.

##### Exemple : 

```python 
lt.in_lexique('mangé')
>>> {'v': 'manger', 'adj': 'mangé'}

lt.remove_element('mangé', 'v')

lt.in_lexique('mangé')
>>> {'adj': 'mangé'}
```

- lt.lexique_update(dictionary)

Prend en argument le dictionnaire des mots à ajouter au lexique, ne renvoie rien.
Réalise une sucession d'ajouts des mots de "dictionnary" dans le lexique.

##### Exemple :

```python 
lt.in_lexique('mangé')
>>> {'adj': 'mangé'}

dictionnary = {'mangé':{'v':'manger'}, 'nouveaumot':{'nc':'nouveaulemme', 'v':'nouveaulemme2'}}

lt.lexique_update(dictionary)

lt.in_lexique('mangé')
>>> {'v':'manger','adj': 'mangé'}

lt.in_lexique('nouveaumot')
>>> {'nc':'nouveaulemme', 'v':'nouveaulemme2'}
```

##### ATTENTION : Après chaque série de manipulations, il est nécessaire de réécrire le lexique à l'aide de la méthode : lexique_rewrite().


## Sources et crédits 

#### LEXIQUE 3.83 : 

- PALLIER Christophe, NEW Boris, 2019 Openlexicon, GitHub repository, 
https://github.com/chrplr/openlexicon

- NEW Boris, PALLIER Christophe, BRYSBAERT Marc and FERRAND Ludovic. 2004. 
"Lexique 2: A New French Lexical Database." Behavior Research Methods, 
Instruments, & Computers 36 (3): 516–524. https://link.springer.com/article/10.3758/BF03195598

- NEW Boris, PALLIER Christophe, FERRAND Ludovic and MATOS Rafael. 2001. 
"Une Base de Données Lexicales Du Français Contemporain Sur Internet: LEXIQUE" 
L’Année Psychologique 101 (3): 447–462. 
https://www.persee.fr/doc/psy_0003-5033_2001_num_101_3_1341

- NEW Boris, BRYSBAERT Marc, VERONIS Jean, and PALLIER Christophe. 2007. 
"The Use of Film Subtitles to Estimate Word Frequencies." 
Applied Psycholinguistics 28 (4): 661–77. https://doi.org/10.1017/S014271640707035X

#### LEFFF (Morphological and syntactic lexicon for French) :

- SAGOT Benoît. 2010. "The Lefff, a freely available and large-coverage morphological 
and syntactic lexicon for French." In Proceedings of the 7th international conference 
on Language Resources and Evaluation (LREC 2010), Istanbul, Turkey. https://hal.inria.fr/inria-00521242/

- COULOMBE Claude, FrenchLefffLemmatizer, GitHub repository, 
https://github.com/ClaudeCoulombe/FrenchLefffLemmatizer

#### spaCy :

- spaCy, GitHub repository, https://github.com/explosion/spaCy

#### StanfordPOSTagger :

- The Stanford Natural Language Processing Group. "Stanford Log-linear Part-Of-Speech Tagger". 
https://nlp.stanford.edu/software/tagger.shtml

- TOUTANOVA Kristina and MANNING Christopher. 2000. "Enriching the Knowledge Sources Used 
in a Maximum Entropy Part-of-Speech Tagger". In Proceedings of the Joint SIGDAT Conference 
on Empirical Methods in Natural Language Processing and Very Large Corpora (EMNLP/VLC-2000), pp. 63-70.

- TOUTANOVA Kristina, KLEIN Dan, MANNING Christopher and SINGER Yoram. 2003. 
"Feature-Rich Part-of-Speech Tagging with a Cyclic Dependency Network". In Proceedings 
of HLT-NAACL 2003, pp. 252-259.


### Licenses and Copyrights : 

- License Lexique 3.83 : Attribution-ShareAlike 4.0 International, https://github.com/chrplr/openlexicon/blob/master/LICENSE.txt
- License spaCy : The MIT License (MIT), https://github.com/explosion/spaCy/blob/master/LICENSE
- Copyright spaCy : Copyright (C) 2016-2019 ExplosionAI GmbH, 2016 spaCy GmbH, 2015 Matthew Honnibal
- License StanfordPOSTagger :  GNU General Public License, https://github.com/stanfordnlp/CoreNLP/blob/master/licenses/gpl-2.0/LICENSE.txt
- FrenchLefffLemmatizer : Lesser General Public License For Linguistic Resources, https://github.com/ClaudeCoulombe/FrenchLefffLemmatizer/blob/master/LICENSE
