Now, hypothetically, in the logs we want some other custom quantity to be the label. We can always search for the custom fields, by adding the required fields in the query. This option of adding the fields in the query, just adds a filter of finding the fields in all the logs.

But, what we want it specifically to be a label, in order to do that we want to change the promtail configuration.

For modifying the log configuration, first we need to understand the structure of the logs.

```json
{
  "level": 50,
  "time": 1736093674621,
  "pid": 1,
  "hostname": "api-769846c849-fmv8v",
  "method": "DELETE",
  "route": "/products",
  "code": "200"
}
```

For example, see the above log, if we want the property `code` or `users` or `products` to be as a label.

In order to make changes in the promtail config, we need to first get the promtail configs, for that we need to again run the following command, plus we need to redirect the output to a `promtail.yaml` file.

```zsh
kubectl get secret loki-promtail -o jsonpath="{.data.promtail\.yaml}" | base64 --decode > promtail.yaml
```

We get the following yaml as the output:

```yaml
server:
  log_level: info
  log_format: logfmt
  http_listen_port: 3101
clients:
  - url: "http://loki:3100/loki/api/v1/push"
positions:
  filename: /run/promtail/positions.yaml
scrape_configs:
  - job_name: kubernetes-pods
    pipeline_stages:
      - cri: {}
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels:
          - __meta_kubernetes_pod_controller_name
        regex: "([0-9a-z-.]+?)(-[0-9a-f]{8,10})?"
        action: replace
        target_label: __tmp_controller_name
      - source_labels:
          - __meta_kubernetes_pod_label_app_kubernetes_io_name
          - __meta_kubernetes_pod_label_app
          - __tmp_controller_name
          - __meta_kubernetes_pod_name
        regex: "^;*([^;]+)(;.*)?$"
        action: replace
        target_label: app
      - source_labels:
          - __meta_kubernetes_pod_label_app_kubernetes_io_instance
          - __meta_kubernetes_pod_label_instance
        regex: "^;*([^;]+)(;.*)?$"
        action: replace
        target_label: instance
      - source_labels:
          - __meta_kubernetes_pod_label_app_kubernetes_io_component
          - __meta_kubernetes_pod_label_component
        regex: "^;*([^;]+)(;.*)?$"
        action: replace
        target_label: component
      - action: replace
        source_labels:
          - __meta_kubernetes_pod_node_name
        target_label: node_name
      - action: replace
        source_labels:
          - __meta_kubernetes_namespace
        target_label: namespace
      - action: replace
        replacement: $1
        separator: /
        source_labels:
          - namespace
          - app
        target_label: job
      - action: replace
        source_labels:
          - __meta_kubernetes_pod_name
        target_label: pod
      - action: replace
        source_labels:
          - __meta_kubernetes_pod_container_name
        target_label: container
      - action: replace
        replacement: /var/log/pods/*$1/*.log
        separator: /
        source_labels:
          - __meta_kubernetes_pod_uid
          - __meta_kubernetes_pod_container_name
        target_label: __path__
      - action: replace
        regex: true/(.*)
        replacement: /var/log/pods/*$1/*.log
        separator: /
        source_labels:
          - __meta_kubernetes_pod_annotationpresent_kubernetes_io_config_hash
          - __meta_kubernetes_pod_annotation_kubernetes_io_config_hash
          - __meta_kubernetes_pod_container_name
        target_label: __path__
limits_config: null
tracing:
  enabled: false
```

Here, under the `pipeline_stages` , we need to add the configurations for extracting our new custom labels `code` and `method`.

```yaml
scrape_configs:
  - job_name: kubernetes-pods
    pipeline_stages:
      - cri: {}
      - match:
          selector: '{app="api"}'
          stages:
            - json:
                expressions:
                  log: null
            - json:
                source: log
                expressions:
                  code: code
                  method: method
            - labels:
                code: null
                method: null
```

We are trying to define a new `match` under which from pod's deployment, we take the selector name as `app="api"`. Then we define the stages:

First, `json` attribute for selecting the `log`, as the structure of our log is `log: {...}`.
Second, another `json` attribute, we now take it's source from `log` defined in the first point, and from the expressions, we extract the `code` and `method`.
Finally, we take both `code` and `method` as labels for our custom labels in Promtail > Loki > Grafana.

### Applying the new Promtail configs

In order to apply the new promtail config, we need to first delete the our promtail secret, and then create a new custom secret which would contain our modified config.

We delete the secret, by running the command:

```zsh
kubectl delete secrets loki-promtail
```

And, again re-create a new secret, by using this command:

```zsh
kubectl create secret generic loki-promtail --from-file=promtail.yaml
```

Now, we need to delete the pod, such that the newly created pod pulls in the newly created secret.

Delete the pods:

```zsh
kubectl delete pods loki-promtail-5qbc7 loki-promtail-9286k loki-promtail-lt89b
```

And, in the grafana dashboard, we can see our custom label being used.
