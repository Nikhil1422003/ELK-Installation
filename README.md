# ELK-Installation

### Step 1:
Launch EC-2 Instance with some configurations (Instance type: M4.large, storage 20 gb, Security group : TCP ,Alltraffic,ssh,http & https)

![no0](https://github.com/user-attachments/assets/ff5094e5-1f37-476d-8d84-1d0321a0d9ca)

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

![no1](https://github.com/user-attachments/assets/2b8f9d1e-ed39-417c-b62e-2f3040ddcb0a)
![no2](https://github.com/user-attachments/assets/1bfe8958-f32e-46dc-a1e2-23fddbbd99ef)

### Step 9:
Configure JVM options located at : ``` /etc/elasticsearch/jvm.options ```(-Xmx4g & -Xms4g shloud be enabled)

![no3](https://github.com/user-attachments/assets/260b0847-cf4f-4c9b-8937-9db8c2ca3acf)

### Step 10: 
Start elastic search : 
```
sudo systemctl start elasticsearch ,
sudo systemctl enable elasticsearch,
sudo systemctl status elasticsearch
```
![no4](https://github.com/user-attachments/assets/be479f3a-532e-4e3e-a446-b8ae283deef8)

### Step 11:
```
curl http://localhost:9200 
```
(Replace localhost with instance private IP) you will get  output as shown below

![no5](https://github.com/user-attachments/assets/2ce80e73-c324-491e-8951-dce66dad8432)

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

![no6](https://github.com/user-attachments/assets/aad60ac9-b5c5-4a53-90ca-eed8d2a53517)

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
![no7](https://github.com/user-attachments/assets/ceb15d4f-89d2-432c-bb61-890d301246b7)

### Step 15: 
Start the service logstash with the command ``` sudo systemctl start logstash ``` then enable ```  sudo systemctl enable logstash ``` then check status ```  sudo systemctl status logstash  ```
### Step 16:
```
sudo curl -XGET 'localhost:9200/_cat/indices?v&pretty'
 ```
Replace localhost with private IP of instance , You will get output as shown below

![no8](https://github.com/user-attachments/assets/b88cf42f-49e4-49e4-bb43-4a3dc732c8f0)

### Step 17:
Install kibana: ``` sudo apt-get install kibana ```
### Step 18:
Configure the file at ``` /etc/kibana/kibana.yml ```
enable - server port :5601 , serverhost & elasticsearch.hosts: ["http://localhost:9200"] replace localhost 

![no9](https://github.com/user-attachments/assets/6b3475d6-162a-4a0d-9882-ee2929b2e67a)
### Step 19: 
Start the kibana service : ``` sudo systemctl start kibana ``` then enable ``` sudo systemctl start kibana ``` and check status ``` sudo systemctl status kibana ```

![no10](https://github.com/user-attachments/assets/63c5d20e-6e73-4805-979c-789af57bef8e)

### Step 20: 
paste ``` http://<IP>:5601 ``` in your browser ip should be your instance public ip then you will get kibana dashboard

![no11](https://github.com/user-attachments/assets/211f31d4-bb97-42ee-89e5-fd2c89acaa03)
![no12](https://github.com/user-attachments/assets/f2f46dc9-eaef-4cc3-8b21-c903c647d820)
![no13](https://github.com/user-attachments/assets/d4881f70-35b2-41a2-8c89-845bbcb94a9f)

