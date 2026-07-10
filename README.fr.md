# ha-facade-cover-control

Blueprint Home Assistant pour la **gestion des volets par façade**, avec prise en compte du soleil, de la chaleur, des jours travaillés, des vacances et d’une fermeture saisonnière.

Ce blueprint est prévu pour les maisons où chaque façade reçoit le soleil à des moments différents et où les volets ne doivent pas tous réagir de la même manière.

## Fonctionnalités

- Pilotage de plusieurs volets pour une même façade/exposition.
- Blocage de l’ouverture du matin lors des journées chaudes, basé sur la température max prévue.
- Réouverture quand le soleil ne tape plus sur la façade.
- Réouverture complète ou partielle configurable.
- Horaires d’ouverture par jour de la semaine.
- Capteurs optionnels : jour travaillé, jour férié, vacances, absence/sécurité.
- Fermeture au coucher du soleil avec offsets mensuels configurables.
- Fonctionne localement dans Home Assistant.

## Blueprint

Fichier :

```text
blueprints/automation/facade_cover_control.yaml
```

## Principe

Journée normale :

```text
ouverture à l’heure configurée, avec respect du lever du soleil si activé
fermeture au coucher du soleil + offset mensuel
```

Journée chaude :

```text
les volets restent fermés le matin
ils peuvent rouvrir lorsque le soleil ne tape plus sur la façade
la réouverture peut être partielle ou complète
fermeture au coucher du soleil + offset mensuel
```

## Importer dans Home Assistant

Cliquez sur le bouton ci-dessous pour importer directement le blueprint depuis GitHub :

[![Ouvrir Home Assistant et importer le blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/WadohS/ha-facade-cover-control/main/blueprints/automation/facade_cover_control.yaml)

URL d’import manuel :

```text
https://raw.githubusercontent.com/WadohS/ha-facade-cover-control/main/blueprints/automation/facade_cover_control.yaml
```

Il est aussi possible de copier manuellement le fichier blueprint dans Home Assistant :

```text
/config/blueprints/automation/facade_cover_control.yaml
```

Puis créer une automatisation à partir du blueprint dans Home Assistant.

## Test recommandé

Commencer avec une seule façade, par exemple cuisine / salle à manger :

```text
cover.volet_cuisine
cover.volet_salle_a_manger
```

Avec un capteur soleil direct de façade, par exemple :

```text
binary_sensor.facade_est_sud_sun_direct
```

Pendant les tests, désactiver les autres automatisations qui pilotent les mêmes volets pour éviter les conflits.

## Statut

Version actuelle : `0.1.0`

Version initiale. À tester sur un petit groupe de volets avant généralisation.

## Documentation

- [Guide de configuration — Français](docs/configuration.fr.md)
- [Configuration guide — English](docs/configuration.en.md)

## Licence

MIT
