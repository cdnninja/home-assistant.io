---
description: Instructions on how to set up VeSync devices with Home Assistant.
ha_release: 0.66
ha_domain: vesync
---

The **VeSync** {% term integration %} connects Home Assistant to the devices in your [VeSync](https://www.vesync.com) account. VeSync is the app and cloud service used by Levoit air purifiers, humidifiers, and fans, Etekcity smart plugs, switches, and bulbs, and Cosori air fryers.

Once your devices are in Home Assistant, you can control them from your dashboards and use them in automations. For example, you can:

- Turn up your air purifier when the PM2.5 level rises, and switch it to sleep mode at bedtime.
- Get a notification when your humidifier runs low on water.
- Track the energy use of appliances plugged into a VeSync smart plug.
- Get a notification when your air fryer has finished cooking.

## Supported devices

The following devices are known to be supported by the integration. Each model name can cover several regional model numbers. Devices that aren't listed here might not work, or might only be partly supported.

### Air purifiers

- Levoit Core 200S (Core200S, LAP-C201S, LAP-C202S)
- Levoit Core 300S (Core300S, LAP-C301S, LAP-C302S)
- Levoit Core 400S (Core400S, LAP-C401S)
- Levoit Core 600S (Core600S, LAP-C601S)
- Levoit EverestAir (LAP-EL551S)
- Levoit LV-PUR131S and LV-RH131S
- Levoit Sprout Air (LAP-B851S)
- Levoit Vital 100S (LAP-V102S)
- Levoit Vital 200S (LAP-V201S)

### Fans

- Levoit Classic 42-Inch Tower Fan (LTF-F422S)
- Levoit Pedestal Fan (LPF-R432S)

### Humidifiers

- Levoit Classic 200S
- Levoit Classic 300S (LUH-A601S)
- Levoit Dual 200S (LUH-D301S)
- Levoit LV600S (LUH-A602S, LUH-A603S)
- Levoit OasisMist 4.5L (LUH-O451S, LUH-O601S)
- Levoit OasisMist 1000S (LUH-M101S)
- Levoit Sprout Humidifier (LEH-B381S)
- Levoit Superior 6000S (LEH-S601S, LEH-S602S)

### Air fryers

- Cosori Smart 5.8 Qt. Air Fryer and Pro models (CS137-AF, CS158-AF)
- Cosori TurboBlaze 6 Qt. Air Fryer (CAF-DC601S)
- Cosori Dual Blaze 6.8 Qt. Air Fryer (CAF-P583S)
- Cosori Dual Air Fryer (CAF-TF101S, CAF-TF102S)
- Cosori Turbo Tower Pro Smart Air Fryer (CAF-DC111S)

### Outlets and smart plugs

- Etekcity 7A WiFi Outlet (ESW01-USA, round)
- Etekcity 10A WiFi Outlet (ESW01-EU, ESW03, ESW10-USA, ESW10-EU)
- Etekcity 15A WiFi Outlet (ESW15-USA, rectangular)
- Etekcity Outdoor Plug (ESO15-TB)
- VeSync smart plugs (BSDOG01, BSDOG02, WHOGPLUG, WYLDR16A1081, and related models)

### Wall switches

- Etekcity Light Switch (ESWL01)
- Etekcity 3-way Light Switch (ESWL03)
- Etekcity Dimmer Switch (ESWD16)

### Light bulbs

- Etekcity Soft White Light Bulb (ESL100)
- Etekcity Cool-to-Warm White Light Bulb (ESL100CW)
- Etekcity Multicolor Bulb (ESL100MC)
- Valceno WiFi Bulb (XYD0001)

## Unsupported devices

The following devices are not supported by the integration:

- Thermostats, such as the Aura Thermostat (LTM-A401S). Home Assistant doesn't provide controls for them.
- Devices that aren't added to your VeSync account

## Prerequisites

1. Install the VeSync app on your phone.
2. Create a VeSync account, or sign in to your existing account.
3. Add your devices to the VeSync app, and check that you can control them from the app.
4. If two-factor authentication is turned on for your VeSync account, turn it off. The integration can't sign in to accounts that use two-factor authentication.

{% include integrations/config_flow.md %}

If a Levoit device is connected to your network, Home Assistant might discover it and suggest that you set up VeSync.

{% configuration_basic %}
Email:
  description: "The email address of your VeSync account. This is the same email address you use to sign in to the VeSync app."
Password:
  description: "The password of your VeSync account."
{% endconfiguration_basic %}

Home Assistant adds all supported devices in your VeSync account. Each device gets its own {% term device %} with the {% term entities %} that match its features.

## Supported functionality

The entities that a device gets depend on its model. Some entities are only created when the device reports that feature.

### Binary sensors

- **Low water**
  - **Description**: Shows a problem when the water tank needs a refill.
  - **Available for**: Humidifiers
- **Water tank lifted**
  - **Description**: Shows a problem when the water tank is removed from the humidifier.
  - **Available for**: Humidifiers that report the tank position
- **Sync baskets**
  - **Description**: Shows if the two baskets are synced, so they finish cooking at the same time.
  - **Available for**: Dual-basket air fryers

### Buttons

- **Reset filter**
  - **Description**: Resets the filter lifetime to 100%. Press it after you replace the filter.
  - **Available for**: Core 200S
- **Stop cooking**
  - **Description**: Ends the current cooking program.
  - **Available for**: Air fryers
  - **Remarks**: Dual-basket air fryers have one button for each basket, like **Basket 1 stop cooking**.
- **Pause cooking** and **Resume cooking**
  - **Description**: Pauses the current cooking program, or resumes it.
  - **Available for**: Air fryers that can pause, like the CS158-AF series and the Turbo Tower Pro

### Fans

Each air purifier and fan gets a fan entity, named after the device.

- **Fan**
  - **Description**: Turns the device on or off, sets the fan speed, and sets the preset mode.
  - **Available for**: Air purifiers and fans
  - **Preset modes**: Depending on the model, **Auto**, **Eco**, **Normal**, **Pet**, **Sleep**, and **Turbo**
  - **Remarks**:
    - The number of speed steps depends on the model. For example, the Core 400S has 4 speeds, and the tower fan has 12.
    - When you set a speed, the device switches to manual mode.
    - Oscillation is available for the tower fan and the pedestal fan. On the pedestal fan, it turns horizontal and vertical oscillation on or off together.
    - The fan entity also shows the current VeSync mode, display status, child lock status, night light status, and active time as attributes, if the model reports them.

### Humidifiers

Each humidifier gets a humidifier entity, named after the device.

- **Humidifier**
  - **Description**: Turns the humidifier on or off, sets the target humidity, and sets the mode. It also shows the current humidity.
  - **Available for**: Humidifiers
  - **Modes**: Depending on the model, **Auto**, **Humidity**, **Normal**, and **Sleep**. **Normal** is the manual mode in the VeSync app.

### Lights

- **Light**
  - **Description**: Turns the light on or off, and sets the brightness.
  - **Available for**: Light bulbs and the ESWD16 dimmer switch
  - **Remarks**: The ESL100CW and other bulbs with adjustable white light also let you set the color temperature, from 2700 K to 6500 K.
- **Night light**
  - **Description**: Turns the color night light on or off, and sets its color and brightness.
  - **Available for**: Humidifiers with a color night light, like the OasisMist 4.5L (LUH-O451S-WEU)

### Numbers

- **Mist level**
  - **Description**: Sets how much mist the humidifier produces. The range depends on the model, for example 1 to 9.
  - **Available for**: Humidifiers
- **Warm mist level**
  - **Description**: Sets the warm mist level. `0` turns warm mist off.
  - **Available for**: Humidifiers with warm mist, like the LV600S and the OasisMist 4.5L

### Selects

- **Night light level**
  - **Description**: Sets the brightness of the night light.
  - **Options**:
    - Humidifiers: **Off**, **Dim**, **Bright**
    - Air purifiers: **Off**, **Dim**, **On**
    - Outlets: **Off**, **On**, **Auto**
  - **Available for**: Classic 300S, OasisMist 1000S (EU model), Sprout Humidifier, Core 200S, Sprout Air, and the ESW15-USA outlet
- **Auto mode preference**
  - **Description**: Sets how the air purifier behaves in auto mode.
  - **Options**: **Default**, **Efficient**, **Quiet**
  - **Available for**: Core series, Vital series, EverestAir, and Sprout Air air purifiers

### Sensors

Air purifiers and humidifiers:

- **Filter lifetime**
  - **Description**: The remaining lifetime of the filter, in percent.
  - **Available for**: Air purifiers that report it
- **HEPA filter lifetime**
  - **Description**: The remaining lifetime of the HEPA filter, in percent.
  - **Available for**: Humidifiers with a HEPA filter, like the Sprout Humidifier
- **Air quality**
  - **Description**: The air quality level reported by the device: **Excellent**, **Good**, **Moderate**, or **Poor**.
  - **Available for**: Air purifiers with an air quality sensor
- **PM1**, **PM2.5**, and **PM10**
  - **Description**: The concentration of particulate matter, in µg/m³.
  - **Available for**: Air purifiers that report it. Most models with an air quality sensor report PM2.5. Some newer models also report PM1 and PM10.
- **Carbon dioxide** and **Volatile organic compounds**
  - **Description**: The carbon dioxide level in ppm, and the volatile organic compounds level.
  - **Available for**: Sprout Air
- **Humidity**
  - **Description**: The current humidity, in percent.
  - **Available for**: Humidifiers
- **Temperature**
  - **Description**: The current room temperature.
  - **Available for**: Humidifiers and fans that report it, like the Superior 6000S, the Sprout Humidifier, and the pedestal fan

Outlets and smart plugs:

- **Current power**
  - **Description**: The power the plugged-in device uses right now, in W.
- **Energy use today**
  - **Description**: The energy used today, in kWh.
- **Energy use weekly**, **Energy use monthly**, and **Energy use yearly**
  - **Description**: The energy used this week, month, and year, in kWh.
- **Current voltage**
  - **Description**: The voltage at the outlet, in V. This is a diagnostic entity.
- **Remarks**: These sensors are created for all outlets. If your model doesn't measure energy, they show as unknown. You can add the energy sensors to the [energy dashboard](/docs/energy/individual-devices/).

Air fryers:

- **Cooking status**
  - **Description**: What the air fryer is doing, like **Preheating**, **Cooking**, **Cooking finished**, **Drawer pulled out**, or **Standby**.
- **Current temperature**
  - **Description**: The temperature inside the air fryer.
- **Cooking set temperature** and **Cooking set time**
  - **Description**: The temperature and time set for the current cooking program.
- **Cooking end**
  - **Description**: The time the current cooking program finishes. It shows as unknown while the air fryer is paused or not cooking.
- **Preheating set time**
  - **Description**: The time set for preheating.
  - **Available for**: Air fryers that can preheat
- **Remarks**: Dual-basket air fryers have these sensors for each basket, like **Basket 1 cooking status**. Temperatures use the unit set on the air fryer.

### Switches

- **On/off switch**
  - **Description**: Turns the outlet or wall switch on or off.
  - **Available for**: Outlets, smart plugs, and wall switches without dimming
  - **Remarks**: This switch is named after the device.
- **Display**
  - **Description**: Turns the display of the device on or off.
  - **Available for**: Air purifiers, fans, and humidifiers that have a display
- **Child lock**
  - **Description**: Locks the buttons on the device.
  - **Available for**: Devices that have a child lock
- **Auto Off**
  - **Description**: Turns the humidifier off when it reaches the target humidity, and on again when the humidity drops below the target.
  - **Available for**: Humidifiers that support it
- **Enable drying mode while power is off**
  - **Description**: Lets the humidifier run its drying mode after you turn it off.
  - **Available for**: Superior 6000S and Sprout Humidifier
- **Light detection**
  - **Description**: Turns the light detection of the air purifier on or off. With light detection, the air purifier responds to the light level in the room.
  - **Available for**: Vital 200S and EverestAir
- **Mute**
  - **Description**: Turns the sounds of the fan off.
  - **Available for**: Classic 42-Inch Tower Fan

### Updates

- **Firmware**
  - **Description**: Shows the installed firmware version, and if a newer version is available.
  - **Available for**: All devices
  - **Remarks**: Install firmware updates from the VeSync app.

{% include integrations/actions.md %}

## VeSync automation examples

You can combine VeSync devices with other devices and services in Home Assistant. Here are a few ideas to get you started.

{% include docs/paste_yaml_tip.md %}

### Automation: Get a notification when the humidifier runs low on water

Know when it's time to refill the water tank, even when you're not in the room.

- **Trigger**: Entity: **Bedroom humidifier Low water** changes to **Problem**
- **Action**: Send a notification message
  - **Target**: My Device (`notify.my_device`)

{% details "YAML example for a low water notification" %}

{% example %}
automation: |
  alias: "Notify when the humidifier runs low on water"
  triggers:
    - trigger: state
      entity_id: binary_sensor.bedroom_humidifier_low_water
      to: "on"
  actions:
    - action: notify.send_message
      target:
        entity_id: notify.my_device
      data:
        message: >
          The bedroom humidifier is low on water. Refill the tank to keep
          it running.
{% endexample %}

{% enddetails %}

### Automation: Run the air purifier at full speed when the air quality drops

When the PM2.5 level stays high for a few minutes, for example while you're cooking, set the air purifier to its highest speed.

- **Trigger**: Numeric state: **Living room purifier PM2.5** above `35` for 5 minutes
- **Action**: Set fan speed
  - **Target**: Living room purifier (`fan.living_room_purifier`)
  - **Speed**: `100`

{% details "YAML example for running the air purifier at full speed" %}

{% example %}
automation: |
  alias: "Run the air purifier at full speed when PM2.5 is high"
  triggers:
    - trigger: numeric_state
      entity_id: sensor.living_room_purifier_pm2_5
      above: 35
      for:
        minutes: 5
  actions:
    - action: fan.set_percentage
      target:
        entity_id: fan.living_room_purifier
      data:
        percentage: 100
{% endexample %}

{% enddetails %}

### Automation: Switch the air purifier to sleep mode at bedtime

Use the sleep mode of your air purifier for a quieter night.

- **Trigger**: Time: `22:00`
- **Action**: Set fan preset mode
  - **Target**: Bedroom purifier (`fan.bedroom_purifier`)
  - **Preset mode**: `sleep`

{% details "YAML example for switching to sleep mode at bedtime" %}

{% example %}
automation: |
  alias: "Switch the air purifier to sleep mode at bedtime"
  triggers:
    - trigger: time
      at: "22:00:00"
  actions:
    - action: fan.set_preset_mode
      target:
        entity_id: fan.bedroom_purifier
      data:
        preset_mode: sleep
{% endexample %}

{% enddetails %}

### Automation: Get a notification when the air fryer has finished cooking

Step away from the kitchen and get a message on your phone when your food is ready.

- **Trigger**: Entity: **Air fryer Cooking status** changes to **Cooking finished**
- **Action**: Send a notification message
  - **Target**: My Device (`notify.my_device`)

{% details "YAML example for an air fryer notification" %}

{% example %}
automation: |
  alias: "Notify when the air fryer has finished cooking"
  triggers:
    - trigger: state
      entity_id: sensor.air_fryer_cooking_status
      to: cooking_end
  actions:
    - action: notify.send_message
      target:
        entity_id: notify.my_device
      data:
        message: "Your air fryer has finished cooking."
{% endexample %}

{% enddetails %}

## Data updates

The VeSync integration {% term polling polls %} the VeSync cloud for the state of all your devices every 60 seconds. This interval keeps the integration within the daily request limit of the VeSync service.

Some data updates less often:

- **Energy use weekly**, **Energy use monthly**, and **Energy use yearly** update every 6 hours.
- The **Firmware** update entities check for new firmware when the integration starts, for example when Home Assistant starts or when you reload the integration.

When you control a device from Home Assistant, the command goes through the VeSync cloud, and Home Assistant shows the new state right away.

## Known limitations

- The integration needs an internet connection and the VeSync cloud. It can't control devices over your local network.
- Accounts that use two-factor authentication aren't supported.
- New devices that you add to the VeSync app aren't added to Home Assistant automatically. To add them, use the [**VeSync: Update devices**](/actions/vesync.update_devices/) action, or reload the integration.
- Devices that you remove from the VeSync app aren't removed from Home Assistant automatically. You can [delete them](#removing-a-device-from-home-assistant) yourself.
- If you set up more than one VeSync account, the **VeSync: Update devices** action only checks the first account. To add new devices from another account, reload that integration entry.
- You can't start a cooking program on an air fryer from Home Assistant. You can follow the cooking progress, stop cooking, and on some models pause and resume.
- Multicolor bulbs can't change color from Home Assistant. You can control their brightness, and the color temperature if the bulb supports it.
- The indicator light and color backlight of the ESWD16 dimmer switch can't be controlled from Home Assistant.
- The fan speed of an air purifier or fan shows as unknown while the device is in a preset mode, like **Auto** or **Sleep**. The Vital series, EverestAir, and Sprout Air show their speed in all modes.
- Firmware updates can't be installed from Home Assistant. Use the VeSync app instead.

## Troubleshooting

{% details "Setup fails with \"Invalid authentication\"" %}

### Symptom: "Invalid authentication"

When you try to set up the integration, the form shows the message "Invalid authentication".

#### Description

The VeSync cloud didn't accept the email address and password. This can happen when there is a typo in the email address or password, or when two-factor authentication is turned on for your account.

#### Resolution

1. Sign out of the VeSync app, and sign in again with the same email address and password. This confirms that your credentials work.
2. If two-factor authentication is turned on for your account, turn it off in the VeSync app.
3. Try to set up the integration again.

{% enddetails %}

{% details "Home Assistant asks you to reauthenticate VeSync" %}

### Symptom: A repair or notification asks you to reauthenticate

Home Assistant shows that the VeSync integration needs your attention, and asks for your email address and password.

#### Description

The VeSync cloud no longer accepts the stored credentials. This usually happens after you change the password of your VeSync account.

#### Resolution

1. Go to {% my integrations title="**Settings** > **Devices & services**" %}.
2. Select the reauthentication prompt for VeSync.
   - If there is no prompt, select **VeSync**, then select the three dots {% icon "mdi:dots-vertical" %} menu next to the entry, and select **Reconfigure**.
3. Enter the email address and the new password of your VeSync account, and select **Submit**.

The integration reloads with the new credentials, and keeps your devices and entities. You can only sign in with the same VeSync account. To use another account, remove the integration and set it up again.

{% enddetails %}

{% details "A new device doesn't show up in Home Assistant" %}

### Symptom: A device in the VeSync app is missing in Home Assistant

You added a device to the VeSync app, but it doesn't show up in Home Assistant.

#### Description

The integration only looks for new devices when it starts. Devices that you add later are not picked up automatically.

#### Resolution

1. Check that the device shows up in the VeSync app, and that you can control it from there.
2. Check that the device is in the [list of supported devices](#supported-devices).
3. Run the [**VeSync: Update devices**](/actions/vesync.update_devices/) action from {% my developer_services title="**Settings** > **Tools** > **Actions**" %}, or reload the integration from {% my integrations title="**Settings** > **Devices & services**" %}.

The new device shows up on the **VeSync** integration page.

{% enddetails %}

{% details "Entities of a device show as unavailable" %}

### Symptom: All entities of a device are unavailable

All entities of one or more VeSync devices show as unavailable.

#### Description

The VeSync cloud reports that the device is offline, or Home Assistant can't reach the VeSync cloud.

#### Resolution

1. Open the VeSync app and check if the device shows as online.
2. If the device is offline in the app, check that it's powered on and connected to your Wi-Fi network. Restarting the device often helps.
3. If all your VeSync devices are unavailable, check the internet connection of Home Assistant. The VeSync service might also be temporarily unavailable.

The entities become available again at the next update, within a minute after the device is back online.

{% enddetails %}

{% details "Weekly, monthly, or yearly energy use doesn't change" %}

### Symptom: Energy use sensors update slowly

The **Energy use weekly**, **Energy use monthly**, and **Energy use yearly** sensors keep the same value for a long time.

#### Description

This is expected. These sensors update every 6 hours. **Current power** and **Energy use today** update every 60 seconds.

{% enddetails %}

If these steps don't solve your problem, [download the diagnostics](/docs/configuration/troubleshooting/#downloading-diagnostics) of the integration and [enable debug logging](/docs/configuration/troubleshooting/#enabling-debug-logging). Add both when you [report an issue](https://github.com/home-assistant/core/issues).

## Removing the integration

This integration follows standard integration removal. No extra steps are required.

{% include integrations/remove_device_service.md %}

### Removing a device from Home Assistant

You can remove a device from Home Assistant after you remove it from your VeSync account.

1. Remove the device in the VeSync app.
2. Go to {% my integrations title="**Settings** > **Devices & services**" %} and select **VeSync**.
3. Select the device.
4. Select the three dots {% icon "mdi:dots-vertical" %} menu, and then select **Delete**.

Devices that are still in your VeSync account can't be deleted. They're added again when the integration reloads.
