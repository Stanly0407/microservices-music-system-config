# microservices-music-system-config

Centralized configuration repository for the
[microservices_music_system](https://github.com/Stanly0407/microservices_music_system) project.

This repo is served by the project's Spring Cloud **Config Server**
(`config-server` module, port `8888`). Each client service imports its
configuration on startup via:

```yaml
spring:
  config:
    import: "optional:configserver:http://config-server:8888"
```

## Layout

Spring Cloud Config Server resolves files by `spring.application.name`:

- `application.yml` — shared defaults applied to **every** client (e.g. actuator exposure).
- `<service-name>.yml` — overrides/additions specific to one service (e.g. `song-service.yml`,
  `api-gateway.yml`).

## Branch / label

The Config Server is configured with `default-label: main`, so configuration changes must be
pushed to the `main` branch to be picked up.

## Testing dynamic refresh

1. Edit a value in one of the `*.yml` files here (e.g. `song-service.yml`'s
   `song.service.welcome-message`) and push to `main`.
2. Call `POST /actuator/busrefresh` on the running client service (e.g. `song-service`).
3. Verify the new value is in effect (e.g. `GET /config-info` on song-service) — no restart
   required.
