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
```bash
    <entry key='database.driver'>org.postgresql.Driver</entry>
    <entry key='database.url'>jdbc:postgresql://traccar-postgres-1/traccar</entry>
    <entry key='database.user'>postgres</entry>
    <entry key='database.password'>password</entry>
```

4. Create /opt/traccar/logs/docker-compose.yml
```bash
version: '3.8'

services:
  postgres:
    image: postgres
    environment:
      POSTGRES_DB: traccar
      POSTGRES_PASSWORD: password
    expose:
      - 5432
    volumes:
      - postgres-traccar-datavolume:/var/lib/postgresql/data

  traccar:
    image: traccar/traccar:latest
    depends_on:
      - postgres
    volumes:
      - ./traccar/logs:/opt/traccar/logs
      - ./traccar/conf:/opt/traccar/conf
      - ./traccar/data:/opt/traccar/data
    ports:
      - "8082:8082"
      - "5000-5150:5000-5150"
      - "5000-5150:5000-5150/udp"

volumes:
  postgres-traccar-datavolume:
```

6. Create container:
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
