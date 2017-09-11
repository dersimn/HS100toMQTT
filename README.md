## Using Docker

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
