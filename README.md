# Power Threshold Auto Switch Off

## Overview

The DEV blueprint is designed to monitor the total power consumption and automatically turn off specified switches when the consumption exceeds a predefined threshold. It includes options for custom actions and notifications.

You can import this blueprint here:
<a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fdavpirelli%2Fha-switch-power-threshold%2F"><img src="https://my.home-assistant.io/badges/blueprint_import.svg"></a>

<img width="1055" alt="image" src="https://github.com/davpirelli/ha-switch-power-threshold/assets/6840116/0435cd96-646c-4521-a8db-937b00374e90">

## Configuration

### Important! You need a "total power" sensor!
In my case, i used a custom template that sum all of my *_power data. Here's how you can do that:
1. With VSCode Server extension or File Editor, open the configuration.yaml file (the one inside the /config folder) and paste this code:
   <pre><code>- platform: template
    sensors:
      total_power:
        friendly_name: "Total Power"
        entity_id:
          - sensor.sensorname1_power
          - sensor.sensorname2_power
        value_template: "{{ (states('sensor.sensorname1_power')|float + states('sensor.sensorname2_power')|float)|round(3) }}"
        unit_of_measurement: "W"</code></pre>

### Inputs

1. **Total Power (W)**
   - *Description:* Select the sensor that represents the sum of all the switches' consumption in watts.

2. **Maximum Threshold in W**
   - *Description:* Enter the threshold in watts. If the total power consumption exceeds this threshold, the cascade shutdown of switches will be triggered.

3. **Check Delay (seconds)**
   - *Description:* Enter the delay in seconds that must pass after the first switch turns off. If, after this delay, the consumption is still above the threshold, the shutdown sequence repeats with the next switch.

4. **Devices to Notify (Push)**
   - *Description:* A list of devices with the HomeAssistant app installed to be notified when the threshold is exceeded.

5. **List of Switches to Turn Off**
   - *Description:* Insert the switches in order of priority. The first switch in the list will be the first to be turned off when the threshold is exceeded.

6. **Custom Actions**
   - *Description:* Specify any additional custom actions to perform when the threshold is reached (e.g., notifications, TTS announcements).

### Variables

- **Threshold Variable**
  - *Description:* Represents the threshold value set by the user.

- **Total Power Variable**
  - *Description:* Represents the total power sensor selected by the user.

- **Switches Variable**
  - *Description:* Represents the list of switches selected by the user.

- **Delay Variable**
  - *Description:* Represents the delay value set by the user.

### Trigger

- *Platform:* Numeric state
- *Entity ID:* Total power sensor
- *Above:* Threshold value

### Actions

1. **Notification Sequence**
   - *Description:* Sends notifications to the specified devices when the threshold is exceeded.

2. **Custom Actions**
   - *Description:* Performs any additional custom actions specified by the user.

3. **Switch Shutdown Sequence**
   - *Description:* Iterates through the list of switches, turning them off with a delay specified by the user.

4. **Notification Sequence (End)**
   - *Description:* Sends notifications to the specified devices when all switches have been turned off.

## Example Usage

1. **Configuration:**
   - Set the total power sensor, threshold, check delay, devices to notify, switches, and any custom actions.

2. **Trigger:**
   - When the total power exceeds the defined threshold, the automation is triggered.

3. **Actions:**
   - Notifications are sent to specified devices.
   - Custom actions are executed.
   - Switches are turned off sequentially with a delay.
   - Notifications are sent to specified devices when all switches have been turned off.

4. **End Result:**
   - The automation responds to high power consumption by notifying users, executing custom actions, and turning off switches in a cascading fashion.

