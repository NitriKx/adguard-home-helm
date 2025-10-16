# adguard-home

![Version: 1.1.0](https://img.shields.io/badge/Version-1.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.107.65](https://img.shields.io/badge/AppVersion-0.107.65-informational?style=flat-square)

A Helm chart for deploying AdGuard Home DNS server

**Homepage:** <https://adguard.com/en/adguard-home/overview.html>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| NitriKx |  |  |

## Source Code

* <https://github.com/AdguardTeam/AdGuardHome>
* <https://hub.docker.com/r/adguard/adguardhome>
* <https://github.com/11notes/docker-adguard>
* <https://github.com/NitriKx/adguard-home-helm>

## Requirements

Kubernetes: `>=1.20.0`

## Installation

### 1. Install the Helm Chart

```bash
helm repo add adguard-home https://nitrikx.github.io/adguard-home-helm
helm repo update
helm install adguard-home adguard-home/adguard-home
```

### 2. Port Forward the Admin Interface Port

After installation, you need to port forward the admin interface port (3000) to access the AdGuard Home web interface:

```bash
# Port forward the httpAdmin port to localhost:3000
kubectl port-forward svc/adguard-home-admin 3000:http-admin

# Or if you want to use a different local port:
kubectl port-forward svc/adguard-home-admin 8080:http-admin
```

### 3. Complete Initial Setup

1. Open your browser and navigate to `http://localhost:3000` (or your chosen port)
2. Follow the AdGuard Home web interface setup wizard
3. Configure your DNS settings, upstream servers, and other preferences
4. Create an admin user account

### 4. Access the Admin Interface

Once setup is complete, you can continue accessing the admin interface through port 3000:
- Port 3000 (HTTP Admin) - `kubectl port-forward svc/adguard-home-admin 3000:http-admin`
- Port 443 (HTTPS) - `kubectl port-forward svc/adguard-home-admin 8443:443`

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| adguardHome.timezone | string | `"UTC"` |  |
| affinity | object | `{}` |  |
| extraEnvVars | list | `[]` |  |
| extraVolumeMounts | list | `[]` |  |
| extraVolumes | list | `[]` |  |
| healthCheck.enabled | bool | `true` |  |
| healthCheck.livenessProbe.failureThreshold | int | `6` |  |
| healthCheck.livenessProbe.initialDelaySeconds | int | `30` |  |
| healthCheck.livenessProbe.periodSeconds | int | `10` |  |
| healthCheck.livenessProbe.successThreshold | int | `1` |  |
| healthCheck.livenessProbe.timeoutSeconds | int | `5` |  |
| healthCheck.readinessProbe.failureThreshold | int | `6` |  |
| healthCheck.readinessProbe.initialDelaySeconds | int | `5` |  |
| healthCheck.readinessProbe.periodSeconds | int | `10` |  |
| healthCheck.readinessProbe.successThreshold | int | `1` |  |
| healthCheck.readinessProbe.timeoutSeconds | int | `5` |  |
| healthCheck.startupProbe.enabled | bool | `false` |  |
| healthCheck.startupProbe.failureThreshold | int | `30` |  |
| healthCheck.startupProbe.initialDelaySeconds | int | `5` |  |
| healthCheck.startupProbe.periodSeconds | int | `10` |  |
| healthCheck.startupProbe.successThreshold | int | `1` |  |
| healthCheck.startupProbe.timeoutSeconds | int | `5` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"11notes/adguard"` |  |
| image.tag | string | `""` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"adguard-home.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"Prefix"` |  |
| ingress.tls | list | `[]` |  |
| nodeSelector | object | `{}` |  |
| persistence.config.accessModes[0] | string | `"ReadWriteOnce"` |  |
| persistence.config.enabled | bool | `false` |  |
| persistence.config.size | string | `"5Gi"` |  |
| persistence.config.storageClass | string | `""` |  |
| persistence.enabled | bool | `false` |  |
| persistence.work.accessModes[0] | string | `"ReadWriteOnce"` |  |
| persistence.work.enabled | bool | `false` |  |
| persistence.work.size | string | `"1Gi"` |  |
| persistence.work.storageClass | string | `""` |  |
| podSecurityContext | object | `{}` |  |
| replicas | int | `1` |  |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| services.admin.annotations | object | `{}` |  |
| services.admin.enabled | bool | `true` |  |
| services.admin.labels | object | `{}` |  |
| services.admin.ports.httpAdmin.name | string | `"http-admin"` |  |
| services.admin.ports.httpAdmin.port | int | `3000` |  |
| services.admin.ports.httpAdmin.protocol | string | `"TCP"` |  |
| services.admin.ports.httpAdmin.targetPort | int | `3000` |  |
| services.admin.ports.http.name | string | `"http"` |  |
| services.admin.ports.http.port | int | `80` |  |
| services.admin.ports.http.protocol | string | `"TCP"` |  |
| services.admin.ports.http.targetPort | int | `80` |  |
| services.admin.ports.httpsUdp.name | string | `"https-udp"` |  |
| services.admin.ports.httpsUdp.port | int | `443` |  |
| services.admin.ports.httpsUdp.protocol | string | `"UDP"` |  |
| services.admin.ports.httpsUdp.targetPort | int | `443` |  |
| services.admin.ports.https.name | string | `"https"` |  |
| services.admin.ports.https.port | int | `443` |  |
| services.admin.ports.https.protocol | string | `"TCP"` |  |
| services.admin.ports.https.targetPort | int | `443` |  |
| services.admin.type | string | `"ClusterIP"` |  |
| services.dhcp.annotations | object | `{}` |  |
| services.dhcp.enabled | bool | `true` |  |
| services.dhcp.labels | object | `{}` |  |
| services.dhcp.ports.dhcpClientTcp.name | string | `"dhcp-client-tcp"` |  |
| services.dhcp.ports.dhcpClientTcp.port | int | `68` |  |
| services.dhcp.ports.dhcpClientTcp.protocol | string | `"TCP"` |  |
| services.dhcp.ports.dhcpClientTcp.targetPort | int | `68` |  |
| services.dhcp.ports.dhcpClientUdp.name | string | `"dhcp-client-udp"` |  |
| services.dhcp.ports.dhcpClientUdp.port | int | `68` |  |
| services.dhcp.ports.dhcpClientUdp.protocol | string | `"UDP"` |  |
| services.dhcp.ports.dhcpClientUdp.targetPort | int | `68` |  |
| services.dhcp.ports.dhcpServer.name | string | `"dhcp-server"` |  |
| services.dhcp.ports.dhcpServer.port | int | `67` |  |
| services.dhcp.ports.dhcpServer.protocol | string | `"UDP"` |  |
| services.dhcp.ports.dhcpServer.targetPort | int | `67` |  |
| services.dhcp.type | string | `"LoadBalancer"` |  |
| services.dns.annotations | object | `{}` |  |
| services.dns.enabled | bool | `true` |  |
| services.dns.labels | object | `{}` |  |
| services.dns.ports.dnsTcp.name | string | `"dns-tcp"` |  |
| services.dns.ports.dnsTcp.port | int | `53` |  |
| services.dns.ports.dnsTcp.protocol | string | `"TCP"` |  |
| services.dns.ports.dnsTcp.targetPort | int | `53` |  |
| services.dns.ports.dns.name | string | `"dns"` |  |
| services.dns.ports.dns.port | int | `53` |  |
| services.dns.ports.dns.protocol | string | `"UDP"` |  |
| services.dns.ports.dns.targetPort | int | `53` |  |
| services.dns.type | string | `"LoadBalancer"` |  |
| services.dnsCrypt.annotations | object | `{}` |  |
| services.dnsCrypt.enabled | bool | `true` |  |
| services.dnsCrypt.labels | object | `{}` |  |
| services.dnsCrypt.ports.dnscryptTcp.name | string | `"dnscrypt-tcp"` |  |
| services.dnsCrypt.ports.dnscryptTcp.port | int | `5443` |  |
| services.dnsCrypt.ports.dnscryptTcp.protocol | string | `"TCP"` |  |
| services.dnsCrypt.ports.dnscryptTcp.targetPort | int | `5443` |  |
| services.dnsCrypt.ports.dnscryptUdp.name | string | `"dnscrypt-udp"` |  |
| services.dnsCrypt.ports.dnscryptUdp.port | int | `5443` |  |
| services.dnsCrypt.ports.dnscryptUdp.protocol | string | `"UDP"` |  |
| services.dnsCrypt.ports.dnscryptUdp.targetPort | int | `5443` |  |
| services.dnsCrypt.type | string | `"LoadBalancer"` |  |
| services.dnsOverQUIC.annotations | object | `{}` |  |
| services.dnsOverQUIC.enabled | bool | `true` |  |
| services.dnsOverQUIC.labels | object | `{}` |  |
| services.dnsOverQUIC.ports.dnsQuic784.name | string | `"dns-quic-784"` |  |
| services.dnsOverQUIC.ports.dnsQuic784.port | int | `784` |  |
| services.dnsOverQUIC.ports.dnsQuic784.protocol | string | `"UDP"` |  |
| services.dnsOverQUIC.ports.dnsQuic784.targetPort | int | `784` |  |
| services.dnsOverQUIC.ports.dnsQuic853.name | string | `"dns-quic-853"` |  |
| services.dnsOverQUIC.ports.dnsQuic853.port | int | `853` |  |
| services.dnsOverQUIC.ports.dnsQuic853.protocol | string | `"UDP"` |  |
| services.dnsOverQUIC.ports.dnsQuic853.targetPort | int | `853` |  |
| services.dnsOverQUIC.ports.dnsQuic8853.name | string | `"dns-quic-8853"` |  |
| services.dnsOverQUIC.ports.dnsQuic8853.port | int | `8853` |  |
| services.dnsOverQUIC.ports.dnsQuic8853.protocol | string | `"UDP"` |  |
| services.dnsOverQUIC.ports.dnsQuic8853.targetPort | int | `8853` |  |
| services.dnsOverQUIC.type | string | `"LoadBalancer"` |  |
| services.dnsOverTLS.annotations | object | `{}` |  |
| services.dnsOverTLS.enabled | bool | `true` |  |
| services.dnsOverTLS.labels | object | `{}` |  |
| services.dnsOverTLS.ports.dnsTls.name | string | `"dns-tls"` |  |
| services.dnsOverTLS.ports.dnsTls.port | int | `853` |  |
| services.dnsOverTLS.ports.dnsTls.protocol | string | `"TCP"` |  |
| services.dnsOverTLS.ports.dnsTls.targetPort | int | `853` |  |
| services.dnsOverTLS.type | string | `"LoadBalancer"` |  |
| tolerations | list | `[]` |  |
| workloadType | string | `"Deployment"` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)
