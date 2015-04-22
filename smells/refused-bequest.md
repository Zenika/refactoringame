# Refused Bequest
><a href='http://i.ytimg.com/vi/j7U-BabN6EU/hqdefault.jpg'>*N'oublie pas qui tu es Simba !*</a>

## Qu'est-ce que c'est ?

Un "refused bequest" ou "refus de leg" se caractérise par une classe qui fait le tri dans ce dont elle hérite.

## En pratique ça ressemble à quoi ?

```java
package bettencourt;

import java.math.BigInteger;
import fric.Poignon;

public class Liliane {

	public boolean aDuPoignon() {
		return true;
	}
	public boolean aBeacoupDePoignon() {
		return true;
	}
	public boolean aEnormementDePoignon() {
		return true;
	}
	public boolean aPresqueTropDePoignon() {
		return true;
	}

	public Poignon gratterDuFric(BigInteger montant){
		return new Poignon(montant);
	}
}
```
```java
package bettencourt.meyers;

import java.math.BigInteger;
import fric.Poignon;
import bettencourt.Liliane;

public class Francoise extends Liliane{

	@Override
	public Poignon gratterDuFric(BigInteger montant) {
		throw new UnsupportedOperationException("JAMAIS !!! Moi vivante, JAAAAMAIS !!!");
	}
}
```

## Pourquoi ça pue ?

Tu casses le 'L' de solid. Les utilisateurs de ta classe vont devoir connaitre l'implémentation alors que le contrat devrait 
suffir.

## Comment est-ce qu'on le détecte ?

Dans les logs :D

Non sérieusement, on le repère vite en voyant trainer des throw new UnsupportedOperationException(...) ou autre 
(la méthode ne fait qu'une ligne et c'est un throw).

## Quand est-ce acceptable ?

Renvoyer vide ou null (ou absent selon le siècle dans lequel vous vivez) peut être une implémentation acceptable.

De la même manière, lever une exception prévue par le contrat de la méthode étendue n'est pas un refus de leg.

## Comment le résoudre ?

Retravaillez votre 'I' de "SOLID", à un certain moment vous avez défini vos interfaces de manière trop générale, vous avez fait des amalgames et ça c'est mal.

## Un petit exemple de refacto pour la route ?

```java
package bettencourt;

public abstract class Bettencourt {
	public boolean aDuPoignon() {
		return true;
	}
	public boolean aBeacoupDePoignon() {
		return true;
	}
	public boolean aEnormementDePoignon() {
		return true;
	}
	public boolean aPresqueTropDePoignon() {
		return true;
	}
}
```
```java
package senil;

import java.math.BigInteger;
import fric.Poignon;

public interface BonnePoire {
	public Poignon gratterDuFric(BigInteger montant);
}
```
```java
package bettencourt;

import java.math.BigInteger;
import senil.BonnePoire;
import fric.Poignon;

public class Liliane extends Bettencourt implements BonnePoire{
	@Override
	public Poignon gratterDuFric(BigInteger montant){
		return new Poignon(montant);
	}
}
```
```java
package bettencourt.meyers;

import bettencourt.Bettencourt;

public class Francoise extends Bettencourt{}
```
