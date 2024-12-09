# docker-traccar-setup

This guide isis for Docker Traccar setup instructions

1. Create work directories:
```bash
mkdir -p /opt/traccar/logs
```

2. Get default traccar.xml:
```bash
cd ~
```
```bash
docker run --rm --entrypoint cat traccar/traccar:latest /opt/traccar/conf/traccar.xml > traccar.xml
```
```bash
sudo mv traccar.xml /opt/traccar/
``` 



```bash

```
