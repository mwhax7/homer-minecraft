# Homer (Minecraft Fork)

A very simple fork of [Homer](https://github.com/bastienwirtz/homer) dashboard with a Minecraft component.

## Minecraft Component

The ***Minecraft component*** shows the status of a server in your Homer dashboard:

- ✅ <kbd>running</kbd> / ❌ <kbd>stopped</kbd> indicator (with green/red dot)
- 🖥️ Server software (e.g., <kbd>Paper</kbd>, <kbd>Spigot</kbd>)
- 📦 Server version (e.g., <kbd>1.21.8</kbd>)
- 👥 Players online / max
- Optional: display a small server icon/logo

**Configuration Exemple**

```yaml
- name: "Minecraft"
  host: "127.0.0.1"
  port: "25565"
  logo: "https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/minecraft.svg"
  type: Minecraft
```

**Result**

<img src="https://i.imgur.com/XyEmma5.png"><img src="https://i.imgur.com/RAqBP9u.png">

## Get started

Homer is a full static html/js dashboard, based on a simple yaml configuration file. See [documentation](docs/configuration.md) for information about the configuration (`assets/config.yml`) options.

It's meant to be served by an HTTP server, **it will not work if you open the index.html directly over file:// protocol**.

### Using docker

The configuration directory is bind mounted to make your dashboard easy to maintain.

**Start the container with `docker run`**

```sh
# Make sure your local config directory exists
docker run -d \
  --name homer \
  -p 8080:8080 \
  --mount type=bind,source="/path/to/config/dir",target=/www/assets \
  --restart=unless-stopped \
  mwhax7/homer-minecraft:latest
```

> [!NOTE]  
> The container will run using a user uid and gid 1000 by default, add `--user <your-UID>:<your-GID>` to the docker command to adjust it if necessary. Make sure this match the permissions of your assets directory.

**or `docker-compose`**

```yaml
services:
  homer:
    image: mwhax7/homer-minecraft:latest
    container_name: homer
    volumes:
      - /path/to/config/dir:/www/assets # Make sure your local config directory exists
    ports:
      - 8080:8080
    user: 1000:1000 # default
    environment:
      - INIT_ASSETS=1 # default, requires the config directory to be writable for the container user (see user option)
    restart: unless-stopped
```

**Environment variables:**

- **`INIT_ASSETS`** (default: `1`)
Install example configuration file & assets (favicons, ...) to help you get started.

- **`SUBFOLDER`** (default: `null`)
If you would like to host Homer in a subfolder, (ex: *<http://my-domain/homer>*), set this to the subfolder path (ex `/homer`).

- **`PORT`** (default: `8080`)
If you would like to change internal port of Homer from default `8080` to your port choice.

- **`IPV6_DISABLE`** (default: 0)
Set to `1` to disable listening on IPv6.

## Credits
[bastienwirtz](https://github.com/bastienwirtz) | [Homer](https://github.com/bastienwirtz/homer)
