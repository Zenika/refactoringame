# Feature Envy
<a href='http://fr.wiktionary.org/wiki/chacun_son_m%C3%A9tier,_les_vaches_seront_bien_gard%C3%A9es'>*Chacun son métier et les vaches seront bien gardées*</a>

## Qu'est-ce que c'est ?

Une classe commence à faire le boulot des autres.

## En pratique ça ressemble à quoi ?

```java
package stella.spotlight;
public class Artiste {
    Instrument instrument = new Guitare();
    Style style = new HipHop();

    public Instrument getInstrument() {
        return instrument;
    }
    public Style getStyle() {
        return style;
    }
}
```
```java
package zero.janvier;
import stella.spotlight.Artiste;
public class BusinessMan {
    Secretaire secretaire = new Secretaire();
    Artiste artisteQueJeManage = new Artiste();

    public boolean jAiDuSuccesDansMesAffaires(){
        return true;
    }
    public boolean jAiDuSuccesDansMesAmours(){
        return true;
    }
    public void changeSouvent(){
        this.secretaire = new Secretaire();
    }

    public Song crierQuiJeSuis(){
        return new Song(this.artisteQueJeManage.getInstrument(),this.artisteQueJeManage.getStyle());
    }
}
```

## Pourquoi ça pue ?

Une fois de plus le 'S' de SOLID qui prend un taquet.
Si les classes commencent à faire ce qu'elles veulent quand elles veulent, alors ça devient n'importe quoi.
> _Il est où déjà le code qui fait de l'HTML escaping ?

> _Dans la classe qui sérialise les dates. Pourquoi ?

Ce n'est pas intuitif, ce n'est pas maintenable, on perd du temps et de l'énergie gratos.

De plus on se retrouve parfois à devoir instancier des objets juste pour réaliser un traitement alors qu'on dispose de tous les objets qui nous semblent nécéssaires.

Enfin, ces features n'étant pas là où on s'attends à les trouver, on va pouvoir ignorer leur existence et les réécrire => Duplication :/

## Comment est-ce qu'on le détecte ?

On détecte souvent ces méthodes à la façon dont elles sont invoquées, on se retrouve à appeller une méthode sur un objet qui n'a pas l'air d'avoir grand chose à voir avec le sujet.
Souvent ce même objet peut n'être instancié que pour l'appel de cette méthode (vu qu'on ne fait rien qui ai un rapport avec sa fonction première).
On détecte souvent la Feature Envy lorsqu'une méthode appelle plusieurs méthodes d'un même autre objet.

## Quand est-ce acceptable ?

Un comportement de ce type peut cependant se justifier dans des cas spécifiques.

Les patterns stratégie et visiteur sont des exemples d'emploi par un objet de méthodes d'un objet d'une autre classe.
Ces deux patterns justifient ce comportement en ce qu'ils sont justement fait pour soulager une classe de traitements dont elle devrait être agnostique.
Ils permettent donc de consolider le 'S' de SOLID plutôt que de le fragiliser.

## Comment le résoudre ?

Rien de plus simple ! Si une (fraction de) méthode commence à tripatouiller trop un autre objet, extraire (délicatement) le bout de logique incriminé et placez-le là il a envie d'être et permettez lui de s'épanouir ! Votre IDE fait ça très bien et vos collègues seront ravis que vous les aidiez à endiguer leur alopécie naissante.

## Un petit exemple de refacto pour la route ?

```java
package stella.spotlight;
public class Artiste {
    Instrument instrument = new Guitare();
    Style style = new HipHop();

    public Instrument getInstrument() {
        return instrument;
    }
    public Style getStyle() {
        return style;
    }
    public Song crierQuiJeSuis(){
        return new Song(this.instrument, this.style);
    }
}
```
```java
package zero.janvier;
import stella.spotlight.Artiste;
public class BusinessMan {
    Secretaire secretaire = new Secretaire();

    public boolean jAiDuSuccesDansMesAffaires(){
        return true;
    }
    public boolean jAiDuSuccesDansMesAmours(){
        return true;
    }
    public void changeSouvent(){
        this.secretaire = new Secretaire();
    }

    public ProducedSong produire(Artiste artiste){
        return new ProducedSong(artiste.crierQuiJeSuis(), this.secretaire.getCachetLegal();
    }
}
```
