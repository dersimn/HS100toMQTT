NodeJS script to control TP-Link HS100 & HS110 devices via MQTT.

Topics:

	hs100/maintenance/_bridge    /online    -> bool
	hs100/maintenance/<DEVICE_ID>/online    -> bool

	hs100/status/<DEVICE_ID>    -> JSON: {"val":false,"power":0,"voltage":230.68353,"current":0.012407}
	hs100/set   /<DEVICE_ID>    <- bool

(Spaces here are only for formatting, the actual topics won't have them.)

## Usage

### Native

	git clone <this repo URL> HS100toMQTT
	cd HS100toMQTT
	npm install
	node index --help

### Docker

Show all available options:

	docker run --rm dersimn/hs100tomqtt --help

Start with:

	docker run -d --net=host dersimn/hs100tomqtt -m mqtt://<MQTT Broker IP>

In order to use automatic device discovery, your Docker host has to suport the `--net=host` parameter - not all Docker installations can do this (see *Docker for Mac* [issue](https://forums.docker.com/t/should-docker-run-net-host-work/14215)).  
If you prefer to run the script in bridge mode or your host doesn't support host networking, provide a list of IP addresses via the `--devices` option:

	docker run -d dersimn/hs100tomqtt -m mqtt://<MQTT Broker IP> --devices="10.1.1.100 10.1.1.101"

### Blocking internet access for your devices

Even though there are currently [no known security issues](https://www.softscheck.com/en/reverse-engineering-tp-link-hs110/) for the HS100 / HS110, if you choose to block internet access for your plugs, be aware that the unterlying [tplink-smarthome-api](https://github.com/plasticrake/tplink-smarthome-api) will throw an error on every polling cycle, because the TP-Link devices will have a wrong time set-up (quite obvious: no Internet, no NTP server, no correct set time and date).  

I've written this [workaround](https://github.com/dersimn/HS100toMQTT/blob/64a364f0336af1cb08791b13346441641fecee26/index.js#L87) until I found a better way to solve this problem: According to [this](https://blog.georgovassilis.com/2016/05/07/controlling-the-tp-link-hs100-wi-fi-smart-plug/) source, the plugs are using `fr.pool.ntp.org` to get their time. If you are able to alter the DNS resolving mechanism of your router (for e.g. when you're using OpenWRT), just make sure to redirect the DNS name to your router IP and setup a local NTP server.

In OpenWRT you can configure this with:

`/etc/config/firewall`:

	config rule
		option enabled '1'
		option src 'lan'
		option name 'Block HS110'
		option src_mac '00:00:00:00:00:00'
		option dest 'wan'
		option target 'REJECT'

`/etc/config/dhcp`:

	config domain
		option name 'fr.pool.ntp.org'
		option ip '10.1.1.1'

## Development

### Show debugging output

For some reason `Ctrl-C` is not working, workaround:

	docker run --rm -it --name=hs100tomqtt dersimn/hs100tomqtt --mqtt-retain=false -m mqtt://MQTT_IP -v debug
	Ctrl-P Ctrl-Q
	docker stop hs100tomqtt

### Manually build
	
	docker build -t hs100tomqtt .
	docker build -t hs100tomqtt:armhf -f Dockerfile.armhf .

## Credits

This project follows [Oliver "owagner" Wagner](https://github.com/owagner)'s architectural proposal for an [mqtt-smarthome](https://github.com/mqtt-smarthome/mqtt-smarthome).  
Built by copy-pasting together [Sebastian "hobbyquaker" Raff](https://github.com/hobbyquaker)'s mqtt-smarthome scripts and [Patrick "plasticrake" Seal](https://github.com/plasticrake)'s [tplink-smarthome-api](https://github.com/plasticrake/tplink-smarthome-api).