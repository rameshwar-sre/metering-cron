# usage-hourly Helm Chart

![Version](https://img.shields.io/badge/Version-0.1.0-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.21+-blue)
![Helm](https://img.shields.io/badge/Helm-3.x-blue)

A Helm chart to deploy the **CCS Usage Hourly CronJob** that collects usage metrics every hour and pushes them to ClickHouse/Metering services.

---

## Prerequisites

- Kubernetes 1.21+  
- Helm 3.x  
- Existing namespace or permissions to create one  
- Existing ConfigMap `usage-metric-config` (if using `create: false` in values.yaml)  

---

## Chart Structure

- `cronjob.yaml` – defines the hourly usage collector CronJob  
- `configmap.yaml` – defines the environment variables (optional if existing ConfigMap is used)  
- `values.yaml` – default configurable values  

---

## Installing the Chart

```bash
# Create namespace if not exists
kubectl create namespace ccs-preprod

# Install or upgrade the chart
helm upgrade --install usage-hourly ./usage-hourly -n ccs-preprod
```

---

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| name | string | usage-hourly | CronJob name |
| namespace | string | ccs-preprod | Namespace for CronJob |
| schedule | string | "0 * * * *" | CronJob schedule |
| concurrencyPolicy | string | Forbid | Concurrency policy |
| suspend | bool | false | Suspend the CronJob? |
| successfulJobsHistoryLimit | int | 24 | Number of successful jobs to retain |
| failedJobsHistoryLimit | int | 1 | Number of failed jobs to retain |
| activeDeadlineSeconds | int | 720 | Max seconds a job can run |
| ttlSecondsAfterFinished | int | 5184000 | TTL for finished jobs (60d) |
| image.repository | string | coredgeio/hourly-usage-collector | Container image repository |
| image.tag | string | fluid-1.8.3 | Container image tag |
| image.pullPolicy | string | IfNotPresent | Image pull policy |
| containerName | string | hourly-runner | Container name inside CronJob |
| configMap.create | bool | false | Whether to create a new ConfigMap |
| configMap.name | string | usage-metric-config | ConfigMap name |
| configMap.optional | bool | false | Optional reference to existing ConfigMap |
| restartPolicy | string | Never | Pod restart policy |

---

## Using Existing ConfigMap

If you already have a ConfigMap with your environment variables:

```yaml
configMap:
  create: false
  name: usage-metric-config
```

Helm will not create a new ConfigMap and the CronJob will use the existing one.

---

## Upgrading the Chart

```bash
helm upgrade usage-hourly ./usage-hourly -n ccs-preprod
```

- Modify `values.yaml` or use `--set` flags for updates (image, schedule, etc.)  

---

## Verifying Deployment

```bash
kubectl get cronjob -n ccs-preprod
kubectl describe cronjob -n ccs-preprod usage-hourly
kubectl get pods -n ccs-preprod -w
```

- Ensure jobs are created according to the schedule  
- Check logs of pods for correct execution  

---

## Notes

- Helm release namespace (`-n`) and CronJob `metadata.namespace` can be different.  
- Recommended to keep them same to avoid confusion.  
- The chart is compatible with Kubernetes 1.21+ and Helm 3.x.  
- TTL and `activeDeadlineSeconds` can be adjusted according to cluster policy.  

