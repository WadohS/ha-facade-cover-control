# Feuille de route / idées

## Court terme

### Mode debug / logbook

Ajouter un mode optionnel de debug/logbook pour faciliter les tests réels.

Idées :

- `enable_logbook: true`
- journaliser la température max prévue, la décision journée chaude, l’heure d’ouverture choisie, l’offset de fermeture et l’état soleil façade ;
- journaliser les actions ignorées, par exemple :
  - ouverture du matin ignorée car `hot_day=true` ;
  - réouverture journée chaude ignorée car façade encore au soleil ;
  - fermeture soir ignorée car volets déjà fermés ;
- éventuellement écrire les mêmes informations dans le helper de statut.

Exemples de messages :

```text
Journée chaude détectée : forecast_max=35.0, seuil=25.0. Ouverture du matin ignorée.
Le soleil ne tape plus sur la façade. Réouverture des volets à 40 %.
Fermeture des volets : offset de juillet +45 min.
```

### Flexibilité météo / prévision

Le blueprint utilise actuellement `weather.get_forecasts` avec `type: daily` et lit la valeur `temperature` du premier élément comme température max prévue du jour.

Options futures :

- permettre un capteur externe dédié à la température max prévue du jour ;
- supporter plusieurs fournisseurs météo avec des formats de prévision différents ;
- ajouter une option `forecast_temperature_mode` :
  - `daily_forecast_temperature`
  - `external_sensor`
- stocker la source météo et la valeur retenue dans le helper de statut pour faciliter le debug.

Cela aiderait les utilisateurs dont l’entité météo affiche surtout la température actuelle, ou dont le format de prévision daily est différent.

### Fenêtre solaire intégrée

Implémenté en `0.1.2` : permettre l’utilisation directe de l’azimut/élévation de `sun.sun` au lieu d’exiger un binary sensor soleil direct.

Objectif : rendre le blueprint plus simple à utiliser, sans devoir créer un template binary sensor séparé pour chaque façade.

Entrées :

- `sun_detection_mode` : `direct_sensor`, `solar_window` ou `direct_sensor_or_solar_window`
- `facade_azimuth` : direction perpendiculaire de la façade / du mur, en degrés depuis le Nord
- `solar_window_before` : degrés retirés à l’azimut de façade
- `solar_window_after` : degrés ajoutés à l’azimut de façade
- `solar_elevation_min`

Comportement :

```text
facade_sunny =
  le capteur direct est on
  OU l’azimut/élévation du soleil est dans la fenêtre solaire configurée
```

Cela doit rester optionnel, car certains utilisateurs auront déjà des capteurs soleil direct plus précis qui tiennent compte des ombres/obstacles mieux qu’un simple calcul d’azimut.

## Plus tard

### Mode dry-run

Ajouter un mode qui journalise ce qui serait fait sans bouger les volets.

### Gestion du mode manuel

Utiliser le helper de statut pour éviter une réouverture après fermeture manuelle par l’utilisateur.

### Ventilation nocturne

Optionnelle, désactivée par défaut. Pourrait ouvrir partiellement certains volets la nuit pour rafraîchir, avec conditions présence/absence/météo.
