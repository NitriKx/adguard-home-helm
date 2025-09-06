# AdGuard Home Helm Chart

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Helm](https://img.shields.io/badge/Helm-3-blue.svg)](https://helm.sh/)

A Helm chart for deploying [AdGuard Home](https://adguard.com/en/adguard-home/overview.html) - a network-wide ad and tracker blocking DNS server.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+

## Installing the Chart

To install the chart with the release name `adguard-home`:

```bash
helm repo add adguard-home https://your-repo-url/charts
helm repo update
helm install adguard-home adguard-home/adguard-home
```

Alternatively, you can install the chart directly from this repository:

```bash
helm install adguard-home ./charts/adguard-home
```

## Uninstalling the Chart

To uninstall the `adguard-home` deployment:

```bash
helm uninstall adguard-home
```

## Configuration

The following table lists the configurable parameters of the AdGuard Home chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | AdGuard Home image repository | `adguard/adguardhome` |
| `image.tag` | AdGuard Home image tag (uses Chart appVersion if empty) | `""` (uses appVersion) |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `service.type` | Kubernetes service type | `ClusterIP` |
| `service.port` | HTTP service port | `80` |
| `service.httpsPort` | HTTPS service port | `443` |
| `service.dnsPort` | DNS service port | `53` |
| `service.dnsTlsPort` | DNS over TLS service port | `853` |
| `ingress.enabled` | Enable ingress | `false` |
| `ingress.className` | Ingress class name | `""` |
| `ingress.hosts` | Ingress hosts configuration | `[{"host": "adguard-home.local", "paths": [{"path": "/", "pathType": "Prefix"}]}]` |
| `ingress.tls` | Ingress TLS configuration | `[]` |
| `persistence.enabled` | Enable persistence | `true` |
| `persistence.accessMode` | PVC access mode | `ReadWriteOnce` |
| `persistence.size` | PVC storage size | `1Gi` |
| `persistence.storageClass` | Storage class name | `""` |
| `resources.limits.cpu` | CPU limit | `500m` |
| `resources.limits.memory` | Memory limit | `512Mi` |
| `resources.requests.cpu` | CPU request | `100m` |
| `resources.requests.memory` | Memory request | `128Mi` |
| `nodeSelector` | Node labels for pod assignment | `{}` |
| `tolerations` | Tolerations for pod assignment | `[]` |
| `affinity` | Affinity for pod assignment | `{}` |

### Image Configuration

By default, the chart uses the `appVersion` from `Chart.yaml` as the image tag. If you want to use a specific version, you can override it:

```yaml
# values.yaml
image:
  tag: "v0.108.0"  # Override to use a specific version
```

If `image.tag` is left empty or not specified, it will automatically use the version defined in the Chart's `appVersion` field.

### Example Configuration

service:
  type: LoadBalancer

ingress:
  enabled: true
  hosts:
    - host: adguard.yourdomain.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: adguard-tls
      hosts:
        - adguard.yourdomain.com

persistence:
  size: 5Gi
  storageClass: "fast-ssd"

resources:
  limits:
    cpu: 1000m
    memory: 1Gi
  requests:
    cpu: 200m
    memory: 256Mi
```

## Accessing AdGuard Home

### Via Service

If you're using `ClusterIP` service type, you can access AdGuard Home through a port-forward:

```bash
kubectl port-forward svc/adguard-home 3000:80
```

Then visit `http://localhost:3000` in your browser.

### Via Ingress

If ingress is enabled, AdGuard Home will be accessible at the configured host.

### Via LoadBalancer

If you're using `LoadBalancer` service type, get the external IP:

```bash
kubectl get svc adguard-home
```

## DNS Configuration

AdGuard Home acts as a DNS server. To use it as your DNS server:

1. **Internal Cluster Access**: Configure your applications to use the service DNS name: `adguard-home.default.svc.cluster.local`

2. **External Access**: Use the LoadBalancer IP or Ingress host as your DNS server

3. **Network-wide**: Configure your router or DHCP server to use AdGuard Home as the DNS server

## Initial Setup

After deployment, you'll need to complete the initial setup:

1. Access the web interface at the configured endpoint
2. Follow the setup wizard
3. Configure your DNS settings
4. Set up filtering rules and blocklists

## Persistence

By default, the chart creates two PersistentVolumeClaims:
- `adguard-home-config`: Stores AdGuard Home configuration
- `adguard-home-work`: Stores work data and logs

Both PVCs use the same storage class and size configuration.

## Security Considerations

- The chart runs AdGuard Home with privileged access by default (required for DNS functionality)
- Consider configuring TLS/HTTPS for production deployments
- Use network policies to restrict access to the AdGuard Home service
- Regularly update the AdGuard Home image for security patches

## Upgrading

To upgrade the chart:

```bash
helm upgrade adguard-home ./charts/adguard-home
```

To upgrade to a new AdGuard Home version, update the `image.tag` value:

```bash
helm upgrade adguard-home ./charts/adguard-home --set image.tag=v0.107.44
```

## Troubleshooting

### Check Pod Status
```bash
kubectl get pods -l app.kubernetes.io/name=adguard-home
```

### View Logs
```bash
kubectl logs -l app.kubernetes.io/name=adguard-home
```

### Check PVC Status
```bash
kubectl get pvc -l app.kubernetes.io/name=adguard-home
```

### Common Issues

1. **Port Conflicts**: Ensure ports 53, 80, 443, 853 are not already in use
2. **Permission Issues**: AdGuard Home requires root privileges for DNS functionality
3. **Storage Issues**: Ensure your storage class has sufficient space

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## License

This Helm chart is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## Links

- [AdGuard Home Documentation](https://github.com/AdguardTeam/AdGuardHome/wiki)
- [AdGuard Home Docker Image](https://hub.docker.com/r/adguard/adguardhome)
- [Helm Documentation](https://helm.sh/docs/)
