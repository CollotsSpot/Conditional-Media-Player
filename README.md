# **Conditional Media Player Card for Home Assistant**

![Screenshot_20240703-052457](https://github.com/CollotsSpot/Conditional-Media-Player/assets/62449370/548272ef-12af-4693-b647-a6d2a85d00b3)


https://github.com/CollotsSpot/Conditional-Media-Player/assets/62449370/36a911b5-783a-4431-b95a-8faf1a6e5a70



Here is my Media Player card for Home Assistant.

Note: I am no expert at coding or Home Assistant. this is my first GitHub project. I'm sure there are more elegant ways of achieving this card. I would love to have it in HACS but do not have the knowledge to do so. If you have any ideas for improvements feel free to comment.

If you like the card as much as I do, you won't mind setting up a few things to get the card working...

## **Prerequisites:**

[Button Card](https://github.com/custom-cards/button-card)

[My Cards Bundle](https://github.com/AnthonMS/my-cards)

[Decluttering Card](https://github.com/custom-cards/decluttering-card)


## Automation

This automation updates the sensors every second.

To create the Automation go to Settings > Automation & Scenes > Create Automation > Create New Automation > Three dot menu > Edit in YAML > Paste this code

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

You should now have the media player working. The Card will not show on the dashboard when the media_player is off or idle. This is normal behavior. The card will always be shown when editing the Dashboard.




## Note

If you want to create a media player for watching Movies and TV Shows etc, and find the timers are not updating, like I did, you will need to create some sensors. Below are instructions on how to do this.

[Instructions for Nvidia Shield](Nvidia-Shield.md)
