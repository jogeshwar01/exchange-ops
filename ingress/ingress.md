#### Install nginx ingress controller

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.2/deploy/static/provider/cloud/deploy.yaml

#### Ingress yml for separate urls

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-nginx
spec:
  ingressClassName: nginx
  rules:
    - host: exchange-backend.jogeshwar.xyz
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: exchange-router-service
                port:
                  number: 80
    - host: exchange-ws.jogeshwar.xyz
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: exchange-ws-stream-service
                port:
                  number: 80
```

#### What does `ImplementationSpecific` mean for `pathType`?

In Kubernetes Ingress, the `pathType` field controls how the path matching is done. There are three possible values:

- **Exact:** Matches the URL path exactly as specified.
- **Prefix:** Matches based on a URL path prefix.
- **ImplementationSpecific:** Leaves the path matching behavior up to the specific Ingress controller (in your case, the nginx ingress controller).

So, **`ImplementationSpecific` means the path matching behavior is determined by the ingress controller** and is not standardized by Kubernetes itself. For the nginx ingress controller, this typically means it uses its own rules for matching paths, which can support more complex patterns like regex.

---

#### Why did we do `$2` in the rewrite-target annotation?

Your annotation is:

```yaml
nginx.ingress.kubernetes.io/rewrite-target: /$2
```

This is related to the path you defined:

```yaml
- path: /backend(/|$)(.*)
```

This path uses a regular expression pattern (thanks to `ImplementationSpecific` allowing that). Here's what it means:

- `/backend` — The literal prefix.
- `(/|$)` — Matches either a slash `/` or end of the string `$`.
- `(.*)` — Captures the rest of the path (anything after `/backend/`).

The parentheses create **capture groups** in the regex:

- `$1` corresponds to the first group `(/|$)`
- `$2` corresponds to the second group `(.*)`

So, when a request comes in like:

```
/backend/foo/bar
```

- `$1` matches the `/` after `/backend`
- `$2` matches `foo/bar`

The rewrite rule:

```yaml
nginx.ingress.kubernetes.io/rewrite-target: /$2
```

means the request URI will be rewritten to:

```
/foo/bar
```

Effectively, this removes `/backend` from the path before sending it to your backend service.

---

#### Summary:

- `ImplementationSpecific` lets nginx handle the path matching, which can include regex.
- The path `/backend(/|$)(.*)` uses regex groups, where `$2` captures everything after `/backend`.
- The rewrite-target `/ $2` rewrites the URL by stripping `/backend` and forwarding the rest to your service.

---

If you want, I can also explain how this affects routing or other ingress annotations!
