# **Conditional Media Player Card for Home Assistant**

![Screenshot_20240703-052457](https://github.com/CollotsSpot/Conditional-Media-Player/assets/62449370/548272ef-12af-4693-b647-a6d2a85d00b3)


https://github.com/CollotsSpot/Conditional-Media-Player/assets/62449370/36a911b5-783a-4431-b95a-8faf1a6e5a70



Here is my Media Player card for home assistant.

Note: I am no expert at coding or Home Assistant. this is my first GitHub project. I'm sure there more elegant ways of achieving this card. I would love to have it in HACS, but do not have the knowledge to do so. If you have any ideas for improvements feel free to comment.

If you like the card as much as i do, you won't mind setting up a few things to get the card working...

## **Prerequisites:**

[Button Card](https://github.com/custom-cards/button-card)

[My Cards Bundle](https://github.com/custom-cards/button-card)

[Decluttering Card](https://github.com/custom-cards/decluttering-card)

##  Sensors & Automation

you will need to create 3 sensor templates per media player and 1 automation for the card to work.

## Sensors

The following sensors provide the timers infomation. replace name and media_player entity to your own. the name of the sensor is important.

To create a sensor template go to Settings > Devices & Services > Helpers > Create Helper > Template > Template a sensor > Paste this code


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
{% set mp = state_attr('media_player.bedroom', 'media_position') %}
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
{% set md = state_attr('media_player.bedroom', 'media_duration') %}
{% set mp = state_attr('media_player.bedroom', 'media_position') %}
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

## Automation

This automation updates the sensors every second.

To create the Automation go to Settings > Automations & Scenes > Create Automation > Create New Automation > Three dot menu > Edit in YAML > Paste this code

(Replace media_player entities to your own, and add more if required)

```
alias: Update Media Players
description: ""
trigger:
  - platform: time_pattern
    seconds: /1
condition:
  - condition: or
    conditions:
      - condition: state
        entity_id: media_player.bedroom
        state: playing
action:
  - service: homeassistant.update_entity
    data: {}
    target:
      entity_id:
        - media_player.bedroom
```

## Decluttering Template

On your Dashboard go to Edit Dashboard > Three dot menu > raw configuration editor. Make space at the top and paste the contents of the [Decluttering Template](decluttering-template.yaml)

## Conditional Decluttering Card

On your Dashboard go to Add Card > Manual > paste code and replace entity, name, colors and sensor to your own liking

```
type: conditional
conditions:
  - condition: state
    entity: media_player.bedroom
    state_not: 'off'
  - condition: state
    entity: media_player.bedroom
    state_not: idle
card:
  type: custom:decluttering-card
  template: media_player
  variables:
    - entity: media_player.bedroom
    - name: Bedroom
    - primary-color: '#6F8081'
    - secondary-color: rgba(96,114,116,0.6)
    - button-background-color: rgba(96,114,116,0.2)
    - sensor: bedroom
```

You should now have the media player working. The Card will not show on the dashboard when the media_player is off or idle. This is normal behaviour. The card will always be shown when editing the Dashboard.




## Note 

If you want to create a media player for watching Movies and TV Shows ect, and find the timers are not updating, like I did, you will need to create some different sensors. Below are instructions on how to do this.

[Instructions for Nvidia Shield](nvidia-shield.md)