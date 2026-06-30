# microservice-config

GitOps "desired state" repo — Helm chart + values that represent what
should be running in the cluster. CI from `microservice-app` updates
`helm/microservice-app/values.yaml` (image tag) on every successful build.
The GitOps controller polls this repo and compares it against the live
cluster state to detect drift.

## Structure
```
helm/microservice-app/   # the Helm chart
environments/            # (future) per-environment value overrides, e.g. dev/staging/prod
```

## Manual deploy (before automating via the GitOps controller)
```bash
helm lint helm/microservice-app
helm template test-release helm/microservice-app   # sanity check rendered manifests

helm upgrade --install microservice-app helm/microservice-app \
  --namespace apps --create-namespace
```

## Notes
- Do NOT manually `kubectl edit` resources deployed from this chart once the
  GitOps controller is running in detect-and-alert mode — that's exactly the
  kind of drift it's meant to catch (which is also a great way to demo it
  working).
