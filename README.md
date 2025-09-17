# Repman setup voor premium plugins

## Authenticatie instellen

Je kunt Composer toegang geven tot je Repman repo op twee manieren:

### Optie 1: Globaal instellen (lokaal of server)
Voer dit commando uit en geef je token op (op te vragen in Repman):

```bash
composer config --global --auth http-basic.studiosambal.repo.repman.io token
Optie 2: Via environment variable (aanrader voor servers, bv. Forge)
Voeg dit toe aan je .bashrc, .zshrc of als environment variable in Forge:

bash
Code kopiëren
export COMPOSER_AUTH='{
  "http-basic": {
    "studiosambal.repo.repman.io": {
      "username": "token",
      "password": "JOUW_TOKEN_HIER"
    }
  }
}'
Repo toevoegen in composer.json
In je project composer.json:

json
Code kopiëren
{
  "repositories": [
    {
      "type": "composer",
      "url": "https://studiosambal.repo.repman.io"
    },
    {
      "type": "composer",
      "url": "https://wpackagist.org"
    }
  ],
  "require": {
    "studiosambal/eventon": "^3.2",
    "composer/installers": "^2.0"
  },
  "extra": {
    "installer-paths": {
      "wp-content/plugins/{$name}/": ["type:wordpress-plugin"]
    }
  }
}
Installeren van een plugin
Nu kun je in elk project eenvoudig premium plugins installeren:

bash
Code kopiëren
composer require studiosambal/eventon
Workflow voor nieuwe pluginversie
Nieuwe plugin downloaden (bijv. EventON van ThemeForest).

Oude bestanden vervangen in de juiste map in je premiumplugins repo.

composer.json van die plugin updaten met het nieuwe versienummer.

Commit en push.

Nieuwe tag maken en pushen:

bash
Code kopiëren
git tag v3.2.2
git push --tags
Repman pakt de nieuwe tag op en maakt hem beschikbaar.

yaml
Code kopiëren

---
