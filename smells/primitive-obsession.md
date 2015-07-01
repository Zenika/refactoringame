# Primitive Obsession
>_ Tu veux des :
>
>[Sucre, cacahuètes, pâte de cacao, lait écrémé en poudre, lactose et protéines de lait, matière grasse végétale, beurre de cacao, beurre concentré, amidon, sirop de glucose, émulsifiant (lécithine de soja), gélifiant (gomme arabique), colorants (E100, E120, E133, E160e, E171), dextrine, agent d'enrobage (cire de carnauba), arômes, sel, huile végétale] ?
>
>_ ???
>
>_ Bah des M&M's quoi ...

## Qu'est-ce que c'est ?

Des primitives partout.

## En pratique ça ressemble à quoi ?
```java
package la.superette.du.coin.du.futur;

import java.util.HashMap;
import java.util.Map;
import la.superette.du.coin.du.futur.utils.ConversionUtil;

public class CaisseEnregistreuse {

	private Map<Long, Float> valeurs = new HashMap<>();
	private Map<Long, String> devises = new HashMap<>();

	public void encaisser(float valeur, String devise) {
		// Le temps est utilisé pour garder la cohérence entre les sommes et les
		// devises.
		long tempsEncaissement = System.currentTimeMillis();

		valeurs.put(tempsEncaissement, valeur);
		devises.put(tempsEncaissement, devise);
	}

	public String formatterPrixPourAffichage(float valeur, String devise) {
		String result = "";
		if (devise.equals("$") || devise.equals("F")) {
			result += devise;
		}
		result += valeur;
		if (devise.equals("€") || devise.equals("£")) {
			result += devise;
		}
		return result;
	}

	/**
	 * Et le prix nobel d'informatique 2015 est decerné à ...
	 * */
	public float calculerLaCaisseEnFinDeJournee() {
		Float total = 0f;
		for (Long temps : valeurs.keySet()) {
			Float valeur = valeurs.get(temps);
			String devise = devises.get(temps);
			if (!devise.equals("€")) {
				valeur = ConversionUtil.convertirEnEuros(valeur, devise);
			}
			total += valeur;
		}
		return total;
	}
}
```
Je vous épargne le contenu de `ConversionUtil`, je ne suis pas sadique.
## Pourquoi ça pue ?

Le fonctionnel se dissout dans les problématiques d'implémentation et une recherche de cohérence qui aurait du être maintenue par une classe.
Faites du DDD bons dieux, combien de fois faudra-t-il vous le dire ?!

## Comment est-ce qu'on le détecte ?

Si les paramètres d'une méthode sont interdépendants, qu'en supprimer un vous oblige à en modifier (voir supprimer) d'autres, alors ça sent.
Si des paramètres ou des variables ont un préfixe commun (userName, userLastName, userId, userShoeSize, ...) alors ça sent vraiment fort.

## Quand est-ce acceptable ?
Il y a un débat sur le sujet, certains disent que dans le cas d'un framework attendant les composantes "data" d'un objet fonctionnel alors cette primitive obsession peut être acceptable. D'autres disent que "proxifier" la classe du framework peut résoudre le problème au prix de quelques getters et setters. Ce n'est donc pas idéal et pas toujours possible (classes finales). Usez de votre meilleur jugement.

## Comment le résoudre ?

Faites de la veritable programation objet (domain-objects, value objects) et pas du déballage de variables.

## Un petit exemple de refacto pour la route ?
```java
package la.superette.du.coin.du.futur;

import java.math.BigDecimal;
import java.math.MathContext;
import java.util.ArrayList;
import java.util.Collection;

import la.superette.du.coin.du.futur.poignon.SommeDeArgent;

public class CaisseEnregistreuse {

	private Collection<SommeDeArgent> paiements = new ArrayList<>();

	public void encaisser(SommeDeArgent paiement) {
		paiements.add(paiement);
	}

	public String formatterPrixPourAffichage(SommeDeArgent paiement) {
		return paiement.toString();
	}

	public float calculerLaCaisseEnFinDeJournee() {
		BigDecimal total = new BigDecimal(0, MathContext.UNLIMITED);
		for (SommeDeArgent sommeDeFric : paiements) {
			total += sommeDeFric.toDevise(SommeDeArgent.EURO);
		}
		return total.floatValue();
	}
}
```
Epargnez moi le contenu de `SommeDeArgent`, soyez pas sadique.
