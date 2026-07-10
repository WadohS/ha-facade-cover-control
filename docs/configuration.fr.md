# Guide de configuration

## Principe

Créer une automatisation par façade / exposition.

Pour chaque instance, choisir :

- les volets de cette façade ;
- un binary sensor qui vaut `on` quand le soleil tape sur cette façade ;
- une entité météo compatible avec `weather.get_forecasts` en `type: daily` ;
- les horaires d’ouverture ;
- le seuil de journée chaude ;
- les offsets mensuels de fermeture.

## Capteur soleil direct

Le blueprint attend un capteur binaire :

```text
on  = le soleil tape sur cette façade
off = le soleil ne tape plus sur cette façade
```

Ce capteur peut être créé avec un template Home Assistant basé sur l’azimut/élévation de `sun.sun`, ou par une autre intégration.

## Logique journée chaude

Le blueprint appelle :

```yaml
weather.get_forecasts
```

avec :

```yaml
type: daily
```

Il utilise la température du premier élément de prévision comme température max prévue du jour.

Si :

```text
température max prévue >= seuil chaleur
```

alors l’ouverture normale du matin est ignorée.

Quand le capteur soleil direct passe à `off`, le blueprint peut rouvrir les volets :

- pas du tout ;
- partiellement ;
- complètement.

## Horaires d’ouverture

On peut définir une heure par jour de la semaine.

Des capteurs optionnels peuvent modifier la logique :

1. absence / sécurité ;
2. vacances ;
3. jour férié ;
4. jour travaillé ;
5. horaire par jour.

## Fermeture

La fermeture est basée sur :

```text
coucher du soleil + offset mensuel
```

Chaque mois possède un offset réglable en minutes.

Exemple :

```text
hiver : fermer avant le coucher du soleil
été : fermer après le coucher du soleil
```

## Recommandation de test

Commencer avec une seule façade et deux volets. Désactiver toute autre automatisation qui pilote les mêmes volets pendant les tests.
