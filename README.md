# Tests unitaires

## 1. Intro

Pour comprendre le principe de test unitaire nous allons construire une maison rouge en lego. Voici à quoi doit ressembler la maison :

![](http://logu-kirou.com/tmp/maison.png)

Pour construire cette maison nous allons premièrement identifier les types de briques qui constitue cette maison.

![](http://logu-kirou.com/tmp/blocs.png)


Une fois qu'on a analysé ces types de brique. Sachant qu'on construit une maison rouge. On va s'assurer que la bloque unitaire qu'on utilise est rouge, qu'il a des encoches et il a x encoches. Quand on prend cette brique on vérifie tout cela spontanément. En faisant cela on détermine un des cas de tests. Ces tests vont nous permettre d'assurer  d'une part la fonction de chaque brique et d'autre part que tout au long de la construction de notre maison on maintien la stabilité et la non régression de chaque étape.
(Être rouge, avoir des encoches,  être associable à d'autres briques...).

## 2. Mise en pratique


Installer le projet :
```
Git clone https://github.com/logu/test-unitaire.git
npm init
```
Mise en place d'un environnement de test: Test framework mocha :
```
npm install -g mocha
npm install --save-dev mocha
mocha --version
```

## 3. Les premiers pas

```
mkdir test && cd test
touch testApp.js && cd ..
```
#### 3.1. Test framework

On edite le fichier testApp.js qu'on vient de créer pour ajouter le code suivant ...

```
describe('String miniscule', function () {
	it('doit retourner un string en minuscule');
	it('doit retirer les tirets');
});

```
et lance la ligne de commande suivante :
```
mocha
```

Si on analyse la sortie :

```
  String miniscule
    - doit retourner un string en minuscule
    - doit retirer les tirets


  0 passing (7ms)
  2 pending
```

On peut voir ici que mocha nous met en place un environnement de test. Pour effectuer des tests on a besoin des assertions qui vont nous permettre de dire j'ai tel valeur en entrée et j'attend tel valeur à la sortie.
Pour celà nous allons donc utiliser chai.

#### 3.2. Mise en place d'assertions

Mise en place d'une logique d'assertion : Librairie d'assertions chai :
```
npm install --save-dev chai
```

```
var chai = require('chai');
var expect = require('chai').expect;

describe('String miniscule', function () {
	it('doit retourner un string en minuscule', function(){
		expect('toto').to.equal('toto');
		expect('toto').to.not.equal('TOTO');
	});
	it('doit retourner un string', function(){
		expect('toto').to.be.a('string');
	});
	it('doit retourner un string sans tirets', function(){
		var inputValue = 'TOTO-BOSS';
		var outputValue = 'TOTO-BOSS';

		expect(inputValue).to.equal(outputValue);
	});
});

```
#### 3.2. Notre premier vrai test :) 

Maintenant qu'on a implémenté un environnement de test et qu'on a vérifié le fonctionnement de celui-ci, créons une fonction 'stringCleaner' qu'on pourra tester.

on va créer un fichier app.js pour y créer notre fonction.
```
touch app.js
```

```
exports.stringCleaner = function(val){
	return val.toLowerCase();
}
```

On va maintenant modifer le test pour tester notre fonction :

```
var chai = require('chai');
var expect = require('chai').expect;
var app = require('./app');

describe('String miniscule', function () {
	it('doit retourner un string en minuscule', function(){
		var inputValue = 'TOTO';
		var outputValue = app.stringCleaner(inputValue);

		expect(outputValue).to.equal('toto');
		expect(outputValue).to.not.equal('TOTO');
	});
	it('doit retourner un string', function(){
		var outputValue = app.stringCleaner('TOTO');
		expect('TOTO').to.be.a('string');
	});
	it('doit retourner un string sans tirets', function(){
		var inputValue = 'TOTO-BOSS';
		var outputValue = app.stringCleaner(inputValue);

		expect(outputValue).to.equal('toto boss');
	});
});

```
Si on lance le test maintenant, on va avoir les deux premiers tests unitaires qui vont passer et le dernier qui va échouer simplement parcequ'on a pas encore implémenté la fonctionalité qui retire les tirets. Nous allons maintenant modifier la fonction pour qu'il retourne un string en minuscule et sans tirets.

```
exports.stringCleaner = function(val){
	return val.toLowerCase().replace(/-/g,' ');
}
```

En lançant le test on peut voir qu'on valide chaque assertion. 

## 4. Hooks, exclusive, include

En plus de it() on peut utiliser les fonction suivantes : 
```before```, ```beforeEach```, ```after``` and ```afterEach```
Si on fait le test on peut voir que ces fonctions sont éxecuter respectivement : 
 - avant de lancer les tests 
 - avant chaque test 
 - après avoir lancer les tests
 - après avoir lancer chaque test

```
afterEach(function() {
    console.log('after!');
});
```

Il est aussi possible d'utilser les fonctions ```only``` ou ```skip``` avec la fonction ```it``` pour lancer seulement un test ou sauter un test. 

```
// exclure un test
...
it.skip('doit retourner un string sans tirets', function(){
		var inputValue = 'TOTO-BOSS';
...
```

## 4. Options de mocha

Avoir un autre format de sorti : la commande suivante va nous donner les differentes sortie possible :

```
mocha --reporters
```

Il est possible d'avoir à la sortie un fichier html : 
```
mocha indexSpec.js --reporter doc > out.html
```

ou un nyancat : 
```
mocha --reporter nyan
```

Des options peuvent être stocker dans un fichier dans le projet : 
```
mocha.opts
```

```
--reporter nyan
--recursive
```

