# Batman Laundry Card for Home Assistant

A fun, Batman-themed `picture-elements` card for Home Assistant to monitor and control your washer and dryer. Displays animated GIFs and PNGs based on appliance states, with text overlays for job status and remaining time.

## Features
- Shows `Batman_Laundry_Empty.png` when both appliances are off.
- Washer: Displays `Batman_Laundry_Clothes.png` when on, with GIFs (`Rinse.gif`, `Sin.gif`, `Spin.gif`) for cycles, and `Batmn_Laundry_Done.gif` when finished.
- Dryer: Shows `Batman_Laundry_Spin.gif` when running alone, and `Batmn_Laundry_Done.gif` when done.
- Text overlays for job states (e.g., "wash," "finished") and completion times in minutes.

## Demo Video
Here’s a short clip of the card in action:

[![batman-laundry-card](https://img.youtube.com/vi/hIn9QnNpZ2Y.0.jpg)](https://www.youtube.com/watch?v=hIn9QnNpZ2Y)

## Files
- `picture-elements.yaml`: The card configuration.
- `configuration.yaml`: Defines `input_text.laundry_label` and template sensors for completion times.
- `images/`: Contains the Batman-themed images (PNG and GIF files) and the demo video.

## Setup Instructions
1. **Add to Home Assistant**:
   - Copy `picture-elements.yaml` into a Lovelace dashboard (*Edit Dashboard > Add Card > Manual*).
   - Add `configuration.yaml` content to your `configuration.yaml` file under `input_text:` and `sensor:` sections.
   - Restart Home Assistant.

2. **Replace Entities**:
   - Update `switch.washer`, `switch.dryer`, `sensor.washer_job_state`, `sensor.dryer_job_state`, `sensor.washer_completion_time_minutes`, and `sensor.dryer_completion_time_minutes` with your own entity names.
   - Ensure your washer/dryer integration provides switch and status sensors.

3. **Host Images**:
   - Place the images in `/config/www/pictures/` on your Home Assistant instance.
   - Replace `your_ip_address` in the YAML with your Home Assistant IP or domain (e.g., `192.168.1.100` or `your-nabu-casa-url`).

4. **Adapt for Generic Smart Devices**:
   - If your devices don’t provide `sensor.washer_completion_time` or `sensor.dryer_completion_time`, modify the template sensors in `configuration.yaml` to use your completion time entities or estimate based on cycle duration (see below).

## Adapting to Any Smart Washer/Dryer
If your devices differ, create these template sensors in `configuration.yaml`:
```yaml
sensor:
  - platform: template
    sensors:
      washer_job_state:
        value_template: >
          {% set status = states('sensor.your_washer_status') %}
          {% if status == 'running' and 'wash' in status.lower() %} wash
          {% elif status == 'running' and 'rinse' in status.lower() %} rinse
          {% elif status == 'running' and 'spin' in status.lower() %} spin
          {% elif status == 'completed' %} finish
          {% else %} idle
          {% endif %}
      dryer_job_state:
        value_template: >
          {% set status = states('sensor.your_dryer_status') %}
          {% if status in ['completed', 'finished'] %} finished
          {% elif status == 'running' %} drying
          {% else %} idle
          {% endif %}
      washer_completion_time_minutes:
        value_template: >
          {% set completion_time = states('sensor.your_washer_end_time') | default('') %}
          {% if completion_time %}
            {% set completion_ts = as_timestamp(completion_time) %}
            {% set current_ts = as_timestamp(now()) %}
            {{ ((completion_ts - current_ts) / 60) | round(0) if completion_ts > current_ts else 0 }}
          {% else %}
            {% set duration = 45 %}
            {% if is_state('switch.your_washer', 'on') %}
              {{ duration - (as_timestamp(now()) - as_timestamp(states('switch.your_washer').last_changed)) / 60 | round(0) }}
            {% else %}
              0
            {% endif %}
          {% endif %}
      dryer_completion_time_minutes:
        value_template: >
          {% set completion_time = states('sensor.your_dryer_end_time') | default('') %}
          {% if completion_time %}
            {% set completion_ts = as_timestamp(completion_time) %}
            {% set current_ts = as_timestamp(now()) %}
            {{ ((completion_ts - current_ts) / 60) | round(0) if completion_ts > current_ts else 0 }}
          {% else %}
            {% set duration = 60 %}
            {% if is_state('switch.your_dryer', 'on') %}
              {{ duration - (as_timestamp(now()) - as_timestamp(states('switch.your_dryer').last_changed)) / 60 | round(0) }}
            {% else %}
              0
            {% endif %}
          {% endif %}
