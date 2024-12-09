# docker-traccar-setup

This guide isis for Docker Traccar setup instructions

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
