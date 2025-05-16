https://github.com/jcwillox/hass-template-climate/issues/102   \
https://community.home-assistant.io/t/climate-template-set-temperature-help-required/621852/5
  
 ```
# ---->>>> HVAC Infinitive ACD Repo<<<<----
# >Fell Hall West
climate:
  - platform: infinitive
    host: 192.168.15.99
    port: 8080
  #doesnt seem to work   friendly_name: hvac_fhallwest

  # ---->>>> HVAC Infinitive MQTT<<<<----
  - platform: climate_template
    name: Hall Master
    unique_id: dd8a6da3-3eac-4f22-8b81-734f1eb97efe
    modes:
      - "off"
      - heat_cool
      - cool
      - heat
      #     - "auto"
      #     - "dry"

    #     - "fan_only"
    # set back to default -"auto" -"low" if fan mode errors
    fan_modes:
      - auto
      - low
      - medium
      - high
    min_temp: 60
    max_temp: 80
    target_temperature_high_template: 80
    target_temperature_low_template: 60

    current_temperature_template: "{{ states('sensor.hvacc_hall_east_temp') }}"
    current_humidity_template: "{{ states('sensor.hall_east_indoor_humidity') }}"

    set_hvac_mode:
      - variables:
          hall_mode: "{{ states('climate.hall_master') }}"
      - service: climate.set_hvac_mode
        data:
          entity_id:
            - climate.hall_east
            - climate.hall_middle
          hvac_mode: "{{ hall_mode if hall_mode != 'heat_cool' else 'auto' }}"

    set_temperature:
      - variables:
          hall_temp: "{{ state_attr('climate.hall_master', 'temperature') | int }}"
          hall_temp_low: "{{ state_attr('climate.hall_master', 'target_temp_low') | int }}"
          hall_temp_high: "{{ state_attr('climate.hall_master', 'target_temp_high') | int }}"
          hall_mode: "{{ states('climate.hall_master') }}"
          eco_low: "{{ states('input_number.hvacc_eco_low') }}"
          eco_hi: "{{ states('input_number.hvacc_eco_hi') }}"
      - service: climate.set_temperature
        data:
          entity_id:
            - climate.hall_east
            - climate.hall_middle
          target_temp_high: "{{ hall_temp if hall_mode == 'cool' else hall_temp_high if hall_mode == 'heat_cool' else eco_hi }}"
          target_temp_low: "{{ hall_temp if hall_mode == 'heat' else hall_temp_low if hall_mode == 'heat_cool' else eco_low }}"

    set_fan_mode:
      - service: climate.set_fan_mode
        data:
          entity_id:
            - climate.hall_east
            - climate.hall_middle
          fan_mode: "{{ state_attr('climate.hall_master', 'fan_mode') }}"
```
