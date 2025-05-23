# Home Assistant Add-On: dump1090 based feeder for FlightRadar24, FlightAware, ADSBexchange and Plane Finder

<a href='https://ko-fi.com/MaxWinterstein' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://storage.ko-fi.com/cdn/kofi6.png?v=6' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

Add-on to feed ADS-B data from a cheap USB ADS-B Stick (e.g. [Nooelec NESDR Mini](https://www.amazon.com/gp/product/B009U7WZCA)) to FlightRadar24, FlightAware, ADSBexchange and Plane Finder.

![screenshot](https://raw.githubusercontent.com/MaxWinterstein/homeassistant-addons/main/adsb-multi-portal-feeder/images/screenshot.png)

## Prolog

This Add-On is based on the incredible [docker-fr24feed-piaware-dump1090](https://github.com/Thom-x/docker-fr24feed-piaware-dump1090) docker image by [Thom-x](https://github.com/Thom-x).

I just added a few sprinkles to make it work with Home Assistant.

## Installation

- If not already done, add the Add-on repostitory ([see](https://github.com/MaxWinterstein/homeassistant-addons#installation))
- If you want to share to FlightRadar24 and/or FlightAware and/or ADSBexchange and/or Plane Finder, generate needed keys ([see below](https://github.com/MaxWinterstein/homeassistant-addons/tree/main/adsb-multi-portal-feeder#flightaware-feeder-id--flightrader24-key--adsbexchange-uuid))
- If you want to use the dump1090 WebInterface (like the screenshot above) you need to set Lat/Lon for your location ([see below](https://github.com/MaxWinterstein/homeassistant-addons/tree/main/adsb-multi-portal-feeder#latitude--longitude))

## Sensors for Home Assistant

### Automatically added sensors

With version `1.27.0` I integrated the lovely project [adsb-hassio-sensors](https://github.com/plo53/adsb-hassio-sensors/tree/master) from [plo53](https://github.com/plo53).

This exposes sensors related to the feeder, e.g. `sensor.adsbfi_icao`, `sensor.adsbfi_mlat`, `sensor.adsbfi_mode_s`, `sensor.adsbfi_status` for the Adsb.fi feeder.

![Assistant ADS-B sensors](https://raw.githubusercontent.com/MaxWinterstein/homeassistant-addons/main/adsb-multi-portal-feeder/images/Home%20Assistant%20ADS-B%20sensors.jpg)  
![Assistant adsb.fi stats.jpg](https://raw.githubusercontent.com/MaxWinterstein/homeassistant-addons/main/adsb-multi-portal-feeder/images/Home%20Assistant%20adsb.fi%20stats.jpg)

The current discussion about that freshly added thing can be found within [#172](https://github.com/MaxWinterstein/homeassistant-addons/issues/172)

### Rest Sensors

If you would like some nice statistics you can use a rest sensor with some template magic to show e.g. the number of aircrafts currently tracked:

![sensor aircraft tracked](https://raw.githubusercontent.com/MaxWinterstein/homeassistant-addons/main/adsb-multi-portal-feeder/images/sensor_aircraft_tracked.png)

<figure>Example of 'Aircrafts tracked' sensor</figure>

See this [Home Assistant Community post](https://community.home-assistant.io/t/flightradar24-as-an-add-on/75081).  
Just replace the `<raspberry pi>` ip with

```yaml
resource: http://f1c878cb-adsb-multi-portal-feeder:8754/monitor.json
```

## Configuration

The whole configuration is meant to work alike the original docker image.  
See [docker-fr24feed-piaware-dump1090](https://github.com/Thom-x/docker-fr24feed-piaware-dump1090) for more info.

I pre-provivde the (i guess) most used workflow: Send data from USB ADS-B stick to FlightRadar24, FlightAware, ADSBexchange and Plane Finder and have a nice little web based overview as Home Assistant menu entry.

It it possible to retrive and use the location information of home assistant itself, using some magic variables, which will be replaced automatically:

- `HOMEASSISTANT_LATITUDE`
- `HOMEASSISTANT_LONGITUDE`
- `HOMEASSISTANT_ELEVATION`

If you want to add anything else just add the corresponding environment variable as config option.  
You might need to press the three little dots in the upper right corner and choose `Edit in YAML`.

E.g.: You want to change the default tracker on the HTML site from FlightAware to Flightradar24?

```json
...
HTML_DEFAULT_TRACKER: 'Flightradar24'
...
```

### FlightAware Feeder ID / FlightRader24 KEY / ADSBexchange UUID / Plane Finder Sharecode

There are multiple options to obtain the needed keys to supply data towards the platforms.  
Running the original image as interactive docker container is one of them.  
Either on your local machine, or e.g. the Home Assistant addon [SSH & Web Terminal
](https://github.com/hassio-addons/addon-ssh).

See [docker-fr24feed-piaware-dump1090](https://github.com/Thom-x/docker-fr24feed-piaware-dump1090) for more info.

### Latitude / Longitude

You can

- Use Google Maps for this: Go to https://www.google.com/maps/ and just right click at your house. It should display the lat/long
- Find it through some website: https://latitudelongitude.org/

## Accessing the UI

- This Add-On provides ingress functionality to some nice map of received data.  
  Simply enable the _Show in sidebar_ function or access vie the _OPEN WEB UI_ button.
- fr24feed (the feeder of FlightRader24) provides some stats at its internal port 8754.  
  To access add some external port at the `configuration` tab at `Network` like this:
  ![network](https://raw.githubusercontent.com/MaxWinterstein/homeassistant-addons/main/adsb-multi-portal-feeder/images/port-8754.png)  
  ![fr24stats](https://raw.githubusercontent.com/MaxWinterstein/homeassistant-addons/main/adsb-multi-portal-feeder/images/flightradar24-stats.png)

## Credits:

- Many thanks and ❤️ goes towards [Thom-x](https://github.com/Thom-x) for his work at [docker-fr24feed-piaware-dump1090](https://github.com/Thom-x/docker-fr24feed-piaware-dump1090)
- Icon from https://favpng.com/png_view/airplane-airplane-flightradar24-android-png/HFYfZ5Dy

## TODO

- Checkout https://github.com/wiedehopf/tar1090
- Use prebuild docker images. Tried whole evening but multi-arch is a pain in that case.
