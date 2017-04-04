# AWS_ANSIBLE_DOCKER_ELK_WORDPRESS

The playbooks found here attempt to deploy the following stack and related components on AWS.    

* Create a VPC in a given AWS region.      
* Create a Security Group for the newly created VPC with following ports allowing Ingress :      
  * 22 [ssh]    
  * 80 [http]    
  * 443 [https]    
  * 5044 [Elasticsearch TCP Input]    
  * 5000 [Logstash TCP Input]   
  * 9200 [Elasticsearch HTTP]    
  * 9300 [Elasticsearch TCP transport]          


* Provision 2 Ec2 instances within the VPC/Subnet.          
* The first EC2 instance (we'll call it "elk_box") hosts 4 Docker containers, namely :        
  * Nginx : for reverse proxy and securing Kibana dashboard.      
  * Elasticsearch      
  * Logstash      
  * Kibana      


* The second EC2 instance (we'll call it "wordpress_box") hosts 3 Docker containers, namely :      
    * Nginx : for reverse proxy to access the wordpress interface.
    * Mysql
    * Wordpress      


* On "wordpress_box" we install the following additional logging components :       
  * Filebeat : to tail files and relay them to Elasticsearch.     
  * Metricbeat : to relay metrics from the operating system and services to Elasticsearch.     
  * AWS Cloudwatch Agent : to relay chosen logs to AWS Cloudwatch logs, which can in future enable settings alerts and notifications.     


* On "elk_box" we install the following additional logging components:  
  * AWS Cloudwatch Agent : to relay chosen logs to AWS Cloudwatch logs, which can in future enable settings alerts and notifications.
