# Juneogo Node Monitoring Setup

Complete monitoring solution for your Juneogo blockchain node using Prometheus, Loki, and Grafana.

## 🏗️ Directory Structure

```
monitoring/
├── docker-compose.yml              # Main orchestration file
├── setup_monitoring.sh             # Automated setup script
├── prometheus/
│   ├── prometheus.yml              # Prometheus configuration
│   └── rules/                      # Alert rules (optional)
├── loki/
│   └── loki.yml                    # Loki configuration
├── promtail/
│   └── promtail.yml                # Log collection configuration
└── grafana/
    ├── provisioning/
    │   ├── datasources/
    │   │   └── datasources.yml     # Auto-configure data sources
    │   └── dashboards/
    │       └── dashboards.yml      # Dashboard provider config
    └── dashboards/
        └── juneogo-monitoring.json # Pre-built dashboard
```

## 🚀 Quick Setup

### Option 1: Automated Setup (Recommended)

1. **Download and run the setup script:**
   ```bash
   curl -o setup_monitoring.sh [script_url]
   chmod +x setup_monitoring.sh
   sudo ./setup_monitoring.sh
   ```

### Option 2: Manual Setup

1. **Create the monitoring directory:**
   ```bash
   mkdir -p monitoring/{prometheus/{rules},loki,promtail,grafana/{provisioning/{datasources,dashboards},dashboards}}
   cd monitoring
   ```

2. **Create all configuration files** (copy from the artifacts above)

3. **Start the monitoring stack:**
   ```bash
   docker-compose up -d
   ```

## 📊 Access Points

After setup, access your monitoring tools:

- **Grafana Dashboard**: http://localhost:3001
  - Username: `admin`
  - Password: `admin123`
- **Prometheus**: http://localhost:9091
- **Loki**: http://localhost:3100

## 🔍 What's Being Monitored

### System Metrics (via Node Exporter)
- **CPU Usage**: Real-time CPU utilization percentage
- **RAM Usage**: Memory consumption and availability
- **Storage Usage**: Disk space used by Juneogo database (`$HOME/.juneogo/db`)
- **Disk I/O**: Read/write operations
- **Network**: Network traffic and connections

### Logs (via Promtail → Loki)
- **Juneogo Service Logs**: Systemd journal entries for `juneogo.service`
- **System Logs**: General system log entries
- **Error Tracking**: Automated error detection and highlighting

### Dashboard Features
- **Real-time Updates**: 5-second refresh rate
- **Historical Data**: 200-hour retention period
- **Interactive Logs**: Searchable and filterable log viewer
- **Alert Thresholds**: Visual indicators for resource usage

## 🛠️ Management Commands

### Start/Stop Services
```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# Restart specific service
docker-compose restart grafana

# View service logs
docker-compose logs -f prometheus
```

### Maintenance
```bash
# Update all images
docker-compose pull && docker-compose up -d

# Check service status
docker-compose ps

# Remove all data (careful!)
docker-compose down -v
```

## 🔧 Customization

### Adding Custom Metrics

1. **Edit Prometheus configuration:**
   ```bash
   nano prometheus/prometheus.yml
   ```

2. **Add new scrape targets:**
   ```yaml
   scrape_configs:
     - job_name: 'custom-exporter'
       static_configs:
         - targets: ['localhost:8080']
   ```

3. **Restart Prometheus:**
   ```bash
   docker-compose restart prometheus
   ```

### Custom Dashboards

1. **Create new dashboard in Grafana UI**
2. **Export JSON and save to:**
   ```
   grafana/dashboards/your-dashboard.json
   ```

### Log Sources

Edit `promtail/promtail.yml` to add new log sources:

```yaml
scrape_configs:
  - job_name: custom-logs
    static_configs:
      - targets:
          - localhost
        labels:
          job: custom-logs
          __path__: /path/to/your/logs/*.log
```

## 🚨 Alerting (Optional)

### Setting Up Alerts

1. **Create alert rules in:**
   ```
   prometheus/rules/juneogo-alerts.yml
   ```

2. **Example alert rule:**
   ```yaml
   groups:
     - name: juneogo-alerts
       rules:
         - alert: HighCPUUsage
           expr: 100 - (avg(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
           for: 5m
           labels:
             severity: warning
           annotations:
             summary: "High CPU usage detected"
   ```

## 🔐 Security Considerations

### Production Deployment

1. **Change default passwords:**
   ```bash
   # Edit docker-compose.yml
   environment:
     - GF_SECURITY_ADMIN_PASSWORD=your_secure_password
   ```

2. **Enable HTTPS:**
   - Add reverse proxy (nginx/traefik)
   - Configure SSL certificates

3. **Restrict access:**
   - Use firewall rules
   - Set up VPN access
   - Configure authentication

## 📈 Performance Tuning

### For High-Volume Nodes

1. **Increase retention:**
   ```yaml
   # In prometheus.yml
   '--storage.tsdb.retention.time=720h'  # 30 days
   ```

2. **Adjust scrape intervals:**
   ```yaml
   scrape_interval: 30s  # Reduce frequency
   ```

3. **Optimize Loki:**
   ```yaml
   # In loki.yml
   ingestion_rate_mb: 32
   ingestion_burst_size_mb: 64
   ```

## 🆘 Troubleshooting

### Common Issues

1. **Port conflicts:**
   - Modify ports in `docker-compose.yml`
   - Current setup uses: 3001 (Grafana), 9091 (Prometheus)

2. **Permission issues:**
   - Ensure Docker has access to log directories
   - Check systemd journal permissions

3. **Data not showing:**
   - Verify node-exporter is running: `curl localhost:9100/metrics`
   - Check Prometheus targets: http://localhost:9091/targets

4. **Logs not appearing:**
   - Verify systemd journal access
   - Check promtail logs: `docker-compose logs promtail`

### Debug Commands

```bash
# Check if metrics are being collected
curl localhost:9100/metrics | grep node_cpu

# Test Loki connectivity
curl localhost:3100/ready

# Verify Prometheus scraping
curl localhost:9091/api/v1/targets
```

## 📊 Monitoring Best Practices

1. **Set up alerts** for critical thresholds
2. **Regular backups** of Grafana dashboards
3. **Monitor disk space** for log storage
4. **Review metrics** regularly for optimization opportunities
5. **Document changes** to configurations

## 🤝 Support

If you encounter issues:

1. Check service logs: `docker-compose logs [service]`
2. Verify configurations match the provided templates
3. Ensure all required ports are available
4. Check Docker and system resources

Your Juneogo node monitoring is now ready! 🎉