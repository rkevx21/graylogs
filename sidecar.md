# Installation

## Requirements

* [Sidecar](https://github.com/Graylog2/collector-sidecar/releases) 
* [Filebeat](https://www.elastic.co/downloads/beats/filebeat)

## Setup

* *Sign In* as an Admin 
* Navigate to *System > Sidecars*
* Click *Create or reuse a token for the graylog-sidecar user.*
* Create Token

### Sidecar

```bash
IP="127.0.0.1"
TOKEN_SIDECAR="12cl5vtbmglh1qa5lmc846cds2jmt3jcnre8ssbobedca953bpno"
sudo wget https://github.com/Graylog2/collector-sidecar/releases/download/1.0.1/graylog-sidecar_1.0.1-1_amd64.deb
sudo dpkg -i graylog-sidecar_1.0.1-1_amd64.deb
sudo mv /etc/graylog/sidecar/sidecar.yml /etc/graylog/sidecar/sidecar_backup.yml
(
cat <<-EOF
server_url: "http://${IP}:9000/api/"
server_api_token: "${TOKEN_SIDECAR}"
EOF
) | sudo tee /etc/graylog/sidecar/sidecar.yml
sudo graylog-sidecar -service install
sudo systemctl start graylog-sidecar
```

**IP**

Input server IP address used.

**TOKEN_SIDECAR**

Input created token

### Filebeat

```bash
sudo curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.2.0-amd64.deb
sudo dpkg -i filebeat-7.2.0-amd64.deb
sudo systemctl start filebeat
```

* Navigate to *System > Inputs*
* Select **Beats** as *Input* 
* Launch Input
* Select *Node*
* Input *Title*
* Checked *Do not add Beats type as prefix*
* Navigate to *System > Sidecars*
* Navigate to *Configuration*
* Create Configuration
 - Input Name
 - Collector : filebeat on Linux
 - Change the exact **hosts** *IP* used
 - **paths** customize data for desired target logs to be collected

```
fields_under_root: true
fields.collector_node_id: ${sidecar.nodeName}
fields.gl2_source_collector: ${sidecar.nodeId}

filebeat.inputs:
- input_type: log
  paths:
    - /var/log/*.log
  type: log
output.logstash:
   hosts: ["127.0.0.1:5044"]
path:
  data: /var/lib/graylog-sidecar/collectors/filebeat/data
  logs: /var/lib/graylog-sidecar/collectors/filebeat/log
```
* Navigate to *Administration*
* Select node 
* Checked *filebeat*
* Navigate *Configure* (right side)
* Select and confirm created configuration
* Navigate to *Overview*
* Show messages