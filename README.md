<<<<<<< HEAD
# extra-manifest

A minimal [Helm](https://helm.sh/) chart that deploys arbitrary Kubernetes manifests. Use it to add extra resources (ConfigMaps, Secrets, or any valid Kubernetes YAML) to a release, or as a standalone way to manage a set of “extra” objects with Helm templating.

## Features

- **Flexible input**: `extraManifests` can be a list or a dict; items can be full YAML objects or raw strings.
- **Templating**: All entries are passed through Helm’s `tpl`, so you can use `{{ .Values.* }}`, `{{ include ... }}`, and other chart context.
- **Any resource kind**: Deploy ConfigMaps, Secrets, Deployments, or any other Kubernetes resource defined in your values.

## Installation

Add the chart repo (if using a Helm repository):

```bash
helm repo add <repo-name> <repo-url>
helm repo update
```

Install or upgrade the chart:

```bash
helm upgrade --install extra-manifest <repo-name>/extra-manifest \
  --namespace my-namespace \
  -f my-values.yaml
```

Or install from a local checkout:

```bash
helm upgrade --install extra-manifest . \
  --namespace my-namespace \
  -f my-values.yaml
```

## Configuration

| Parameter        | Type   | Default | Description                                      |
|-----------------|--------|---------|--------------------------------------------------|
| `extraManifests`| list/dict | `null` | Kubernetes manifests to deploy (see examples).   |

### Defining `extraManifests`

- **List**: list of manifests; order is preserved.
- **Dict**: keys are ignored; values are used as the list of manifests.

Each item can be:

1. **A YAML object (dict)**  
   Rendered with `tpl(toYaml .)`, so you can use templating inside values.

2. **A string**  
   Treated as a template and rendered with `tpl`; useful for multi-line or heavily templated manifests.

## Examples

### ConfigMap (object form)

```yaml
extraManifests:
  - apiVersion: v1
    kind: ConfigMap
    metadata:
      name: prometheus-extra
      labels:
        app: prometheus
    data:
      extra-data: "value"
```

### Secret (object form)

```yaml
extraManifests:
  - apiVersion: v1
    kind: Secret
    type: Opaque
    metadata:
      name: my-secret
    data:
      plaintext: Zm9vYmFy
      templated: '{{ print "foobar" | upper | b64enc }}'
```

### Templated string (e.g. with common labels)

```yaml
extraManifests:
  - |
    apiVersion: v1
    kind: Secret
    type: Opaque
    metadata:
      name: super-secret
      labels:
        {{- range $key, $value := .Values.commonLabels }}
        {{ $key }}: {{ $value | quote }}
        {{- end }}
    data:
      plaintext: Zm9vYmFy
      templated: '{{ print "foobar" | upper | b64enc }}'
```

### Dict form (keys ignored)

```yaml
extraManifests:
  config:
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-config
    data:
      key: value
  secret:
    apiVersion: v1
    kind: Secret
    metadata:
      name: my-secret
    data:
      token: {{ .Values.token | b64enc }}
```

## Use cases

- Attach extra ConfigMaps or Secrets to an existing Helm-based stack by installing this chart with the same release name/namespace or alongside it.
- Deploy a set of “bootstrap” or one-off resources (e.g. CRDs, default ConfigMaps) with versioning and rollback via Helm.
- Share the same `values.yaml` structure (e.g. `commonLabels`, image registries) across multiple arbitrary manifests using `tpl`.

## Requirements

- Kubernetes cluster
- Helm 3.x
- `extraManifests` must be valid Kubernetes resource YAML (or templates that render to it)

## License

See repository license.
||||||| parent of 3c858d8 (Add files)
=======
# extra-manifest

A minimal [Helm](https://helm.sh/) chart that deploys arbitrary Kubernetes manifests. Use it to add extra resources (ConfigMaps, Secrets, or any valid Kubernetes YAML) to a release, or as a standalone way to manage a set of “extra” objects with Helm templating.

## Features

- **Flexible input**: `extraManifests` can be a list or a dict; items can be full YAML objects or raw strings.
- **Templating**: All entries are passed through Helm’s `tpl`, so you can use `{{ .Values.* }}`, `{{ include ... }}`, and other chart context.
- **Any resource kind**: Deploy ConfigMaps, Secrets, Deployments, or any other Kubernetes resource defined in your values.

## Installation

Add the chart repo (if using a Helm repository):

```bash
helm repo add <repo-name> <repo-url>
helm repo update
```

Install or upgrade the chart:

```bash
helm upgrade --install extra-manifest <repo-name>/extra-manifest \
  --namespace my-namespace \
  -f my-values.yaml
```

Or install from a local checkout:

```bash
helm upgrade --install extra-manifest . \
  --namespace my-namespace \
  -f my-values.yaml
```

## Configuration

| Parameter        | Type   | Default | Description                                      |
|-----------------|--------|---------|--------------------------------------------------|
| `extraManifests`| list/dict | `null` | Kubernetes manifests to deploy (see examples).   |

### Defining `extraManifests`

- **List**: list of manifests; order is preserved.
- **Dict**: keys are ignored; values are used as the list of manifests.

Each item can be:

1. **A YAML object (dict)**
   Rendered with `tpl(toYaml .)`, so you can use templating inside values.

2. **A string**
   Treated as a template and rendered with `tpl`; useful for multi-line or heavily templated manifests.

## Examples

### ConfigMap (object form)

```yaml
extraManifests:
  - apiVersion: v1
    kind: ConfigMap
    metadata:
      name: prometheus-extra
      labels:
        app: prometheus
    data:
      extra-data: "value"
```

### Secret (object form)

```yaml
extraManifests:
  - apiVersion: v1
    kind: Secret
    type: Opaque
    metadata:
      name: my-secret
    data:
      plaintext: Zm9vYmFy
      templated: '{{ print "foobar" | upper | b64enc }}'
```

### Templated string (e.g. with common labels)

```yaml
extraManifests:
  - |
    apiVersion: v1
    kind: Secret
    type: Opaque
    metadata:
      name: super-secret
      labels:
        {{- range $key, $value := .Values.commonLabels }}
        {{ $key }}: {{ $value | quote }}
        {{- end }}
    data:
      plaintext: Zm9vYmFy
      templated: '{{ print "foobar" | upper | b64enc }}'
```

### Dict form (keys ignored)

```yaml
extraManifests:
  config:
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-config
    data:
      key: value
  secret:
    apiVersion: v1
    kind: Secret
    metadata:
      name: my-secret
    data:
      token: {{ .Values.token | b64enc }}
```

## Use cases

- Attach extra ConfigMaps or Secrets to an existing Helm-based stack by installing this chart with the same release name/namespace or alongside it.
- Deploy a set of “bootstrap” or one-off resources (e.g. CRDs, default ConfigMaps) with versioning and rollback via Helm.
- Share the same `values.yaml` structure (e.g. `commonLabels`, image registries) across multiple arbitrary manifests using `tpl`.

## Requirements

- Kubernetes cluster
- Helm 3.x
- `extraManifests` must be valid Kubernetes resource YAML (or templates that render to it)

## License

See repository license.
>>>>>>> 3c858d8 (Add files)
