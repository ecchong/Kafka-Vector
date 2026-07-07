## Launch
podman play kube kafka-vector-pod.yaml

## Tear down
podman play kube --down kafka-vector-pod.yaml

## Monitor
podman exec -it kafka-syslog-pod-kafka kafka-console-consumer --topic syslog-queue --from-beginning --bootstrap-server localhost:9092

## Test
logger -d -n localhost -P 5140 "Test syslog message from my local machine"

