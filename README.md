# ELK-Installation

### Step 1:
Launch EC-2 Instance with some configurations (Instance type: M4.large, storage 20 gb, Security group : TCP ,Alltraffic,ssh,http & https)
### Step 2:
Install Java :
```
sudo apt-get install default-jre
```
### Step 3: 
Check version :
```
Java --version 
```
### Step 4: 
Install Elastic search Artifacts : 
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
### Step 5:
Install APT Transport HTTPS :  
```
sudo apt-get install apt-transport-https
```
### Step 6: 
Install Artifacts:
```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```
### Step 7:
Install Elastic Search :
```
sudo apt-get update && sudo apt-get install elasticsearch
```
### Step 8: 
Configure the file located at : ``` /etc/elasticsearch/elasticsearch.yml ```,(node name, Network host , http port cluster.initial_master_nodes: ["node-1"] these should be enabled
### Step 9:
Configure JVM options located at : ``` /etc/elasticsearch/jvm.options ```(-Xmx4g & -Xms4g shloud be enabled)
### Step 10: 
Start elastic search : 
```
sudo systemctl start elastic search ,
sudo systemctl enable elastic search,
sudo systemctl status elastic search
```
### Step 11:
```
curl http://localhost:9200 
```
(Replace localhost with instance private IP)
### Step 12:
Install LOGSTASH : 
```
sudo apt-get install logstash
```
### Step 13:
Create file in the location of ``` /home ``` and create file with the name of ``` test_log.log ``` and paste 
```
INFO somelog
ERR somelog2
ERR somelog3
```
these command in this file then CD 
### Step 14:
Create a file with : ```sudo nano /etc/logstash/conf.d/apache-01.conf```
paste the following code in that file
```
input {
        file {
                path => "/home/test_log.log"
                start_position => "beginning"
        }
}
output{
        elasticsearch {
                hosts => ["localhost:9200"]
        }
}
```
Replace localhost with instance private IP
### Step 15: 
Start the service logstash with the command ``` sudo systemctl start logstash ``` then enable ```  sudo systemctl enable logstash ``` then check status ```  sudo systemctl status logstash  ```
### Step 16:
```
sudo curl -XGET 'localhost:9200/_cat/indices?v&pretty'
 ```
Replace localhost with private IP of instance
### Step 17:
Install kibana: ``` sudo apt-get install kibana ```
### Step 18:
Configure the file at ``` /etc/kibana/kibana.yml ```
enable - server port :5601 , serverhost & elasticsearch.hosts: ["http://localhost:9200"] replace localhost 
### Step 19: 
Start the kibana service : ``` sudo systemctl start kibana ``` then enable ``` sudo systemctl start kibana ``` and check status ``` sudo systemctl status kibana ```

### Step 20: 
paste ``` http://<IP>:5601 ``` in your browser ip should be your instance public ip then you will get kibana dashboard
