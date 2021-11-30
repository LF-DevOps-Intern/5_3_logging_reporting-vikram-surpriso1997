# Installatin of logstatsh and formatting of nginx logs

1.  Installation of logstash:
    First the required gpg keys and repos of elastic are added and then log stash is installed:

```bash

 wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add

 sudo apt-get install apt-transport-https

 echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

 sudo apt-get update && sudo apt-get install logstash


```

![install ](https://github.com/LF-DevOps-Intern/5_3_logging_reporting-vikram-surpriso1997/blob/main/4/screenshot/install-logstash-dependenceis.png)

Checking status of log stash service:

![chekcing staus](https://github.com/LF-DevOps-Intern/5_3_logging_reporting-vikram-surpriso1997/blob/main/4/screenshot/logstash-running.png)

2. Dowloading the nginx log file:

```bash

    wget https://github.com/elastic/examples/blob/master/Common%20Data%20Formats/nginx_logs/nginx_logs

```

![wget](https://github.com/LF-DevOps-Intern/5_3_logging_reporting-vikram-surpriso1997/blob/main/4/screenshot/wget%20nginx%20logs.png)

3. Creation of conf file for parsing nginx logs:
   A new conf file logstash.conf is created at `/etc/logstash/conf.d/`:

```conf

input {
	file {
     		path => ["~/Documnents/ops-docs/nginx-logs"]
        	start_position => "beginning"
			sincedb_path=> "/dev/null"

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
 	file { path => "~/Documents/ops-docs/logstash-output.json"}
}

![conf file](https://github.com/LF-DevOps-Intern/5_3_logging_reporting-vikram-surpriso1997/blob/main/4/screenshot/nginx-confi-file.png)

Logstash has three fields input. filter and output.

1. Input is the soruce of the log files
2. Filter uses some plugin like to filter the input file
3. Output is used to specify the destination of output log


```

4. Logstash can be running the follwoing command from `/usr/share/logstash` dir:

```bash

sudo bin/logstash --path.settings /etc/logstash --path.data sensor39 -f /etc/logstash/conf.d/logstash.conf

```

But I encountered this where the output file is not generated and no obvious errors while running the logstash:

![logstah error](https://github.com/LF-DevOps-Intern/5_3_logging_reporting-vikram-surpriso1997/blob/main/4/screenshot/error-logstash-.png)
