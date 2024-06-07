# Using env secrets

Rather than putting secrets in plain text into the `values.yaml` file, use env variables and kube secrets.

Ensure that in `values.yaml`, `envFromConfigMap` and `envFromSecrets` are set:

```yaml
envFromConfigMap:
  name: unifi-poller-env
envFromSecrets:
  name: unifi-poller-secrets
```

Refer to the [unpoller application configuration](https://unpoller.com/docs/install/configuration/) documentation for the correct env values. Env values will always override values in the config file.

Example `secrets.yaml` file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: unifi-poller-env
data:
  UP_UNIFI_DEFAULT_URL: https://127.0.0.1:8443
  UP_UNIFI_DEFAULT_USER: lessecretapiuser
---
apiVersion: v1
kind: Secret
type: Opaque
metadata:
  name: unifi-poller-secrets
stringData:
  UP_UNIFI_DEFAULT_PASS: supersecretpassword
```
