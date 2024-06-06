# unifi-poller

![Version: 0.2.0](https://img.shields.io/badge/Version-0.2.0-informational?style=flat-square) ![AppVersion: 2.11.2](https://img.shields.io/badge/AppVersion-2.11.2-informational?style=flat-square)

Collects all UniFi Controller, Site, Device & Client Data - Export to InfluxDB, Prometheus and/or Loki

**Homepage:** <https://github.com/unifi-poller/unifi-poller>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| Gavin Mogan | <helm@gavinmogan.com> |  |
| Matt Ribbins | <matt@mattribbins.co.uk> | <https://mattribbins.co.uk> |

## Source Code

* <https://github.com/unifi-poller/unifi-poller>
* <https://hub.docker.com/r/golift/unifi-poller>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| config.influxdb.db | string | `"unifi"` |  |
| config.influxdb.disable | bool | `true` | Disable influxdb output https://unpoller.com/docs/dependencies/influxdb Warning: Do not use InfluxDB 2.0 or greater at this time. There are no pre-built graphs to display the data it collects. |
| config.influxdb.interval | string | `"30s"` |  |
| config.influxdb.pass | string | `"unifipoller"` | Be sure to create this database. |
| config.influxdb.url | string | `"http://127.0.0.1:8086"` | InfluxDB does not require auth by default, so the user/password are probably unimportant. |
| config.influxdb.user | string | `"unifipoller"` | Strongly advise putting this in the config file, refer to SECRETS.md |
| config.influxdb.verify_ssl | bool | `false` | The UniFi Controller only updates traffic stats about every 30 seconds. Setting this to something lower may lead to "zeros" in your data. If you're getting zeros now, set this to "1m" |
| config.loki.disable | bool | `false` | Disables loki output Enabling will push UniFi's Events, Anomalies, Alarms and IDS data to Loki. https://unpoller.com/docs/dependencies/loki |
| config.loki.interval | string | `"2m"` | Polling interval. The recommended interval is 2m but anything from 1m to 15m should work fine |
| config.loki.pass | string | `"loki"` | Basic auth password. Strongly advise putting this in the config file, refer to SECRETS.md |
| config.loki.tenant_id | string | `""` | Loki tenant ID, alternative to basic auth |
| config.loki.timeout | string | `"10s"` | Timeout period. Adjust if errors start appearing |
| config.loki.url | string | `"http://127.0.0.1:3100"` |  |
| config.loki.user | string | `"loki"` | Basic auth username Note: only basic auth is supported at present |
| config.loki.verify_ssl | bool | `false` | Enable verification of SSL cert |
| config.poller.debug | bool | `false` | Turns on line numbers, microsecond logging, and a per-device log. This may be noisy if you have a lot of devices. It adds one line per device. |
| config.poller.plugins | list | `[]` | Load dynamic plugins. Advanced use; only sample mysql plugin provided by default. |
| config.poller.quiet | bool | `false` | Turns off per-interval logs. Only startup and error logs will be emitted. Recommend enabling debug with this setting for better error logging. |
| config.prometheus.disable | bool | `false` | Disable prometheus output https://unpoller.com/docs/dependencies/prometheus |
| config.prometheus.http_listen | string | `"0.0.0.0:9130"` | IP and port /metrics is exported to. |
| config.prometheus.report_errors | bool | `false` |  |
| config.unifi.controllers | list | `[]` | Repeat any controllers like default above |
| config.unifi.defaults | object | `{"pass":"unifipoller","role":"main controller","save_alarms":false,"save_anomalies":false,"save_dpi":false,"save_events":false,"save_ids":false,"save_sites":true,"sites":["all"],"url":"https://127.0.0.1:8443","user":"unifipoller","verify_ssl":false}` | The following section contains the default credentials/configuration for any dynamic controller (see above section), or the primary controller if you do not provide one and dynamic is disabled. In other words, you can just add your controller here and delete the following section. |
| config.unifi.defaults.pass | string | `"unifipoller"` | Password to talk to unifi. Strongly advise putting this in the config file, refer to SECRETS.md |
| config.unifi.defaults.role | string | `"main controller"` | Username to talk to unifi. Strongly advise putting this in the config file, refer to SECRETS.md |
| config.unifi.defaults.save_alarms | bool | `false` | Enable collection of alarms (InfluxDB / Loki only) |
| config.unifi.defaults.save_anomalies | bool | `false` | Enable collection of anomalies (InfluxDB / Loki only) |
| config.unifi.defaults.save_dpi | bool | `false` | Enable collection of Deep Packet Inspection data. This data breaks down traffic types for each client and site, it powers a dedicated DPI dashboard. Enabling this adds roughly 150 data points per client.  That's 6000 metrics for 40 clients.  This adds a little bit of poller run time per interval and causes more API requests to your controller(s). Don't let these "cons" sway you: it's cool data. Please provide feedback on your experience with this feature. |
| config.unifi.defaults.save_events | bool | `false` | Enable collection of events (InfluxDB / Loki only) |
| config.unifi.defaults.save_ids | bool | `false` | Enable collection of Intrusion Detection System Data (InfluxDB / Loki only). Only useful if IDS or IPS are enabled on one of the sites. |
| config.unifi.defaults.save_sites | bool | `true` | Enable collection of site data. This data powers the Network Sites dashboard. It's not valuable to everyone and setting this to false will save resources. |
| config.unifi.defaults.sites | list | `["all"]` | If the controller has more than one site, specify which sites to poll here. Set this to ["default"] to poll only the first site on the controller. A setting of ["all"] will poll all sites; this works if you only have 1 site too. |
| config.unifi.defaults.url | string | `"https://127.0.0.1:8443"` | Url to talk to unifi |
| config.unifi.defaults.verify_ssl | bool | `false` | If your UniFi controller has a valid SSL certificate (like lets encrypt), you can enable this option to validate it. Otherwise, any SSL certificate is valid. If you don't know if you have a valid SSL cert, then you don't have one |
| config.unifi.dynamic | bool | `false` | Setting this to true and providing default credentials allows you to skip configuring controllers in this config file. Instead you configure them in your prometheus.yml config. Prometheus then sends the controller URL to unifi-poller when it performs the scrape. This is useful if you have many, or changing controllers. Most people can leave this off. See wiki for more. |
| config.webserver.accounts | object | `{"captain":"$2a$04$mxw6i0LKH6u46oaLK2cq5eCTAAFkfNiRpzNbz.EyvJZZWNa2FzIlS"}` | Web server accounts. Captain is the default provider user, change!! Passwords are hashed and generated via the command `unpoller -e -` If commented out, authentication is disabled |
| config.webserver.enable | bool | `false` | Enables the built in web server and API. Advanced use, see wiki https://unpoller.com/docs/advanced/API |
| config.webserver.html_path | string | `"/usr/local/lib/unpoller/web"` | HTML web path, should not require changing |
| config.webserver.max_events | int | `200` |  |
| config.webserver.port | int | `37288` | Port for the webserver |
| config.webserver.ssl_cert_path | string | `""` | Path of the SSL certificate |
| config.webserver.ssl_key_path | string | `""` | Path of the SSL private key |
| envFromConfigMap | object | `{}` |  |
| envFromSecrets | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"golift/unifi-poller"` |  |
| image.tag | string | `"v2.11.2"` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"unifi-poller.local"` |  |
| ingress.hosts[0].paths | list | `[]` |  |
| ingress.tls | list | `[]` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| pod.annotations | object | `{}` |  |
| pod.labels | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| replicaCount | int | `1` |  |
| resources.limits.cpu | float | `0.3` |  |
| resources.limits.memory | string | `"64Mi"` |  |
| resources.requests.cpu | float | `0.05` |  |
| resources.requests.memory | string | `"32Mi"` |  |
| securityContext.allowPrivilegeEscalation | bool | `false` |  |
| securityContext.capabilities.drop[0] | string | `"ALL"` |  |
| securityContext.readOnlyRootFilesystem | bool | `true` |  |
| securityContext.runAsGroup | int | `10010` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `10010` |  |
| securityContext.seccompProfile.type | string | `"RuntimeDefault"` |  |
| service.annotations | object | `{}` |  |
| service.port | int | `9130` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `nil` |  |
| serviceMonitor.enabled | bool | `true` |  |
| serviceMonitor.scrapeInterval | string | `"30s"` |  |
| tolerations | list | `[]` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.13.1](https://github.com/norwoodj/helm-docs/releases/v1.13.1)
