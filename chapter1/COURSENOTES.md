Installing ActiveMQ in Ubuntu 20.04
===================================

The `apt` package is broken for the 20.04 distribution. It is more advantageous to install from the ActiveMQ webpage.  
The `apt` package for 20.04 installs ActiveMQ version 5.15.11 so I downloaded [binaries for this version](https://activemq.apache.org/activemq-5015011-release).

I follow installation instructions from ["How to Install Apache ActiveMQ on Ubuntu 20.04"](https://websiteforstudents.com/how-to-install-apache-activemq-on-ubuntu-20-04-18-04/). In a nutshell:

```bash
sudo apt-get install default-jre
cd /tmp
wget http://archive.apache.org/dist/activemq/5.15.11/apache-activemq-5.15.8-bin.tar.gz
tar -xvzf apache-activemq-5.15.11-bin.tar.gz
sudo mv apache-activemq-5.15.11 /opt/activemq
sudo addgroup --quiet --system activemq
sudo adduser --quiet --system --ingroup activemq --no-create-home --disabled-password activemq
sudo chown -R activemq:activemq /opt/activemq
sudo ln -s /opt/activemq/bin/activemq /opt/bin/activemq
```

We want to create a systemd service, thus the following file is required:
```bash
sudo nano /etc/systemd/system/activemq.service
```

with contents:

```bash
[Unit]
Description=Apache ActiveMQ
After=network.target
[Service]
Type=forking
User=activemq
Group=activemq

ExecStart=/opt/activemq/bin/activemq start
ExecStop=/opt/activemq/bin/activemq stop

[Install]
WantedBy=multi-user.target
```

To start and enable the new `activemq` service:

```bash
sudo systemctl daemon-reload
sudo systemctl start activemq
sudo systemctl enable activemq
```

To verify if the service is functioning,

```bash
activemq status
```

I need to add myself to group `activemq` if I want to run the `console`, since a file will be written under `/opt/activemq`
