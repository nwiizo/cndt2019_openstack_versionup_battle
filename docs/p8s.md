## 監視サーバーの構築
利用OS:`Ubuntu 18.04.1 LTS` 
### Docker のインストール
```
apt install -y docker.io docker-compose
```
### Grafana 及びPrometheus インストール
```
docker run -d -p 3000:3000 grafana/grafana
docker run -p 9090:9090 prom/prometheus
docker run -d -p 9100:9100 --net="host" prom/node-exporter
```
### Influxdbのインストールおよび検証
#### インストール
```
apt install -y influxdb influxdb-client  influxdb-dev
systemctl start influxdb.service
systemctl start influxdb.service
```
#### 設定
```
CREATE USER admin WITH PASSWORD 'samples.password' WITH ALL PRIVILEGES
GRANT ALL PRIVILEGES TO admin
REVOKE ALL PRIVILEGES FROM admin
```

#### サンプルスクリプト
``` bash
#!/bin/bash
flock -x /tmp/ab/wing ab -n 100 -c 5 http://phpcon-2018.conohawing.com/  > /tmp/ab/wing.txt


wing_time=`cat /tmp/ab/wing.txt | grep "Time per request" |grep all | awk '{print $4;}'`
wing_longest_request=`cat /tmp/ab/wing.txt | grep "longest request" | awk '{print $2}'`

echo ${wing_time}
curl_wing="curl -i -XPOST 'http://localhost:8086/write?db=statsdemo' --data-binary 'time_per_request,host=wing value=${wing_time}'"
curl_request_wing="curl -i -XPOST 'http://localhost:8086/write?db=statsdemo' --data-binary 'longest_request,host=wing value=${wing_longest_request}'"

eval ${curl_wing}
```

## [prometheus-openstack-exporter](https://github.com/CanonicalLtd/prometheus-openstack-exporter) の構築
監視サーバーで各ホストを監視する。
```
apt-get install python-neutronclient python-novaclient python-keystoneclient python-netaddr python-cinderclient
```
