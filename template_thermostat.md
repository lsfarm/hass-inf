https://github.com/jcwillox/hass-template-climate/issues/102   \
https://community.home-assistant.io/t/climate-template-set-temperature-help-required/621852/5
  
  ```
# ---->>>> HVAC Infinitive MQTT<<<<----
  - platform: climate_template
    name: Hall Master
    unique_id: 9de8f337-3560-4e61-8df8-de8c7778c6f7
    modes:
      - "off"
      - heat_cool
      - cool
      - heat
      #     - "auto"
      #     - "dry"

    #     - "fan_only"
    fan_modes:
      - auto
      - low
      - "medium"
      - "high"
    min_temp: 60
    max_temp: 80

    # get current temp.
    current_temperature_template: "{{ states('sensor.hvacc_hall_east_temp') }}"

    # get current humidity.
    current_humidity_template: "{{ states('sensor.hall_east_indoor_humidity') }}"

    set_temperature:
      - condition: state
        entity_id: climate.hall_master
        state: "cool"

      - service: climate.set_temperature
        data:
          entity_id:
            - climate.hall_east
            - climate.hall_middle
          target_temp_high: "{{ state_attr('climate.hall_master', 'temperature') | int }}"
          target_temp_low: 60

      - condition: state
        entity_id: climate.hall_master
        state: "heat"
      - service: climate.set_temperature
        data:
          entity_id:
            - climate.hall_east
            - climate.hall_middle
          target_temp_high: 80
          target_temp_low: "{{ state_attr('climate.hall_master', 'temperature') | int }}"
    #temperature: "{{ temperature }}"
    set_fan_mode:
      - service: input_text.set_value
        data:
          entity_id: input_text.hvac_mode_wohnzimmer
          value: "{{ state_attr('climate.test_thermostat', 'fan_mode') }}"

    set_fan_mode:
      - service: input_text.set_value
        data:
          entity_id: input_text.hvac_mode_wohnzimmer
          value: "{{ state_attr('climate.test_thermostat', 'fan_mode') }}"
```
