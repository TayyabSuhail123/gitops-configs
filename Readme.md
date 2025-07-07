# GitOps Configs with ArgoCD, Vault & External Secrets

This repository demonstrates a full GitOps setup using **ArgoCD**, **HashiCorp Vault**, and **External Secrets Operator (ESO)** to securely deploy applications into Kubernetes.

---

## 🔧 Stack

- **ArgoCD**: GitOps CD tool for Kubernetes
- **Vault**: Secret management tool
- **External Secrets Operator**: Sync secrets from Vault to Kubernetes
- **Minikube**: Local Kubernetes for testing
- **nginx**: Sample deployment for demo purposes

---

## 📁 Repo Structure

```
gitops-configs/
├── apps/
│   └── nginx-dev.yaml
│   ├── dev/
│   │   └── nginx/
│   │       ├── deployment.yaml
│   │       └── external-secret.yaml
│   ├── prod/
│   └── staging/
│  
├── argocd/
│   ├── app-of-apps.yaml
│   └── argocd-values.yaml
├── eso/
│   ├── base/
│   │   ├── secretstore.yaml
│   │   └── vault-credentials-secret.yaml
│   └── overlays/
├── vault/
│   └── policy-templates/
│       └── eso-policy.hcl
└── README.md
```

---

## 🚀 Workflow Summary

1. **App-of-Apps Pattern** is used to bootstrap environments
2. ArgoCD syncs all apps from the `apps/` folder
3. NGINX is deployed in `dev` namespace via ArgoCD
4. Secrets are pulled from Vault using External Secrets
5. Vault is authenticated using AppRole

---

## 🔒 Vault AppRole Secrets

AppRole credentials (`role_id` & `secret_id`) are stored in a **Kubernetes secret** in the `vault` namespace and referenced in ESO `SecretStore`.

> ✅ We avoided committing sensitive values by templating the secret or keeping it locally ignored.

---

## 🔑 External Secrets

The ExternalSecret resource in `apps/dev/nginx` references keys stored in Vault under the path `secret/data/nginx`.

---

## 📌 Notes

- Avoid committing secrets to Git. Use sealed-secrets or CI-side secret injection.
- The `vault-credentials-secret.yaml` is only for local testing.
- Customize the `app-of-apps.yaml` to reflect real envs.

---

## 🧪 Tested On

- Minikube v1.32+
- ArgoCD v2.10+
- Vault OSS 1.14+
- External Secrets v0.9+

---

## ✅ Future Improvements

- Add CI/CD to auto-push image tags
- Setup staging/prod envs
- Use Vault Agent Injector for dynamic secrets
