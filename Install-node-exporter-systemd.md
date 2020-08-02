# Install Monitoring tools

## Node-Exporter

### Create User

```sh
useradd -r --shell /bin/false node_exporter
```

### Install node-exporter

```sh
wget -O node_exporter.tar.gz https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
tar xvf node_exporter.tar.gz
cp node_exporter/node_exporter /usr/local/bin
```

### Grant ownership to user

```sh
chown node_exporter:node_exporter /usr/local/bin/node_exporter
rm -rf rm -rf prometheus*
```

### Create systemd unit file

```sh
vim /etc/systemd/system/node_exporter.service

## Sample config###
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter \

[Install]
WantedBy=multi-user.target
```

### Reload systemd

```sh
systemctl daemon-reload
systemctl start node_exporter
systemctl status node_exporter
```

### Start auto on system start

```sh
systemctl emable node_exporter
```
