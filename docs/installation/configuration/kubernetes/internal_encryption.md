# Internal Encryption (TLS/SSL)

!!! warning "If enabling this feature post-deployment, consider pausing the system before proceeding to upgrade"

As of [4.3.1.0](https://github.com/CybercentreCanada/assemblyline/releases/tag/v4.3.1.stable0), the [Assemblyline Helm Chart](https://github.com/CybercentreCanada/assemblyline-helm-chart)
supports enabling end-to-end encryption internally between components.

The flag `enableInternalEncryption` found in the values.yaml is disabled by default
(as to not conflict with pre-existing deployments) but can be enabled. Enabling the feature causes Helm to generate a
Root CA for the release (`{ .Release.Name }.internal-generated-ca`) that's only used to sign certificates for servers in Assemblyline.

## Changes for other Helm charts

Enabling this feature will require some changes to the values used by the accompanying charts that we use.

For example, to enable the feature with the default values using Helm-generated Root CA for verification:

```yaml
---
# Generated certificates are stored in Secrets as <server_hostname>-cert
# Each Secret has two keys: 'tls.crt', 'tls.key' which has their respective public certificate and private key

datastore:
  # Change of protocol for readiness probes
  protocol: https

  # By default, we configure Elasticsearch instances (including the log-storage) to use
  # the contents of '/usr/share/elasticsearch/config/http_ssl/' when settings up HTTPS
  # This can be overridden by setting the environment variables directly, see values.yaml for more details
  extraVolumes: |
    - name: elastic-certificates
      emptyDir: {}
    - name: datastore-master-cert
      secret:
        secretName: datastore-master-cert
  extraVolumeMounts: |
    - name: elastic-certificates
      mountPath: /usr/share/elasticsearch/config/certs
    - name: datastore-master-cert
      mountPath: /usr/share/elasticsearch/config/http_ssl
      readOnly: true

# Internally hosted Kibana instance. For more details, see: https://github.com/elastic/helm-charts/tree/7.17/kibana#configuration
kibanaHost: https://kibana/kibana
kibana:
  elasticsearchHosts: https://log-storage-master:9200

  # For Filebeat, Metricbeat, APM, and Kibana, we mount the Root CA in '/etc/certs/ca.crt'
  # This can be overridden by replacing the ELASTICSEARCH_SSL_CERTIFICATEAUTHORITIES environment variable
  extraVolumes:
    - name: root-ca
      secret:
        secretName: "<release_name>.internal-generated-ca"
  extraVolumeMounts:
    - name: root-ca
      mountPath: /etc/certs/ca.crt
      subPath: tls.crt

# Internally hosted MinIO filestore. For more details, see: https://github.com/minio/charts/tree/main/minio#configuration
filestore:
  tls:
    enabled: true
    certSecret: filestore-cert
    publicCrt: tls.crt
    privateKey: tls.key
```

## Changes to Assemblyline Configuration

Since we're enabling HTTPS on all internal endpoints, this means we also have to update the URLs that we use for connecting
to the various databases.

This includes, but is not limited to:

- `configuration.metrics.apm_server.server_url: http → https`
- `configuration.metrics.elasticsearch.hosts: http → https (for all hosts)`
- `configuration.datastore.hosts: http → https (for all hosts)`
- `configuration.filestore.*: use_ssl=False → use_ssl=True (for all hosts)`

## Changes to Ingress Controller

Because we're enabling full encryption to everything Assemblyline-related, this also includes traffic going from the ingress
controller to the appropriate services. (ie. '/' → `frontend`, '/api/' → `ui`, etc.)

A simple way of doing this is setting the `ingressAnnotation` in your values.yaml to force the ingress controller to forward the request over HTTPS rather than HTTP.

In microk8s, using their built-in ingress, you'd have to add:

`nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"`

However, this particular annotation doesn't work for all ingress controllers, so you'll need to refer to the respective documentation to find the right annotation configuration. For example, the annotation for [NGINX](https://docs.nginx.com/nginx-ingress-controller/configuration/ingress-resources/advanced-configuration-with-annotations/) installed via `helm` would be:

`nginx.org/ssl-services: "ui,frontend,kibana,socketio"`.