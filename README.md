## npm

	npm install -g hs100tomqtt
	hs100tomqtt --help

## Docker

In order to use automatic device discovery, you have to run docker with `--net=host` or equivalent configuration.  
If your system doesn't work with `--net=host` (see *Docker for Mac* [issue](https://forums.docker.com/t/should-docker-run-net-host-work/14215)), use a pre-defined device table instead, call `--help` and look for option `--device-table`.

### docker run

	docker run -d --net=host dersimn/hs100tomqtt -m mqtt://MQTT_IP
	docker run -d --net=host dersimn/hs100tomqtt:armhf -m mqtt://MQTT_IP

	docker run -d dersimn/hs100tomqtt -m mqtt://MQTT_IP --device-table /node/exampleDeviceTable.json

Run `docker run --rm dersimn/hs100tomqtt --help` to list all options.

### docker-compose service

	hs100tomqtt:
	  image: dersimn/hs100tomqtt
	  environment:
	    - HS100TOMQTT_MQTT_URL="mqtt://MQTT_IP"
	  network_mode: "host"

or

	hs100tomqtt:
	  image: dersimn/hs100tomqtt
	  volumes:
        - ~/deviceTable.json:/node/deviceTable.json:ro
	  environment:
	    - HS100TOMQTT_MQTT_URL=mqtt://MQTT_IP
	    - HS100TOMQTT_DEVICE_TABLE=/node/deviceTable.json

## Development / Debug

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
Built by copy-pasting together [Sebastian "hobbyquaker" Raff](https://github.com/hobbyquaker)'s mqtt-smarthome scripts and [Patrick "plasticrake" Seal](https://github.com/plasticrake)'s [hs100-api](https://github.com/plasticrake/hs100-api).