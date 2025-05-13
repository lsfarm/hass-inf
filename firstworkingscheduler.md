```
type: custom:scheduler-card
title: Fellowship Hall Schedule
customize:
  script.hall_climate:
    actions:
      - service: script.hall_climate
        name: Auto Set
        icon: mdi:thermometer-auto
        variables:
          hvac_mode:
            name: HVAC mode
            options:
              - value: heat
                icon: mdi:fire
              - value: cool
                icon: mdi:snowflake
              - value: "off"
                icon: mdi:power
          target_temp_low:
            name: Low Setpoint
            min: 55
            max: 80
          target_temp_high:
            name: Hi Setpoint
            min: 60
            max: 85
          fan_mode:
            name: Fan mode
            options:
              - value: auto
                icon: mdi:fan-auto
              - value: high
                icon: mdi:fan-speed-3
              - value: quiet
                icon: mdi:fan-speed
time_step: 1
```
```
alias: Hall Climate
description: Sets climate parameters for scheduler-card
variables:
  target_entity: climate.hall_east
sequence:
  - data:
      target_temp_high: "{{ target_temp_high | float }}"
      target_temp_low: "{{ target_temp_low | float }}"
    target:
      entity_id: "{{ target_entity }}"
    action: climate.set_temperature
  - delay:
      seconds: 1
  - target:
      entity_id: "{{ target_entity }}"
    data:
      fan_mode: "{{ fan_mode }}"
    action: climate.set_fan_mode
  - delay:
      seconds: 1
  - target:
      entity_id: "{{ target_entity }}"
    data:
      hvac_mode: "{{ hvac_mode }}"
    action: climate.set_hvac_mode
mode: single
icon: mdi:thermometer-auto
```
