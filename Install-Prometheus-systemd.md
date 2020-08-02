# Install Monitoring tools

## Prometheus

### Create User

```sh
useradd -r --shell /bin/false prometheus
```

### Create required directories

```sh
mkdir /etc/prometheus
mkdir /var/lib/prometheus
```

### Apply permissions to directories

```sh
chown prometheus:prometheus /etc/prometheus
chown prometheus:prometheus /var/lib/prometheus
```

### Install prometheus

```sh
wget -O prometheus.tar.gz https://github.com/prometheus/prometheus/releases/download/v2.20.0/prometheus-2.20.0.linux-amd64.tar.gz
tar xvf prometheus.tar.gz
cp prometheus/prometheus /usr/local/bin
cp prometheus/promtool /usr/local/bin
```

### Grant ownership to user

```sh
chown prometheus:prometheus /usr/local/bin/prometheus
chown prometheus:prometheus /usr/local/bin/promtool
```

### Move and grant access to library files

```sh
cp -r prometheus/consoles /etc/prometheus/
cp -r prometheus/console_libraries /etc/prometheus/
chown -R prometheus:prometheus /etc/prometheus/consoles
chown -R prometheus:prometheus /etc/prometheus/console_libraries
rm -rf prometheus*
```

### Configure prometheus

```sh
vim /etc/prometheus/prometheus.yml
chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

### Create systemd unit file

```sh
vim /etc/systemd/system/prometheus.service

## Sample config###
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries
    --web.enable-lifecycle

[Install]
WantedBy=multi-user.target
```

### Reload systemd

```sh
systemctl daemon-reload
systemctl start prometheus
systemctl status prometheus
```

### Start auto on system start

```sh
systemctl emable prometheus
```
