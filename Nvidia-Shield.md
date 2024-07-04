# **Instructions for Nvidia Shield**

For some reason the method doesn't work for the Nvidia Shield. The timers update randomly, every 5-10 seconds, or more. I had to come up with a different solution. Below are three different sensors you will need to create for media played from an Nvidia Shield. Maybe other video media players too.

## Sensors

### **Shield Media Duration**

```
{% set md = state_attr('media_player.shield', 'media_duration') %}
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

### **Shield Media Position**

```
{% set mp = state_attr('media_player.shield', 'media_position') %}
{% set mpua = state_attr('media_player.shield', 'media_position_updated_at') %}
{% if mp == none %}
  00:00
{% elif (mp | int) < 3600 %}
  {{ ( as_timestamp(now()) - as_timestamp(mpua) + mp ) | int | timestamp_custom('%M:%S') }}
{% elif (mp | int) > 3600 %}
  {{ ( as_timestamp(now()) - as_timestamp(mpua) + mp ) | int | timestamp_custom('%-H:%M:%S', false) }}
  {% else %}
  00:00
{% endif %}
```

### **Shield Media Position Integer**

```
{% set mp = state_attr('media_player.shield', 'media_position') %}
{% set mpua = state_attr('media_player.shield', 'media_position_updated_at') %}
  {{ ( as_timestamp(now()) - as_timestamp(mpua) + mp ) }}
```

### **Shield Media Remaining**

```
{% set md = state_attr('media_player.shield', 'media_duration') %}
{% set mp = state_attr('media_player.shield', 'media_position') %}
{% set mpi = states('sensor.shield_media_position_integer') %}
{% if md == none %}
  -00:00 |
{% elif (md | int - mpi | int) < 3600 %}
  -{{ (md | int - mpi | int) | timestamp_custom('%M:%S') }} |
{% elif (md | int - mpi | int) > 3600 %}
  -{{ (md | int - mpi | int) | timestamp_custom('%-H:%M:%S', false) }} |
  {% else %}
  -00:00 |
{% endif %}
```

## Automation

The new 'Shield Media Position Integer' sensor needs to be added to the automation

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
        entity_id: media_player.shield
        state: playing
action:
  - service: homeassistant.update_entity
    data: {}
    target:
      entity_id:
        - sensor.shield_media_position
        - sensor.shield_media_position_integer
```
