# adguard-home

![Version: 1.0.0](https://img.shields.io/badge/Version-1.0.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.107.43](https://img.shields.io/badge/AppVersion-v0.107.43-informational?style=flat-square)

A Helm chart for deploying AdGuard Home DNS server

**Homepage:** <https://adguard.com/en/adguard-home/overview.html>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| NitriKx |  |  |

## Source Code

* <https://github.com/AdguardTeam/AdGuardHome>
* <https://hub.docker.com/r/adguard/adguardhome>
* <https://github.com/NitriKx/adguard-home-helm>

## Requirements

Kubernetes: `>=1.20.0`

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
| image.repository | string | `"adguard/adguardhome"` |  |
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
| podSecurityContext.fsGroup | int | `1000` |  |
| podSecurityContext.runAsNonRoot | bool | `true` |  |
| podSecurityContext.runAsUser | int | `1000` |  |
| replicas | int | `1` |  |
| resources | object | `{}` |  |
| securityContext.allowPrivilegeEscalation | bool | `false` |  |
| securityContext.capabilities.drop[0] | string | `"ALL"` |  |
| securityContext.readOnlyRootFilesystem | bool | `false` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `1000` |  |
| services.admin.annotations | object | `{}` |  |
| services.admin.enabled | bool | `true` |  |
| services.admin.labels | object | `{}` |  |
| services.admin.ports.http-beta.port | int | `3000` |  |
| services.admin.ports.http-beta.protocol | string | `"TCP"` |  |
| services.admin.ports.http-beta.targetPort | int | `3000` |  |
| services.admin.ports.http.port | int | `80` |  |
| services.admin.ports.http.protocol | string | `"TCP"` |  |
| services.admin.ports.http.targetPort | int | `80` |  |
| services.admin.ports.https-udp.port | int | `443` |  |
| services.admin.ports.https-udp.protocol | string | `"UDP"` |  |
| services.admin.ports.https-udp.targetPort | int | `443` |  |
| services.admin.ports.https.port | int | `443` |  |
| services.admin.ports.https.protocol | string | `"TCP"` |  |
| services.admin.ports.https.targetPort | int | `443` |  |
| services.admin.type | string | `"ClusterIP"` |  |
| services.dhcp.annotations | object | `{}` |  |
| services.dhcp.enabled | bool | `true` |  |
| services.dhcp.labels | object | `{}` |  |
| services.dhcp.ports.dhcp-client-tcp.port | int | `68` |  |
| services.dhcp.ports.dhcp-client-tcp.protocol | string | `"TCP"` |  |
| services.dhcp.ports.dhcp-client-tcp.targetPort | int | `68` |  |
| services.dhcp.ports.dhcp-client-udp.port | int | `68` |  |
| services.dhcp.ports.dhcp-client-udp.protocol | string | `"UDP"` |  |
| services.dhcp.ports.dhcp-client-udp.targetPort | int | `68` |  |
| services.dhcp.ports.dhcp-server.port | int | `67` |  |
| services.dhcp.ports.dhcp-server.protocol | string | `"UDP"` |  |
| services.dhcp.ports.dhcp-server.targetPort | int | `67` |  |
| services.dhcp.type | string | `"LoadBalancer"` |  |
| services.dns.annotations | object | `{}` |  |
| services.dns.enabled | bool | `true` |  |
| services.dns.labels | object | `{}` |  |
| services.dns.ports.dns-tcp.port | int | `53` |  |
| services.dns.ports.dns-tcp.protocol | string | `"TCP"` |  |
| services.dns.ports.dns-tcp.targetPort | int | `53` |  |
| services.dns.ports.dns.port | int | `53` |  |
| services.dns.ports.dns.protocol | string | `"UDP"` |  |
| services.dns.ports.dns.targetPort | int | `53` |  |
| services.dns.type | string | `"LoadBalancer"` |  |
| services.dnsCrypt.annotations | object | `{}` |  |
| services.dnsCrypt.enabled | bool | `true` |  |
| services.dnsCrypt.labels | object | `{}` |  |
| services.dnsCrypt.ports.dnscrypt-tcp.port | int | `5443` |  |
| services.dnsCrypt.ports.dnscrypt-tcp.protocol | string | `"TCP"` |  |
| services.dnsCrypt.ports.dnscrypt-tcp.targetPort | int | `5443` |  |
| services.dnsCrypt.ports.dnscrypt-udp.port | int | `5443` |  |
| services.dnsCrypt.ports.dnscrypt-udp.protocol | string | `"UDP"` |  |
| services.dnsCrypt.ports.dnscrypt-udp.targetPort | int | `5443` |  |
| services.dnsCrypt.type | string | `"LoadBalancer"` |  |
| services.dnsOverQUIC.annotations | object | `{}` |  |
| services.dnsOverQUIC.enabled | bool | `true` |  |
| services.dnsOverQUIC.labels | object | `{}` |  |
| services.dnsOverQUIC.ports.dns-quic-784.port | int | `784` |  |
| services.dnsOverQUIC.ports.dns-quic-784.protocol | string | `"UDP"` |  |
| services.dnsOverQUIC.ports.dns-quic-784.targetPort | int | `784` |  |
| services.dnsOverQUIC.ports.dns-quic-853.port | int | `853` |  |
| services.dnsOverQUIC.ports.dns-quic-853.protocol | string | `"UDP"` |  |
| services.dnsOverQUIC.ports.dns-quic-853.targetPort | int | `853` |  |
| services.dnsOverQUIC.ports.dns-quic-8853.port | int | `8853` |  |
| services.dnsOverQUIC.ports.dns-quic-8853.protocol | string | `"UDP"` |  |
| services.dnsOverQUIC.ports.dns-quic-8853.targetPort | int | `8853` |  |
| services.dnsOverQUIC.type | string | `"LoadBalancer"` |  |
| services.dnsOverTLS.annotations | object | `{}` |  |
| services.dnsOverTLS.enabled | bool | `true` |  |
| services.dnsOverTLS.labels | object | `{}` |  |
| services.dnsOverTLS.ports.dns-tls.port | int | `853` |  |
| services.dnsOverTLS.ports.dns-tls.protocol | string | `"TCP"` |  |
| services.dnsOverTLS.ports.dns-tls.targetPort | int | `853` |  |
| services.dnsOverTLS.type | string | `"LoadBalancer"` |  |
| tolerations | list | `[]` |  |
| workloadType | string | `"Deployment"` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)
