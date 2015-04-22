# Inapropriate Intimacy
*Ne tripotons pas la guitare avec tous les doigts*

<a href='https://www.youtube.com/watch?v=vD4c2wO_pyY'>- Bobby Lapointe -</a>

## Qu'est-ce que c'est ?

Des classes en savent trop l'une sur l'autre et se sentent trop à l'aise l'une chez l'autre

## En pratique ça ressemble à quoi ?

```java
package faites.vous.des.potes;
public interface Copain {
    public void aLAise(Copain copain);
}
```
```java
package faites.vous.des.potes.soulards;
public class Freddo implements Copain {
	private PorteFeuille larfeuille = new PortefeuilleEnCroco();
	Alcool jAiBu;

	public PorteFeuille prendreSonLarfeuille() {
		return larfeuille;
	}

	@Override
	public void aLAise(Copain ceSacreVieux) {
		JeanMichMuch ceSacreVieuxJeanMichMuch = (JeanMichMuch) ceSacreVieux;
		Refrigirateur frigo = ceSacreVieuxJeanMichMuch.allerDirectASonFrigo();
		Biere laBinouzeAmonPote = frigo.ouvrir().prendreLaDerniereBinouze();
		picoler(laBinouzeAmonPote);
	}

	private void picoler(Alcool prendreLaDerniereBinouze) {
		jAiBu = prendreLaDerniereBinouze;
	}
}
```
```java
package faites.vous.des.potes.cleptos;
public class JeanMichMuch implements Copain{
	private Refrigirateur frigo = new Frigidaire();
	Argent monPoignon;

	public Refrigirateur allerDirectASonFrigo() {
		return frigo;
	}
	
	public void aLAise(Copain monVieuxPote){
		Freddo monVieuxPoteFreddo = (Freddo) monVieuxPote;
		Argent sonFric = monVieuxPoteFreddo.prendreSonLarfeuille().prendreDuPoignon(50);
		seMettreDansLaPoche(sonFric);
	}

	private void seMettreDansLaPoche(Argent duPoignon) {
		monPoignon = duPoignon; 
	}
}
```

## Pourquoi ça pue ?

De toutes évidence si une classe en sait trop sur une autre elle va pouvoir commencer à trifouiller ce qu'elle ne devrait peut-être pas. On se retrouve vite avec des effets de bords inattendus, une classe n'est plus maîtresse de ce qui se passe chez elle et ne contrôle plus les entrées et sorties.

Outre le fait que le 'S' (encore lui) de SOLID est une fois de plus bafoué, on peut aussi noter qu'il est ainsi plus difficile de modifier le comportement d'une classe avec laquelle d'autres sont trop intimes.

## Comment est-ce qu'on le détecte ?

Lorsqu'on commence à voir une méthode appeller un getter d'un objet pour lire (voir modifier !!) des éléments qui ne sont sensés appartenir qu'à ce dernier.

## Quand est-ce acceptable ?

Probablement jamais !

## Comment le résoudre ?

Principalement en ne mettant pas des accesseurs à volonté et en extrayant au besoin les méthodes ou données d'une classe qui en a peut-être trop sur les bras.
