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
