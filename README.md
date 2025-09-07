# AdGuard Home Helm Chart

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Helm](https://img.shields.io/badge/Helm-3-blue.svg)](https://helm.sh/)

A Helm chart for deploying [AdGuard Home](https://adguard.com/en-adguard-home/overview.html) - a network-wide ad and tracker blocking DNS server.

[![CI](https://github.com/NitriKx/adguard-home-helm/workflows/CI/badge.svg)](https://github.com/NitriKx/adguard-home-helm/actions)

## Prerequisites

- Kubernetes 1.20+
- Helm 3.0+ with [unittest plugin](https://github.com/helm-unittest/helm-unittest)
- [Task](https://taskfile.dev/) (optional, for development workflow automation)
- [pre-commit](https://pre-commit.com/) (optional, for automated code quality checks)

## Installing the Chart

Add the Helm repository and install the chart:

```bash
# Add the repository
helm repo add nitrikx https://nitrikx.github.io/adguard-home-helm
helm repo update

# Install the chart
helm install adguard-home nitrikx/adguard-home
```

## Configuration

The following table lists the configurable parameters of the AdGuard Home chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `workloadType` | Workload type: Deployment or StatefulSet | `Deployment` |
| `replicas` | Number of replicas | `1` |
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
| `healthCheck.enabled` | Enable readiness and liveness probes | `true` |
| `healthCheck.readinessProbe.*` | Readiness probe configuration | See values.yaml |
| `healthCheck.livenessProbe.*` | Liveness probe configuration | See values.yaml |
| `healthCheck.startupProbe.*` | Startup probe configuration (K8s 1.18+) | See values.yaml |
| `adguardHome.timezone` | Timezone for AdGuard Home container | `"UTC"` |
| `adguardHome.configYaml` | Custom AdGuard Home configuration YAML | `""` |

### AdGuard Home Configuration

The chart provides configuration options specific to AdGuard Home:

#### Timezone Configuration

AdGuard Home uses timezone information for log timestamps and scheduled tasks. Configure the timezone using:

```yaml
adguardHome:
  timezone: "America/New_York"  # or any valid timezone
```

**Common timezone examples:**
- `"UTC"` - Coordinated Universal Time
- `"America/New_York"` - Eastern Time
- `"Europe/London"` - British Summer Time
- `"Asia/Tokyo"` - Japan Standard Time
- `"Australia/Sydney"` - Australian Eastern Time

#### Custom Configuration YAML

You can provide a complete AdGuard Home configuration by specifying a custom YAML configuration. When provided, the chart will create an init container that writes your configuration to `AdGuardHome.yaml` on every pod startup.

**Important Notes:**
- The configuration is written on every pod restart, ensuring your settings are always applied
- The init container uses a lightweight BusyBox image
- Only specify this if you need full control over AdGuard Home configuration

**Basic Configuration Example:**
```yaml
adguardHome:
  configYaml: |
    bind_host: 0.0.0.0
    bind_port: 80
    dns:
      bind_hosts:
        - 0.0.0.0
      port: 53
      upstream_dns:
        - 8.8.8.8
        - 1.1.1.1
    filtering:
      protection_enabled: true
      filtering_enabled: true
```

**Advanced Configuration with Authentication:**
```yaml
adguardHome:
  configYaml: |
    bind_host: 0.0.0.0
    bind_port: 80
    users:
      - name: admin
        password: $2a$10$example.hash.here
    dns:
      bind_hosts:
        - 0.0.0.0
      port: 53
      upstream_dns:
        - https://dns.google/dns-query
        - https://cloudflare-dns.com/dns-query
      bootstrap_dns:
        - 8.8.8.8
        - 1.1.1.1
    filtering:
      protection_enabled: true
      filtering_enabled: true
      safe_search:
        enabled: true
      parental_control:
        enabled: false
    dhcp:
      enabled: false
    tls:
      enabled: true
      server_name: adguard.example.com
      force_https: true
    log:
      enabled: true
      file: ""
      max_backups: 0
      max_size: 100
      max_age: 3
      compress: false
      local_time: false
      verbose: false
```

**Configuration Reference:**
For a complete list of available configuration options, refer to the [AdGuard Home Configuration Documentation](https://github.com/AdguardTeam/AdGuardHome/wiki/Configuration).

### Image Configuration

By default, the chart uses the `appVersion` from `Chart.yaml` as the image tag. If you want to use a specific version, you can override it:

```yaml
# values.yaml
image:
  tag: "v0.108.0"  # Override to use a specific version
```

If `image.tag` is left empty or not specified, it will automatically use the version defined in the Chart's `appVersion` field.

### Workload Configuration

#### Deployment vs StatefulSet

The chart supports both Kubernetes Deployment and StatefulSet workload types:

**Deployment** (default):
- Suitable for stateless or simple stateful applications
- Uses separate PersistentVolumeClaims for storage
- Easier scaling and rolling updates
- No persistent pod identity

**StatefulSet**:
- Suitable for stateful applications requiring persistent identity
- Uses volumeClaimTemplates for automatic PVC creation
- Ordered deployment and scaling
- Stable network identity and storage

```yaml
# Use StatefulSet with 2 replicas
workloadType: StatefulSet
replicas: 2

# Use Deployment with 3 replicas (default)
workloadType: Deployment
replicas: 3
```

**Important Notes:**
- When using StatefulSet, PVCs are managed automatically via volumeClaimTemplates
- The chart will not create separate PVC resources when StatefulSet is selected
- StatefulSet provides stable pod names (e.g., `adguard-home-0`, `adguard-home-1`)
- Consider using StatefulSet if you need:
  - Persistent pod identity
  - Ordered deployment/scaling
  - Stable storage that survives pod rescheduling

### Example Configuration

```
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

## Development

### Pre-commit Hooks

This repository includes pre-commit hooks to ensure code quality. The hooks automatically run linting and validation checks before each commit.

#### Setup

### Use pre-commit framework**
```bash
# Install pre-commit
pip install pre-commit

# Install the pre-commit hooks
pre-commit install --install-hooks

# Run manually on all files
pre-commit run --all-files
```


### Development Tasks

This repository includes a [Taskfile](https://taskfile.dev/) for development workflow automation. If you have Task installed, you can use the following commands:

```bash
# List all available tasks
task --list

# Lint the Helm chart
task lint

# Render chart templates
task template

# Package the chart
task package

# Install the chart locally (requires Kubernetes cluster)
task install

# Uninstall the chart
task uninstall
```

## Testing

This chart includes comprehensive unit tests using the [Helm unittest plugin](https://github.com/helm-unittest/helm-unittest). The tests validate different configurations and ensure the chart works correctly.

### Running Tests

```bash
# Run all tests
helm unittest charts/adguard-home

# Run specific test file
helm unittest charts/adguard-home --file default_test.yaml

# Run with verbose output
helm unittest charts/adguard-home -v
```


## CI/CD

### Release Management

This repository uses [release-please](https://github.com/google-github-actions/release-please-action) for automated release management:

#### How It Works

1. **Conventional Commits**: Use conventional commit format for changes
2. **Automatic PR Creation**: release-please creates release PRs automatically
3. **Version Bumping**: Versions are bumped based on commit types
4. **Changelog Generation**: Automatic CHANGELOG.md generation
5. **GitHub Releases**: Automatic creation of GitHub releases

#### Commit Types

- `feat:` - New features (minor version bump)
- `fix:` - Bug fixes (patch version bump)
- `docs:` - Documentation changes
- `refactor:` - Code refactoring
- `test:` - Test additions/changes
- `chore:` - Maintenance tasks

#### Release Process

1. Push commits to `main` branch
2. release-please creates/updates a release PR
3. Review and merge the release PR
4. release-please creates a GitHub release with tag
5. Release workflow packages and publishes the chart

### Local Development

For local development, you can run the same checks using the Taskfile:

```bash
# Run linting
task lint

# Run tests
helm unittest charts/adguard-home

# Package chart
task package
```

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

**Note**: All pull requests must pass the CI pipeline before merging.

## License

This Helm chart is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## Links

- [AdGuard Home Documentation](https://github.com/AdguardTeam/AdGuardHome/wiki)
- [AdGuard Home Docker Image](https://hub.docker.com/r/adguard/adguardhome)
- [Helm Documentation](https://helm.sh/docs/)
