# Room Card Minimalist

[![hacs][hacs-badge]][hacs-url]
[![release][release-badge]][release-url]
![downloads][downloads-badge]
![build][build-badge]

![Card - Dark Theme](https://github.com/unbekannt3/hass-room-card-minimalist/blob/main/docs/images/cards-dark.png?raw=true)
![Card - Light Theme](https://github.com/unbekannt3/hass-room-card-minimalist/blob/main/docs/images/cards-light.png?raw=true)

## What is Room Card Minimalist

Room Card Minimalist is based on [patrickfnielsen/hass-room-card](https://github.com/patrickfnielsen/hass-room-card) but extensively redesigned with added functionality in the style of the [room-card from UI Lovelace Minimalist](https://ui-lovelace-minimalist.github.io/UI/usage/cards/card_room/) which I've used in the past and missed ever since for "normal" Lovelace setups.

It provides a fixed size card with a room name, styled icon, and optional secondary info. You can configure up to 4 entities to be displayed as buttons with icons that change based on the entity state.

## Installation

### HACS (Recommended)

Room Card is available in [HACS][hacs] (Home Assistant Community Store):

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=unbekannt3&repository=room-card-minimalist)

or search for "room-card-minimalist" in HACS.

### Manual

1. Download `room-card-minimalist.js` file from the [latest release][release-url].
2. Put `room-card-minimalist.js` file into your `config/www` folder.
3. Add reference to `room-card-minimalist.js` in your Dashboard. There's two ways to do that:
   - **Using UI:** _Settings_ → _Dashboards_ → _More Options icon_ → _Resources_ → _Add Resource_ → Set _Url_ as `/local/room-card-minimalist.js` → Set _Resource type_ as `JavaScript Module`.
     **Note:** If you do not see the Resources menu, you will need to enable _Advanced Mode_ in your _User Profile_
   - **Using YAML:** Add following code to `lovelace` section.
     ```yaml
     resources:
       - url: /local/room-card-minimalist.js
         type: module
     ```

## Configuration variables

The editor is supported, but if you want to use `yaml`, here are the properties:

### Card Configuration

| Name                               | Type    | Default  | Description                                                                                                                                        |
| :--------------------------------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                             | string  | Required | Name of the room to render.                                                                                                                        |
| `icon`                             | string  | Required | Icon to render.                                                                                                                                    |
| `icon_color`                       | string  | Optional | The color of the room icon. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).                                 |
| `secondary`                        | string  | Optional | Secondary info to render. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).                                   |
| `secondary_color`                  | string  | Optional | Color of the secondary text. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).                                |
| `card_template`                    | string  | Optional | Color template for the card. See [Available Color Templates](#available-color-templates) for options.                                              |
| `show_background_circle`           | boolean | `true`   | Whether to show the background circle behind the icon.                                                                                             |
| `background_circle_color`          | string  | Optional | Color of the background circle or empty for template color. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/). |
| `tap_action`                       | object  | Optional | Action to perform on tap. See [Home Assistant actions](https://www.home-assistant.io/dashboards/actions/).                                         |
| `hold_action`                      | object  | Optional | Action to perform on hold. See [Home Assistant actions](https://www.home-assistant.io/dashboards/actions/).                                        |
| `entities_reverse_order`           | boolean | `false`  | Display entities from bottom to top instead of top to bottom.                                                                                      |
| `use_template_color_for_title`     | boolean | `false`  | Use the card template color for the room title text.                                                                                               |
| `use_template_color_for_secondary` | boolean | `false`  | Use the card template color for the secondary text/template.                                                                                       |
| `entities`                         | list    | Optional | List of entities to display as buttons (max 4).                                                                                                    |

### Entity Configuration

| Name                   | Type    | Default  | Description                                                                                                                  |
| :--------------------- | :------ | :------- | :--------------------------------------------------------------------------------------------------------------------------- |
| `type`                 | enum    | Required | Use `entity` or `template`.                                                                                                  |
| `icon`                 | string  | Required | Icon to render.                                                                                                              |
| `icon_off`             | string  | Optional | Icon to render when state is off. If not set, the icon will not change.                                                      |
| `entity`               | string  | Required | Required if type is `entity`. The entity ID to monitor.                                                                      |
| `on_state`             | string  | Required | Required if type is `entity` and not a climate entity. The state value that will be considered as "on".                      |
| `condition`            | string  | Required | Required if type is `template`. Template that returns any value for "on" state, empty for "off".                             |
| `color_on`             | string  | Optional | Color for entity icon when on. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).        |
| `color_off`            | string  | Optional | Color for entity icon when off. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).       |
| `background_color_on`  | string  | Optional | Background color for entity when on. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).  |
| `background_color_off` | string  | Optional | Background color for entity when off. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/). |
| `template_on`          | string  | Optional | Color template to apply when entity is on (e.g., `blue`).                                                                    |
| `template_off`         | string  | Optional | Color template to apply when entity is off (e.g., `red`).                                                                    |
| `use_light_color`      | boolean | `false`  | For light entities: use the actual light color as the active state color.                                                    |
| `tap_action`           | object  | Optional | Action to perform on tap. See [Home Assistant actions](https://www.home-assistant.io/dashboards/actions/).                   |
| `hold_action`          | object  | Optional | Action to perform on hold. See [Home Assistant actions](https://www.home-assistant.io/dashboards/actions/).                  |

### Climate Entity Configuration

For climate entities (entities starting with `climate.`), the card automatically detects the available HVAC modes and provides mode-specific configuration options instead of the generic `on_state`, `color_on/off`, `template_on/off`, and `background_color_on/off` fields.

| Name                      | Type   | Default  | Description                                                                                                                                    |
| :------------------------ | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------- |
| `color_[mode]`            | string | Optional | Color for entity icon when in specific HVAC mode. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/).       |
| `background_color_[mode]` | string | Optional | Background color for entity when in specific HVAC mode. May contain [templates](https://www.home-assistant.io/docs/configuration/templating/). |
| `template_[mode]`         | string | Optional | Color template to apply when entity is in specific HVAC mode (e.g., `blue`, `red`).                                                            |

**Available HVAC modes:** `off`, `heat`, `cool`, `heat_cool`, `auto`, `dry`, `fan_only`

**Note:** The actual available modes depend on your specific climate entity. The card will automatically show configuration options only for the modes your climate entity supports.

### Available Color Templates

The following color templates are available for `card_template`, `template_on`, and `template_off`:

| Template Name | Color                                                           | Description                         |
| :------------ | :-------------------------------------------------------------- | :---------------------------------- |
| `blue`        | ![#3D5AFE](https://dummyimage.com/15/3d5afe/3d5afe) Blue        | Default blue color scheme `#3D5AFE` |
| `lightblue`   | ![#03A9F4](https://dummyimage.com/15/03a9f4/03a9f4) Light Blue  | Light blue color scheme `#03A9F4`   |
| `red`         | ![#F54436](https://dummyimage.com/15/f54436/f54436) Red         | Red color scheme `#F54436`          |
| `green`       | ![#01C852](https://dummyimage.com/15/01c852/01c852) Green       | Green color scheme `#01C852`        |
| `lightgreen`  | ![#8BC34A](https://dummyimage.com/15/8bc34a/8bc34a) Light Green | Light green color scheme `#8BC34A`  |
| `yellow`      | ![#FF9101](https://dummyimage.com/15/ff9101/ff9101) Yellow      | Yellow/amber color scheme `#FF9101` |
| `purple`      | ![#661FFF](https://dummyimage.com/15/661fff/661fff) Purple      | Purple color scheme `#661FFF`       |
| `orange`      | ![#FF5722](https://dummyimage.com/15/ff5722/ff5722) Orange      | Orange color scheme `#FF5722`       |
| `pink`        | ![#E91E63](https://dummyimage.com/15/e91e63/e91e63) Pink        | Pink color scheme `#E91E63`         |
| `grey`        | ![#9E9E9E](https://dummyimage.com/15/9e9e9e/9e9e9e) Grey        | Grey/neutral color scheme `#9E9E9E` |
| `teal`        | ![#009688](https://dummyimage.com/15/009688/009688) Teal        | Teal color scheme `#009688`         |
| `indigo`      | ![#3F51B5](https://dummyimage.com/15/3f51b5/3f51b5) Indigo      | Indigo color scheme `#3F51B5`       |

These templates use CSS variables that can be customized in your Home Assistant theme. If UI Lovelace Minimalist or another theme is installed which provides the `--color-*` variables, the templates will use these colors. Otherwise, fallback colors are provided (see [src/room-card-minimalist.js](https://github.com/unbekannt3/room-card-minimalist/blob/main/src/room-card-minimalist.js) => const COLOR_TEMPLATES for details).

### YAML Example

```yaml
type: custom:room-card-minimalist
name: Living Room
icon: mdi:sofa
card_template: blue
use_template_color_for_title: true
use_template_color_for_secondary: true
entities_reverse_order: false
secondary: '{{states("sensor.living_room_temperature")}} °C'
tap_action:
  action: navigate
  navigation_path: /lovelace/living-room
hold_action:
  action: more-info
entities:
  - type: entity
    entity: light.living_room_ceiling
    icon: mdi:ceiling-light
    icon_off: mdi:ceiling-light-outline
    on_state: 'on'
    use_light_color: true
    color_off: grey
    tap_action:
      action: toggle
    hold_action:
      action: more-info
  - type: template
    icon: mdi:lightbulb-group
    icon_off: mdi:lightbulb-group-outline
    condition: >-
      {% set lights_on = expand(area_entities('Living Room')) |
      selectattr('domain','eq','light') | selectattr('state','eq','on') | list |
      count %}{% if lights_on > 0 %}{{ lights_on }} lights on{% endif %}
    color_on: yellow
  - type: entity
    entity: binary_sensor.living_room_motion
    on_state: 'on'
    icon: mdi:motion-sensor
    icon_off: mdi:motion-sensor-off
    color_on: green
  - type: entity
    entity: climate.living_room
    icon: mdi:thermostat
    icon_off: mdi:thermostat-off
    # Climate entities use mode-specific configuration:
    color_off: grey
    template_off: grey
    template_heat: red
    template_cool: lightblue
    template_auto: green
```

### Theme customization

Room Card Minimalist works without a theme installed however, I personally use the awesome [Material Design 3 Theme](https://github.com/Nerwyn/material-you-theme) from Nerwyn in combination with [Material You Utilities](https://github.com/Nerwyn/material-you-utilities).

<!-- Badges -->

[hacs-url]: https://github.com/hacs/integration
[hacs-badge]: https://img.shields.io/badge/hacs-default-orange.svg?style=flat-square
[release-badge]: https://img.shields.io/github/v/release/unbekannt3/room-card-minimalist?style=flat-square
[downloads-badge]: https://img.shields.io/github/downloads/unbekannt3/room-card-minimalist/total?style=flat-square
[build-badge]: https://img.shields.io/github/actions/workflow/status/unbekannt3/room-card-minimalist/build.yaml?branch=main&style=flat-square

<!-- References -->

[home-assistant]: https://www.home-assistant.io/
[home-assitant-theme-docs]: https://www.home-assistant.io/integrations/frontend/#defining-themes
[hacs]: https://hacs.xyz
[release-url]: https://github.com/unbekannt3/room-card-minimalist/releases
