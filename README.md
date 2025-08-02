# Juneogo Node Monitoring

Complete monitoring solution for Juneogo blockchain nodes using Prometheus, Grafana, and Loki.

## 🚀 Quick Start

```bash
git clone https://github.com/SRV-YouSoRandom/juneo-monitoring.git
cd juneo-monitoring
docker compose up -d
```

**Access Grafana**: http://your-server-ip:3001
- Username: `admin`
- Password: `admin123`

## 📊 What's Monitored

- **System Metrics**: CPU, RAM, Storage, Network I/O
- **Juneogo Database**: Disk usage for `~/.juneogo/db`
- **Service Logs**: Real-time Juneogo service logs via systemd
- **Historical Data**: 200-hour retention with 5-second refresh

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f grafana

# Update images
docker compose pull && docker compose up -d
```

## 🔧 Ports Used

- **Grafana**: 3001
- **Prometheus**: 9091
- **Loki**: 3100
- **Node Exporter**: 9100

## 🔐 Security Notes

For production use:
1. Change default Grafana password in `compose.yaml`
2. Restrict port access with firewall rules
3. Consider using a reverse proxy with SSL

## 📈 Dashboard Features

- Real-time system performance graphs
- Interactive log viewer with search/filtering
- Pre-configured alerts for resource thresholds
- Responsive design for mobile monitoring

Ready to monitor your Juneogo node! 🎉
