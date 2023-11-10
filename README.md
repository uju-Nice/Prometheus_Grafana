# Prometheus_Grafana

 Promtheus and Grafana
Use Prometheus for Monitoring and Grafana for data visualisation of the cluster

Prometheus Donwloads on prometheus server:

wget  https://github.com/prometheus/prometheus/releases/download/v2.36.1/prometheus-2.36.1.linux-amd64.tar.gz

ls -al

tar xvfz prometheus-2.36.1.linux-amd64.tar.gz

cd prometheus-2.36.1.linux-amd64
tar xvfz prometheus-*.tar.gz
cd prometheus-*

Before starting Prometheus, let's configure it.

vi prometheus.yml                ..... paste the below command and configure it accrodingly  

global:
  scrape_interval:     15s           ......controls how often Prometheus will scrape targets.
  evaluation_interval: 15s           ......controls how often Prometheus will evaluate rules

# Alertmanager configuration
alerting:
alertmanagers:
- static_configs:
- targets: ["ip of alert-manager server:9094”]. // According to the IP address of the alert-manager hostname


rule_files:                       .... specifies the location of any rules we want the Prometheus server to load
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
  
  - job_name: node
    static_configs:
      - targets: ['localhost:9100']


Run this command only on the prometheus server ONLY:

./prometheus --config.file=prometheus.yml        ...... This should start the promtehues

http://ip address of the server:9090/metrics             ...... use this url to validate if prometeus is running
http://ip address of the server:9090/graph              ... For in built browser for graph
 ./prometheus --web.listen-address="specific ip address:9090    to reconfigure promethues server in the terminal 

./prometheus --help     ..... To check more prometheus commands 


NOTE: With the above command you will see these below lines on your server

usage: prometheus [<flags>]


The Prometheus monitoring server

Installation of alert manager on the node you want for the alert manager

wget https://github.com/prometheus/alertmanager/releases/download/v0.24.0/alertmanager-0.24.0.linux-amd64.tar.gz

tar xvfx alertmanager-0.24.0.linux-amd64.tar.gz

ls al

cd alertmanager-0.24.0.linux-amd64

./alertmanager

NOTE: alertmanager listens on port 9093


Installation of Node exporter on the node you want for the node exporter

wget https://github.com/prometheus/node_exporter/releases/download/v1.4.0/node_exporter-1.4.0.linux-amd64.tar.gz

tar xvfz node_exporter-1.4.0.linux-amd64.tar.gz

ls -al

cd node_exporter-1.4.0.linux-amd64

./node_exporter

NOTE: node-exporter listens on port 9100

 http://10.0.0.12:9100/metrics    .......For the node exporter server On the browser







 GRAFANA
Grafana is used to visualize metric scraped from prometheus server

sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana -y
sudo apt-get install grafana-enterprise -y

sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server

sudo systemctl enable grafana-server.service

./bin/grafana-server web

to install the enterprise edition
 
sudo apt-get install -y adduser libfontconfig1

wget https://dl.grafana.com/enterprise/release/grafana-enterprise_9.3.1_amd64.deb

sudo dpkg -i grafana-enterprise_9.3.1_amd64.deb

sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable grafana-server
sudo /bin/systemctl status grafana-server
sudo /bin/systemctl start grafana-server

By default, Grafana will be listening on http://localhost:3000

There are methods to forward metrics to grafana server

Method 1. Create a Prometheus data source in Grafana:

Click on the "cogwheel" on the left hand side of the grafana Ui and click on setting.
Click on "Data Sources".
Click on "Add data source".
Select "Prometheus" as the type.
Set the appropriate Prometheus server URL (for example, http://localhost:9090/)
Adjust other data source settings as desired (for example, choosing the right Access method).
Click "Save & Test" to save the new data source.

Method 2.

curl -O -L "https://github.com/grafana/agent/releases/latest/download/agent-linux-amd64.zip";
unzip "agent-linux-amd64.zip";
chmod a+x agent-linux-amd64;
