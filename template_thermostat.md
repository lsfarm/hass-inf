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
      - service: input_number.set_value
        data:
          entity_id: input_number.hvacc_eco_low
          value: "{{ state_attr('climate.test_thermostat', 'temperature') }}"
    set_fan_mode:
      - service: input_text.set_value
        data:
          entity_id: input_text.hvac_mode_wohnzimmer
          value: "{{ state_attr('climate.test_thermostat', 'fan_mode') }}"
```
