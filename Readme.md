# ZiGate component for Home Assistant


# :warning: Since Home Assistant 0.118 this integration is deprecated !
I recommand using ZHA integration instead with ZiGate firmware 3.1d


A new component to use the ZiGate (<http://zigate.fr>)

![Tests](https://github.com/doudz/homeassistant-zigate/workflows/Tests/badge.svg)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/sebramage)
[![hacs_badge](https://img.shields.io/badge/HACS-Default-green.svg)](https://github.com/custom-components/hacs)
[![Donate with Bitcoin](https://en.cryptobadges.io/badge/small/3DHvPBWyf5Vsp485tGFu7WfYSd6r5qgZdH)](https://en.cryptobadges.io/donate/3DHvPBWyf5Vsp485tGFu7WfYSd6r5qgZdH)

Currently it supports sensor, binary_sensor and switch, light, cover and climate

To install:

- if not exists, create folder 'custom\_components' under your home assitant directory (beside configuration.yaml)
- copy all the files in your hass folder, under 'custom\_components' like that :

```text
custom_components/
└── zigate
    ├── __init__.py
    ├── services.yaml
    ├── switch.py
    ├── sensor.py
    ├── light.py
    ├── binary_sensor.py
    ├── cover.py
    └── climate.py
```
- restart your Home Assistant, so that it installs requirements
- adapt your configuration.yaml

To pair a new device, go in developer/services and call the 'zigate.permit\_join' service.
You have 30 seconds to pair your device.

Configuration example :

```yaml
# Enable ZiGate (port will be auto-discovered)
zigate:

```

or

```yaml
# Enable ZiGate
zigate:
  port: /dev/ttyUSB0
  channel: 24
  enable_led : false

```

or
if you want to use Wifi ZiGate (or usb zigate forwarded with ser2net for example)
Port is optional, default is 9999

```yaml
# Enable ZiGate Wifi
zigate:
  host: 192.168.0.10:9999

```

If you want to use PiZiGate, just add `gpio: true`. Other options are still available (channel, etc)

```yaml
# Enable PiZiGate
zigate:
  gpio: true
  port: /dev/ttyAMA0
  # Regarding the raspberry
  #port: /dev/serial0

```

If you're using Rpi3, you can have some trouble trying to use PiZiGate.
If needed, add the following line into config.txt (If you're using Hass.io you have to access that on the SD card directly. Simply plug it into your PC and edit it there. The config.txt is not accessible from your Hass.io system, you may need to open the SD card on a Windows or Linux system.):

```text
dtoverlay=pi3-miniuart-bt
enable_uart=1
```

## Polling

By default, the component will poll the state of any device available on idle (light, relay, etc) every 120sec

To disable polling, add `polling: false` in config

You can adjust the polling by setting `scan_interval` in config (default to 120)

## Upgrade firmware

You could upgrade the zigate firmware to the latest available release by calling `zigate.upgrade_firmware`.

- If you're using PiZigate or ZiGate DIN the process is fully automatic
- If you're using USB TTL ZiGate you have to put zigate in download mode first
- Always call zigate.stop_zigate before unplugging the USB ZiGate

## Admin Panel

ZiGate lib has now an embedded admin panel, to enable it, add `admin_panel: true` in config.
To access the admin you need to open another browser to addresse http://[ip_address]:9998
Integration of the admin panel in HA has been disabled because of security issue

```yaml
zigate:
  admin_panel: true
```

## Package

Additionnally you could add the zigate package to have a new tab with all zigate devices and a "permit join" switch.

To install, just copy the "packages" folder in your hass config folder and if needed add the following in your configuration.yaml

```yaml
homeassistant:
  packages: !include_dir_named packages
```

## How enable debug log

```yaml
logger:
  default: error
  logs:
    zigate: debug
    custom_components.zigate: debug

```

Alternatively you could call the service `logger.set_level` with data `{"custom_components.zigate": "debug", "zigate": "debug"}`

## How to adjust device parameter

Some devices have the ability to change some parameters, for example on the Xiaomi vibration sensor you can adujst the sensibility. You'll be able to do that using the service `write_attribute` with parameters :
`{ "addr": "8c37", "endpoint":"1", "cluster":"0", "attribute_id":"0xFF0D", "manufacturer_code":"0x115F", "attribute_type":"0x20", "value":"0x01" }`

In this example, the value is the sensiblity, it could be 0x01 for "high sens", 0x0B for "medium" and 0x15 for "low"

## Battery level

I recommand the following package to create a nice battery tab with alerts !
[battery_alert.yaml](https://github.com/notoriousbdg/Home-AssistantConfig/blob/master/packages/battery_alert.yaml)

## How to contribute

If you are looking to make a contribution to this project we suggest that you follow the steps in these guides:

- <https://github.com/firstcontributions/first-contributions/blob/master/README.md>
- <https://github.com/firstcontributions/first-contributions/blob/master/github-desktop-tutorial.md>

Some developers might also be interested in receiving donations in the form of hardware such as Zigbee modules or devices, and even if such donations are most often donated with no strings attached it could in many cases help the developers motivation and indirect improve the development of this project.

## Comment contribuer

Si vous souhaitez apporter une contribution à ce projet, nous vous suggérons de suivre les étapes décrites dans ces guides:

- <https://github.com/firstcontributions/first-contributions/blob/master/README.md>
- <https://github.com/firstcontributions/first-contributions/blob/master/github-desktop-tutorial.md>

Certains développeurs pourraient également être intéressés par des dons sous forme de matériel, tels que des modules ou des dispositifs Zigbee, et même si ces dons sont le plus souvent donnés sans aucune condition, cela pourrait dans de nombreux cas motiver les développeurs et indirectement améliorer le développement de ce projet.
