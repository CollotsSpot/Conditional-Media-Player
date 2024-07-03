# **Media Player Card**

![Screenshot_20240703-052457](https://github.com/CollotsSpot/Conditional-Media-Player/assets/62449370/548272ef-12af-4693-b647-a6d2a85d00b3)

Here is my Media Player card for home assistant.

Note: I am no expert at coding or Home Assistant. this is my first GitHub project. I'm sure there more elegant ways of achieving this card. I would love to have it in HACS, but do not have the knowledge to do so. If you have any ideas for improvements feel free to comment.

If you like the card as much as i do, you won't mind setting up a few things to get the card working...

## **Prerequisites:**

[Button Card](https://github.com/custom-cards/button-card)

[Decluttering Card](https://github.com/custom-cards/decluttering-card)

## Automation & Sensors

you will need to create 3 sensor templates per media player and 1 automation for the card to work. To create a sensor template go to Settings > Devices & Services > Helpers > Create Helper > Template > Template a sensor. 

the following sensors are too get the process bar and timers working. replace name and media_player entity to your own. the name of the sensor is important.

### **Bedroom Media Duration**

```
{% set md = state_attr('media_player.bedroom', 'media_duration') %}
{% if md == none %}
  00:00
{% elif (md | int) < 3600 %}
  {{ (md | int) | timestamp_custom('%M:%S') }}
{% elif (md | int) > 3600 %}
  {{ (md | int) | timestamp_custom('%-H:%M:%S', false) }}
{% else %}
  00:00
{% endif %}
```

### **Bedroom Media Position**

```
{% set mp = state_attr('media_player.bedroom_hifi', 'media_position') %}
{% if mp == none %}
  00:00
{% elif (mp | int) < 3600 %}
  {{ (mp | int) | timestamp_custom('%M:%S') }}
{% elif (mp | int) > 3600 %}
  {{ (mp | int) | timestamp_custom('%-H:%M:%S', false) }}
{% else %}
  00:00
{% endif %}
```

### **Bedroom Media Remaining**

```
{% set md = state_attr('media_player.bedroom_hifi', 'media_duration') %}
{% set mp = state_attr('media_player.bedroom_hifi', 'media_position') %}
{% if mp == none %}
  -00:00 |
{% elif (md | int - mp | int) < 3600 %}
  -{{ (md | int - mp | int) | timestamp_custom('%M:%S') }} |
{% elif (md | int - mp | int) > 3600 %}
  -{{ (md | int - mp | int) | timestamp_custom('%-H:%M:%S', false) }} |
{% else %}
  -00:00 |
{% endif %}
```