# Configuring Loki

In order to configure the querying of logs to Loki, we need to configure the `promtail` to scrape the logs. Here in this example, promtail is scraping logs from the `/var/log/*log` and `<path of our app>/*log`.

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/api/v1/push

scrape_configs:
  - job_name: system
    static_configs:
      - targets:
          - localhost
        labels:
          job: varlogs
          __path__: /var/log/*log

  - job_name: api
    static_configs:
      - targets:
          - localhost
        labels:
          job: apilogs
          __path__: <path of the dir>/*log
```

Now, in order to apply this config and start promtail, we need to run the following command:

```bash
sudo ./promtail-linux-amd64 -config.file=promtail-local-config.yaml
```
