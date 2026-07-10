# Guide de configuration

## Principe

Créer une automatisation par façade / exposition.

Pour chaque instance, choisir :

- les volets de cette façade ;
- soit un binary sensor qui vaut `on` quand le soleil tape sur cette façade, soit les paramètres de fenêtre solaire intégrée ;
- une entité météo compatible avec `weather.get_forecasts` en `type: daily` ;
- les horaires d’ouverture ;
- le seuil de journée chaude ;
- les offsets mensuels de fermeture.

## Helper de statut optionnel

Le blueprint peut utiliser un `input_text` optionnel comme petite mémoire persistante, inspirée de CCA.

Créer un helper par façade / instance d’automatisation, par exemple :

```text
input_text.facade_est_sud_cover_status
```

Le helper stocke du JSON avec les actions déjà faites aujourd’hui : ouverture normale, réouverture après chaleur, fermeture du soir. Cela permet d’éviter les répétitions après un rechargement ou un redémarrage.

Si aucun helper n’est sélectionné, le blueprint fonctionne quand même, mais avec moins d’état persistant.

Le helper stocke aussi la température max prévue utilisée pour la dernière action (`forecast_max`) et si la journée a été considérée comme chaude (`hot_day`). C’est utile pour diagnostiquer une mauvaise entité météo.

Pour la chaleur, choisir une entité météo dont la prévision daily contient bien la température max de la journée. Dans cette installation, `weather.tacoignieres` / Météo-France semble être un meilleur candidat qu’une entité météo dont la température affichée est surtout la température actuelle.

## Détection soleil direct

Le blueprint propose trois modes de détection du soleil direct sur la façade :

```text
Capteur direct seulement
Fenêtre solaire seulement
Capteur direct ou fenêtre solaire
```

### Capteur direct

Utiliser ce mode si vous avez déjà un capteur binaire :

```text
on  = le soleil tape sur cette façade
off = le soleil ne tape plus sur cette façade
```

Ce capteur peut être créé avec un template Home Assistant basé sur l’azimut/élévation de `sun.sun`, ou par une autre intégration. Il devient optionnel si vous utilisez la fenêtre solaire intégrée.

### Fenêtre solaire intégrée

Le mode fenêtre solaire utilise directement l’azimut et l’élévation de `sun.sun`. Il évite de créer un binary sensor séparé pour chaque façade.

Paramètres :

```yaml
facade_azimuth: 145
solar_window_before: 65
solar_window_after: 75
solar_elevation_min: 3
```

`facade_azimuth` correspond à la direction perpendiculaire à la façade / au mur, en degrés depuis le Nord. C’est le centre de la fenêtre solaire, pas le début ni la fin.

`solar_window_before` correspond aux degrés à retirer à l’azimut de façade.

`solar_window_after` correspond aux degrés à ajouter à l’azimut de façade.

Exemple :

```text
facade_azimuth = 145°
solar_window_before = 65°  -> la fenêtre commence à 80°
solar_window_after  = 75°  -> la fenêtre finit à 220°
```

Dans cet exemple, la façade est considérée au soleil si :

```text
l’azimut du soleil est entre 80° et 220°
et l’élévation solaire est >= 3°
```

Pour une façade Est ou Sud-Est qui chauffe tôt le matin, augmenter `solar_window_before` permet d’éviter que les volets ne rouvrent trop tôt alors que le soleil du matin tape encore directement.

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

Quand la détection choisie indique que la façade n’est plus au soleil, le blueprint peut rouvrir les volets :

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
