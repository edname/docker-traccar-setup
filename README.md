# docker-traccar-setup

This guide isis for Docker Traccar setup instructions

1. Create work directories:
```bash
mkdir -p /opt/traccar/logs
```

2. Get default traccar.xml:
```bash
sudo docker run --rm --entrypoint cat traccar/traccar:latest /opt/traccar/conf/traccar.xml > ~/traccar.xml
```
```bash
sudo mv ~/traccar.xml /opt/traccar/
``` 

3. Edit traccar.xml

4. Create container:
```bash
docker run \
--name traccar \
--hostname traccar \
--detach --restart unless-stopped \
--publish 80:8082 \
--publish 5000-5150:5000-5150 \
--publish 5000-5150:5000-5150/udp \
--volume /opt/traccar/logs:/opt/traccar/logs:rw \
--volume /opt/traccar/traccar.xml:/opt/traccar/conf/traccar.xml:ro \
traccar/traccar:latest
```
