# Installatin of logstatsh and formatting of nginx logs

1.  Installation of logstash:
    First the required gpg keys and repos of elastic are added and then log stash is installed:

```bash

 wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add –

 sudo apt-get install apt-transport-https

 echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

 sudo apt-get update && sudo apt-get install logstash


```

Checking status of log stash service:

2. Dowloading the nginx log file:

```bash

    wget https://github.com/elastic/examples/blob/master/Common%20Data%20Formats/nginx_logs/nginx_logs

```

3. Creation of conf file for parsing nginx logs:

```conf

input {
	file {
     		path => ["~/Documnents/nginx_logs"]
        	start_position => "beginning"
 	}
}
filter {
     	grok {
       	match => {
"message" => [ "%{IPORHOST:remote_ip} - %{DATA:user_name} \[%{HTTPDATE:access_time}\] \"%{WORD:http_method} %{DATA:url} HTTP/%{NUMBER:http_version}\" %{NUMBER:response_code} %{NUMBER:body_sent_bytes} \"%{DATA:referrer}\" \"%{DATA:agent}\""] }
   	}

  	geoip {
       	source => "remote_ip"
   	}
}
output {
stdout { codec => rubydebug }
 	file { path => "~/Documents/logstash-output.json"}
}

Logstash has three fields input. filter and output. 

1. Input is the soruce of the log files 
2. Filter uses some plugin like to filter the input file
3. Output is used to specify the destination of output log


```
