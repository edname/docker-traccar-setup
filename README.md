# How to install and setup traccar on docker and portainer

## docker-traccar-setup guide

This guide is for Docker Traccar setup instructions.

1. Create work directories:
```bash
mkdir -p /opt/traccar/conf/
```

2. Get default traccar.xml:
```bash
sudo docker run --rm --entrypoint cat traccar/traccar:latest /opt/traccar/conf/traccar.xml > ~/traccar.xml
```
```bash
sudo mv ~/traccar.xml /opt/traccar/conf/
``` 

3. Edit traccar.xml
```bash
    <entry key='database.driver'>org.postgresql.Driver</entry>
    <entry key='database.url'>jdbc:postgresql://traccar-postgres-1/traccar</entry>
    <entry key='database.user'>postgres</entry>
    <entry key='database.password'>password</entry>
```

4. Create /opt/traccar/docker-compose.yml
```bash
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
      - /traccar/logs:/opt/traccar/logs
      - /traccar/conf:/opt/traccar/conf
      - /traccar/data:/opt/traccar/data
    ports:
      - "8082:8082"
      - "5000-5150:5000-5150"
      - "5000-5150:5000-5150/udp"

volumes:
  postgres-traccar-datavolume:
```

6. Create container. Run this command where docker-compose.yml was created
```bash
sudo docker compose up -d
```

7. Acces Traccar web interface to create Admin user
```bash
127.0.0.1:8082
```

8. Lets test device tracking. First we have to create device with name and identifier as:
```bash
TracTest
```

9. Lets send some dummy data to this device. If you are connected on the DEV machine you can send it to Localhost. Otherwise you can enter the IP of the server.
```bash
curl -X POST 'http://localhost:5055/?id=TracTest&lat=54.698475&lon=25.263613&speed=5'
```


