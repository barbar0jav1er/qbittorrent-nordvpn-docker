# qBittorrent + NordVPN Docker Container

A Docker container that combines qBittorrent-nox with NordVPN for secure torrenting.

## Features

- **Latest qBittorrent-nox**: Uses static binaries from [userdocs/qbittorrent-nox-static](https://github.com/userdocs/qbittorrent-nox-static)
- **NordVPN Integration**: Automatic VPN connection with kill switch
- **Web UI**: Access qBittorrent through your browser
- **Secure**: All traffic routed through VPN
- **Easy Setup**: Simple Docker Compose configuration

## Quick Start (Recommended)

Use the pre-built Docker image for the easiest setup:

1. **Create docker-compose.yml**:
   ```yaml
   version: '3.8'
   services:
     qbittorrent-nordvpn:
       image: ghcr.io/barbar0jav1er/qbittorrent-nordvpn-docker:latest
       container_name: qbittorrent-nordvpn
       cap_add:
         - NET_ADMIN
       devices:
         - /dev/net/tun
       environment:
         - NORDVPN_TOKEN=${NORDVPN_TOKEN}
         - NORDVPN_COUNTRY=${NORDVPN_COUNTRY:-US}
         - NORDVPN_GROUP=${NORDVPN_GROUP:-P2P}
         - NORDVPN_PROTOCOL=${NORDVPN_PROTOCOL:-udp}
         - QBITTORRENT_PORT=${QBITTORRENT_PORT:-8080}
         - QBITTORRENT_USERNAME=${QBITTORRENT_USERNAME:-admin}
         - QBITTORRENT_PASSWORD=${QBITTORRENT_PASSWORD}
         - TZ=${TZ:-UTC}
       ports:
         - "${QBITTORRENT_PORT:-8080}:8080"
         - "6881:6881"
         - "6881:6881/udp"
       volumes:
         - ./config:/config
         - ./downloads:/downloads
       restart: unless-stopped
   ```

2. **Create .env file** with your NordVPN settings:
   ```bash
   NORDVPN_TOKEN=your_nordvpn_token_here
   NORDVPN_COUNTRY=US
   NORDVPN_GROUP=P2P
   NORDVPN_PROTOCOL=udp
   QBITTORRENT_PORT=8080
   QBITTORRENT_USERNAME=admin
   TZ=UTC
   ```

3. **Get your NordVPN token**:
   - Visit: https://my.nordaccount.com/dashboard/nordvpn/
   - Generate a new access token
   - Replace `your_nordvpn_token_here` in your `.env` file

4. **Start the container**:
   ```bash
   docker-compose up -d
   ```

5. **Access Web UI**:
   - Open http://localhost:8080 (or your configured port)
   - Default username: `admin`
   - Password: Check container logs for generated password

### Available Image Tags

- `latest`: Latest stable release
- `main`: Latest development build
- `v1.x.x`: Specific version tags

### Pull the image manually:
```bash
docker pull ghcr.io/barbar0jav1er/qbittorrent-nordvpn-docker:latest
```

## Build from Source (Advanced)

If you prefer to build the image yourself:

1. **Clone this repository**:
   ```bash
   git clone https://github.com/barbar0jav1er/qbittorrent-nordvpn-docker.git
   cd qbittorrent-nordvpn-docker
   ```

2. **Create environment file**:
   ```bash
   cp .env.example .env
   ```

3. **Edit `.env` file** with your NordVPN token and settings

4. **Start the container**:
   ```bash
   docker-compose up -d
   ```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `NORDVPN_TOKEN` | Your NordVPN access token | Required |
| `NORDVPN_COUNTRY` | Country to connect to | US |
| `NORDVPN_GROUP` | Server group (P2P recommended) | P2P |
| `NORDVPN_PROTOCOL` | Protocol (udp/tcp) | udp |
| `QBITTORRENT_PORT` | Web UI port | 8080 |
| `QBITTORRENT_USERNAME` | Web UI username | admin |
| `QBITTORRENT_PASSWORD` | Web UI password | Generated |
| `TZ` | Timezone | UTC |

### NordVPN Setup

1. **Get Token**: Visit https://my.nordaccount.com/dashboard/nordvpn/
2. **Generate Token**: Create a new access token
3. **Add to .env**: Copy token to `NORDVPN_TOKEN` in your `.env` file

### Ports

- **8080**: qBittorrent Web UI (configurable)
- **6881**: BitTorrent incoming connections

### Volumes

- `./config`: qBittorrent configuration and database
- `./downloads`: Downloaded files

## Usage

### Starting the Container
```bash
docker-compose up -d
```

### Stopping the Container
```bash
docker-compose down
```

### Viewing Logs
```bash
docker-compose logs -f
```

### Updating
```bash
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

## Security Notes

- All traffic is routed through NordVPN
- Kill switch prevents leaks if VPN disconnects
- Container runs with necessary network capabilities
- Use strong passwords for Web UI access

## Troubleshooting

### VPN Connection Issues
```bash
# Check VPN status
docker-compose exec qbittorrent-nordvpn nordvpn status

# Check logs
docker-compose logs qbittorrent-nordvpn
```

### qBittorrent Access Issues
- Ensure port is not blocked by firewall
- Check if container is running: `docker-compose ps`
- Verify VPN is connected before qBittorrent starts

### Permission Issues
- Ensure `config` and `downloads` directories are writable
- Check user permissions if using custom UID/GID

## License

This project is open source. qBittorrent and NordVPN have their own respective licenses.

## Disclaimer

This software is for educational and personal use only. Users are responsible for complying with their local laws and NordVPN's terms of service.