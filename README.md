# Kafka / Vector pods
Listen for syslog messages at port 5140 and put them into Kafka queue at port 9094.
The topic "syslog-queue" is configured in the [vector.toml](vector.toml) file.
No security and no password.

## Launch pods
~~~
podman play kube kafka-vector-pod.yaml
~~~

## Tear down pods
~~~
podman play kube --down kafka-vector-pod.yaml
~~~

## Monitor the status of the Kafka queue
~~~
podman exec -it kafka-syslog-pod-kafka kafka-console-consumer --topic syslog-queue \
--from-beginning --bootstrap-server localhost:9094
~~~

## Send a test message using logger
~~~
logger -d -n localhost -P 5140 "Test syslog message from my local machine"
~~~

## Send a test JSON message using kafka producer
~~~
podman exec -i kafka-syslog-pod-kafka kafka-console-producer --bootstrap-server localhost:9094 \
--topic syslog-queue < sample.json
~~~

## Run an Ansible Rulebook
~~
ansible-rulebook -r rulebooks/kafka-test.yml
~~

## Setup RHEL to forward syslog (optional)
Add destination to `/etc/rsyslog.conf`

~~~
*.* @ryzen5.lab.automate.nyc:9094
~~~

or TCP
~~~
*.* @@ryzen5.lab.automate.nyc:9094
~~~

~~~
sudo semanage port -a -t syslogd_port_t -p udp 9094
sudo systemctl restart rsyslog
sudo systemctl status rsyslog
~~~
