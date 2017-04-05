# AWS_ANSIBLE_DOCKER_ELK_WORDPRESS        
Ansible playbooks to provision and configure :      
* Wordpress       
* Mysql    
* Elasticsearch   
* Logstash    
* Kibana    
* Docker
* Docker-compose


## Ansible Support      
These playbooks have been tested on the following Ansible versions.         

* [Ansible 2.2.2.0](http://releases.ansible.com/ansible/ansible-2.2.2.0.tar.gz)      
* [Ansible 2.2.1.0](http://releases.ansible.com/ansible/ansible-2.2.1.0.tar.gz)     


## Dependencies       

* [Ansible](https://www.ansible.com/)       
* [boto](http://boto.cloudhackers.com/en/latest/)      
* [boto3](https://boto3.readthedocs.io/en/latest/)         
* [module](https://github.com/metacloud/molecule)       
  * **Note**: Running module tests work only on Ansible 2.2.1.0 as mentioned [here](https://github.com/ansible/ansible/issues/23016)         


## Description
The playbooks found here attempt to deploy the following stack and related components on AWS.    

* Create a VPC in a given AWS region.      
* Create a Subnet within the VPC.
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
  * AWS Cloudwatch Agent : to relay chosen logs to AWS Cloudwatch logs, which can in future enable setting alerts and notifications.     


* On "elk_box" we install the following additional logging components:  
  * AWS Cloudwatch Agent : to relay chosen logs to AWS Cloudwatch logs, which can in future enable setting alerts and notifications.      


 ## Logging / Monitoring
 Logging is primary done on the Docker container hosts themselves rather than within the containers themselves. This is so that the logging infrastructure can be kept flexible and be in a state to enable logging backend changes without rebuilding the Docker images themselves. The details of logging on each of the hosts are as follows :      

* elk_box      
  * AWS Cloudwatch logs agent is installed on the host.      
  * Nginx access and error logs, that reside within the container are mapped to a volume on the host.      
  * The Nginx logs are then streamed to AWS Cloudwatch logs.      
  * The host syslog is stream to AWS Cloudwatch logs.  


 ## Roles            
 * [common](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/common)       
 * [vpc](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/vpc)       
 * [security_groups](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/security_groups)          
 * [elk_spawn](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/elk_spawn)       
 * [wordpress_spawn](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/wordpress_spawn)      
 * [elk](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/elk)       
 * [wordpress](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/wordpress)      
 * [logger](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/logger)         
 * [filebeat](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/filebeat)         
 * [metricbeat](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/metricbeat)         
 * [cloudwatch](https://github.com/gautammanohar/ansible-docker-aws-elk-wp/tree/master/roles/cloudwatch)        



 ## Testing

 ## Production Improvements
