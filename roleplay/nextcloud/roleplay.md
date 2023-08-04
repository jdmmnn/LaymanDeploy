# Nextcloud deployment roleplay with LaymanDeploy

## Intent

I want to roleplay the deployment of Nextcloud with my Deployment tool [LaymanDeploy](https://github.com/jdmmnn/LaymanDeploy) to see how it would work out and where blackboxes are and how they could be solved.

## Requirements

- Running LaymanDeploy instance in kubernetes cluster.
- Running Database instance in kubernetes cluster.
  - preferably PostgreSQL, MySQL or MariaDB.
- Running reverse Proxy for cluster.

## Deployment Process

- The User requests the deployment of a Plugin via the Frontend which is then forwarded to the Backend.
- The Backend request the plugin details form the Plugin Repository and starts the Deployment wizard where all necessary input is collected through gotmpl templating.
- The Backend validates the helmfile with the inputs.
- helmfile deploys the Deployment to the Kubernetes cluster.
- The User receives a notification about the successful deployment.

### Plugin Definition Example; inspired by [sovereign-workplace](https://gitlab.opencode.de/bmi/souveraener_arbeitsplatz/deployment/sovereign-workplace/-/tree/main/helmfile/apps/nextcloud)


``helmfile.yaml``
```yaml
---
releases:
  - name: "nextcloud"
    chart: "nextcloud/nextcloud"
    version: "3.5.19"
    needs:
      - "sovereign-workplace-nextcloud-bootstrap"
    values:
      - "values-nextcloud.gotmpl"
      - "values-nextcloud.yaml"
    condition: "nextcloud.enabled"
    timeout: 1800

commonLabels:
  deploy-stage: "component-1"
  component: "nextcloud"

bases:
  - "../../bases/environments.yaml"
...
```

Templating for installation wizzard
``values-nextcloud.gotmpl``
```yaml
---
nextcloud:
  host: "{{ .Values.global.hosts.nextcloud }}.{{ .Values.global.domain }}"
  username: "nextcloud"
  password: {{ .Values.secrets.nextcloud.adminPassword }}
externalDatabase:
  database: "{{ .Values.databases.nextcloud.name }}"
  user: "{{ .Values.databases.nextcloud.username }}"
  host: "{{ .Values.databases.nextcloud.host }}"
  password: "{{ .Values.databases.nextcloud.password | default .Values.secrets.mariadb.nextcloudUser }}"
redis:
  auth:
    enabled: true
    password: {{ .Values.secrets.redis.password }}
ingress:
  enabled: {{ .Values.ingress.enabled }}
  className: {{ .Values.ingress.ingressClassName }}
  tls:
    - secretName: "{{ .Values.ingress.tls.secretName }}"
      hosts:
        - "{{ .Values.global.hosts.nextcloud }}.{{ .Values.global.domain }}"
image:
  repository: "{{ .Values.global.imageRegistry }}/{{ .Values.images.nextcloud.repository }}"
  pullPolicy: "Always"
  tag: "{{ .Values.images.nextcloud.tag }}"
  pullSecrets:
    {{ .Values.global.imagePullSecrets | toYaml | nindent 4 }}

metrics:
  token: "{{ .Values.secrets.nextcloud.metricsToken }}"

{{- if .Values.cluster.persistence.readWriteMany.enabled }}
replicaCount: {{ .Values.replicas.nextcloud }}
{{- else }}
replicaCount: 1
{{- end }}

resources:
  {{ .Values.resources.nextcloud | toYaml | nindent 2 }}
...
```

``values-nextcloud.yaml``
```yaml
---
persistence:
  enabled: true
  existingClaim: "nextcloud-main"
  nextcloudData:
    enabled: true
    existingClaim: "nextcloud-data"

redis:
  enabled: false

cronjob:
  enabled: true
  lifecycle:
    postStartCommand:
      - "sh"
      - "-c"
      - 'sed -i "s/\*\/5 \* \* \* \* php -f \/var\/www\/html\/cron.php/\*\/1 \* \* \* \* php -f \/var\/www\/html\/cron.php/g" /var/spool/cron/crontabs/www-data'

internalDatabase:
  enabled: false
postgresql:
  enabled: false
mariadb:
  enabled: false
externalDatabase:
  enabled: true
  type: "mysql"

metrics:
  enabled: false

# this is not documented but can be found in values.yaml
service:
  port: "80"
```
