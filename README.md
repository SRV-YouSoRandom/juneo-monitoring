# Juneogo Node Monitoring

Complete monitoring solution for Juneogo blockchain nodes using Prometheus, Grafana, and Loki.

<img width="2404" height="3040" alt="212 90 121 86_3001_d_juneogo-monitoring-working_juneogo-node-monitoring-working_orgId=1 from=now-1h to=now timezone=browser refresh=10s" src="https://github.com/user-attachments/assets/e8d1cda5-64af-4eca-9015-e7f4bb668862" />


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
