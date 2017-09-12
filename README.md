## Usage

	npm install -g hs100tomqtt
	hs100tomqtt --help

## Using with Docker

In order to use automatic device discovery, you have to run docker with `--net=host` or equivalent configuration.

### docker run

	docker run -d dersimn/HS100toMQTT -m mqtt://MQTT_IP

Run `docker run --rm dersimn/HS100toMQTT --help` to list all options.

### docker-compose service

	HS100toMQTT:
		image: dersimn/HS100toMQTT
		environment:
			- HS100TOMQTT_MQTT_URL="mqtt://MQTT_IP"

### Debug

#### Show debugging output

For some reason `Ctrl-C` is not working, workaround with named container:

	docker run --rm -it --name=HS100toMQTT dersimn/HS100toMQTT --mqtt-retain=false -m mqtt://MQTT_IP -v debug
	Ctrl-P Ctrl-Q
	docker stop HS100toMQTT

#### Manually build
	
	docker build -t username/HS100toMQTT .

## Credits

This project follows [Oliver "owagner" Wagner](https://github.com/owagner)'s architectural proposal for an [mqtt-smarthome](https://github.com/mqtt-smarthome/mqtt-smarthome).  
Built by copy-pasting together [Sebastian "hobbyquaker" Raff](https://github.com/hobbyquaker)'s mqtt-smarthome scripts and [Patrick "plasticrake" Seal](https://github.com/plasticrake)'s [hs100-api](https://github.com/plasticrake/hs100-api).