## Start Demo

--8<-- "snippets/codespace-details-warning-box.md"
--8<-- "snippets/bizevent-start-demo.js"

Click this button to launch the demo in a new tab.


=== "Run in Cloud"
    --8<-- "snippets/codespace-details-warning-box.md"
    --8<-- "snippets/start-demo-run-in-cloud.md"

=== "Run Locally"
    --8<-- "snippets/start-demo-run-locally.md"

## Start the system

You will now start two containers on the same Docker network:

1. Dynatrace OpenTelemetry collector: Listens for incoming NetFlow data and sends it onwards to Dynatrace
1. nflow-generator: Dummy device which generates the netflow data

### Create a new Network

First, create a new docker network so both containers are on the same network and thus nflow-generator can find and send data to the collector:

```
docker network create shared
```

## Start the Collector

Start the collector by running the command as-is (leave this running):

```
docker run --rm -p 2055:2055 \
  -e DT_ENDPOINT=https://$DT_ENVIRONMENT_ID.live.dynatrace.com/api/v2/otlp \
  -e DT_API_TOKEN=$DT_API_TOKEN \
  -v .:/app \
  --name dtcol \
  --network shared \
  ghcr.io/dynatrace/dynatrace-otel-collector/dynatrace-otel-collector:0.30.0 --config=app/config.yaml
```

The configuration of the collector can be found [here](https://github.com/Dynatrace/demo-netflow/blob/main/config.yaml){target=_blank}.
The collector is configured to listen for netflow data on `2055` then export it simultaneously to both `debug` and Dynatrace via the `OTLP HTTP` protocol. The debug output prints it to stdout (look at the collector terminal for output).

!!! note "Remove debug in production"
    Debug logging is used here but is not recommended for production. Remove `debug` from the `exporters` block in production.

## Start nflow-generator

Open a new terminal window and start the NetFlow generator device which will send data to the collector:

```
docker run -it --rm \
  --network shared \
  --name nflowgen \
  networkstatic/nflow-generator -t dtcol -p 2055
```

## View Debug Data

Open the collector terminal window (the first one launched). You should now see data flowing through the collector on the way to Dynatrace.

```
LogRecord #1
ObservedTimestamp: 2025-06-12 05:10:32.532212841 +0000 UTC
Timestamp: 2025-06-12 05:10:31.965015972 +0000 UTC
SeverityText:
SeverityNumber: Unspecified(0)
Body: Empty()
Attributes:
     -> source.address: Str(172.16.50.10)
     -> source.port: Int(0)
     -> destination.address: Str(132.12.130.10)
     -> destination.port: Int(0)
     -> network.transport: Str(icmp)
     -> network.type: Str(ipv4)
     -> flow.io.bytes: Int(51)
     -> flow.io.packets: Int(44)
     -> flow.type: Str(netflow_v5)
     -> flow.sequence_num: Int(19)
     -> flow.time_received: Int(1749705032532212841)
     -> flow.start: Int(1749705031965015972)
     -> flow.end: Int(1749705032237015972)
     -> flow.sampling_rate: Int(0)
     -> flow.sampler_address: Str(10.89.1.3)
Trace ID:
Span ID:
Flags: 0
```

<div class="grid cards" markdown>
- [Click here to continue :octicons-arrow-right-24:](view-data.md)
</div>
