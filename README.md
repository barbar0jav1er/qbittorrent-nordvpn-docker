# qBittorrent + NordVPN Docker Container

A Docker container that combines qBittorrent-nox with NordVPN for secure torrenting.

## Features

- **Latest qBittorrent-nox**: Uses static binaries from [userdocs/qbittorrent-nox-static](https://github.com/userdocs/qbittorrent-nox-static)
- **NordVPN Integration**: Automatic VPN connection with kill switch
- **Web UI**: Access qBittorrent through your browser
- **Secure**: All traffic routed through VPN
- **Easy Setup**: Simple Docker Compose configuration

## Quick Start

1. **Clone this repository**:
   ```bash
   git clone <repository-url>
   cd qbittorrent-nordvpn
   ```

2. **Create environment file**:
   ```bash
   cp .env.example .env
   ```

3. **Edit `.env` file** with your NordVPN token:
   - Get your NordVPN token from: https://my.nordaccount.com/dashboard/nordvpn/
   - Replace `your_nordvpn_token_here` with your actual token
   - Set your preferred country and other settings

4. **Start the container**:
   ```bash
   docker-compose up -d
   ```

5. **Access Web UI**:
   - Open http://localhost:8080 (or your configured port)
   - Default username: `admin`
   - Password: Check container logs for generated password

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