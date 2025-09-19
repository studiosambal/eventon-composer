# Plugin installeren via Composer die niet via WordPress zelf beschikbaar is

Soms moet er op één site een extra plugin worden geïnstalleerd die niet in de standaard repository staat (bijv. EventON of een andere premium plugin). Dit kan via het deployment script in Laravel Forge.

---

## Stappenplan

### 1. Download de plugin

- Download het originele **zip-bestand** van de plugin (zoals je het ook via WordPress → Plugins → Upload zou doen).
- Let op: laat het zipje zoals het is, dus niet uitpakken of opnieuw inpakken.

### 2. Upload naar Github

- Zet het zip-bestand in de Dropbox-map `/Dropbox/Websites/_premiumplugins`
- Ga naar [Github](https://github.com/studiosambal/premiumplugins) en bekijk daar de repo
    - Kopieer de **directe `View raw` link**

Voorbeeld:

```
- Link van repo: https://github.com/studiosambal/premiumplugins/blob/main/eventon.zip
- De view raw link: https://github.com/studiosambal/premiumplugins/raw/refs/heads/main/eventon.zip
```

### 3. Pas het deployscript aan

In het deployment script van de site (Forge) dit blok toevoegen:

> Zie ErvaarOudewater.nl als voorbeeld 

```bash
# --- PREMIUM PLUGIN DIE NIET VIA WP BESCHIKBAAR IS ---
# ZIE NOTION VOOR MEER INFO: https://www.notion.so/studiosambal/Plugin-installeren-via-Composer-die-niet-via-WordPress-zelf-beschikbaar-is-2714cc6762a780b6a773ec1fa012d223?source=copy_link

# Alleen deze twee aanpassen per plugin
PLUGIN_URL="https://github.com/studiosambal/premiumplugins/raw/refs/heads/main/eventon.zip" # Pas deze aan!
PLUGIN_NAME="studiosambal/pluginnaam"   # vendor/slug » map wordt wp-content/plugins/<slug>/

# Repo definiëren en installeren
$FORGE_COMPOSER clear-cache

JSON_REPO=$(cat <<EOF
{
  "type": "package",
  "package": {
    "name": "$PLUGIN_NAME",
    "version": "dev-main",
    "type": "wordpress-plugin",
    "dist": { "url": "$PLUGIN_URL", "type": "zip" }
  }
}
EOF
)
$FORGE_COMPOSER config repositories.customplugin "$JSON_REPO"
$FORGE_COMPOSER require $PLUGIN_NAME:dev-main --no-interaction --prefer-dist -o --no-progress

```

---

### 4. Wat aanpassen?

- **PLUGIN_URL** → zet hier de Github-raw-link van de plugin.
- **PLUGIN_NAME** → kies een logische naam, bv. `studiosambal/eventon`.
    - Het deel **na de slash** bepaalt de mapnaam in `wp-content/plugins/`.
    - Voorbeeld: `studiosambal/eventontest` → map wordt `wp-content/plugins/eventontest/`.

---

### 5. Deployment uitvoeren

- Klik in Forge op **Deploy now**.
- Composer haalt de plugin op en zet deze in de juiste pluginmap.
- Eventueel wordt de inhoud één niveau omhoog gezet als de zip nog een extra map bevat (zoals `eventON/`).

---

### 6. Plugin activeren

- Ga in WordPress naar **Plugins → Geïnstalleerde plugins**.
- Activeer de plugin handmatig.
