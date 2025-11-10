# Annexe F — GitOps Détaillé : L'Infrastructure as Code Élevé

> *"GitOps : où Git devient le source de vérité ultime, et les déploiements deviennent des merges requests."*

## La révolution GitOps

GitOps transforme la gestion d'infrastructure : au lieu de clics manuels et scripts ad-hoc, tout passe par Git. C'est l'évolution naturelle de Infrastructure as Code.

### L'analogie du développement logiciel

**Développement traditionnel** :
- Code dans IDE
- Commit vers Git
- CI/CD déploie automatiquement

**GitOps** :
- Infrastructure = code dans Git
- Modifications via pull requests
- Outils synchronisent automatiquement l'état réel avec Git

**Résultat** : Infrastructure aussi versionnable, testable, et collaborative que le code applicatif.

## Les principes fondamentaux

### Principe 1 : Tout est dans Git

**Infrastructure déclarative** :
```yaml
# infrastructure/
├── clusters/
│   ├── staging/
│   │   ├── cluster.yaml
│   │   └── apps/
│   │       ├── frontend/
│   │       └── backend/
│   └── production/
│       ├── cluster.yaml
│       └── apps/
├── policies/
│   ├── rbac/
│   └── network/
└── configs/
    ├── monitoring/
    └── security/
```

**Avantages** :
- Historique complet des changements
- Rollback instantané
- Auditabilité parfaite
- Collaboration équipe

### Principe 2 : État réel = État désiré dans Git

**Synchronisation automatique** :
- Outil surveille les changements Git
- Compare état actuel vs désiré
- Applique les différences automatiquement
- Rapporte les dérives

### Principe 3 : Approbation via Pull Requests

**Workflow Git standard** :
```bash
# 1. Créer une branche
git checkout -b feature/new-app

# 2. Ajouter les manifests
kubectl create deployment my-app --dry-run=client -o yaml > deployment.yaml

# 3. Commit et push
git add .
git commit -m "Add my-app deployment"
git push origin feature/new-app

# 4. Pull request pour review
# 5. Merge = déploiement automatique
```

## Les outils GitOps

### ArgoCD : Le leader du marché

**Architecture** :
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://git.example.com/manifests
    path: apps/my-app
    targetRevision: HEAD
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true      # Supprime les ressources absentes de Git
      selfHeal: true   # Corrige automatiquement les dérives
```

**Fonctionnalités avancées** :
- **Multi-sources** : Plusieurs repos Git
- **Helm integration** : Déploiement de charts
- **Kustomize support** : Personnalisation de manifests
- **RBAC** : Contrôle d'accès granulaire

### Flux : Le puriste GitOps

**Principe** : Tout est YAML, pas d'UI

```yaml
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: GitRepository
metadata:
  name: my-app
  namespace: flux-system
spec:
  interval: 1m
  url: https://git.example.com/manifests
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: my-app
  namespace: flux-system
spec:
  interval: 5m
  path: ./apps/my-app
  prune: true
  sourceRef:
    kind: GitRepository
    name: my-app
```

**Avantages** :
- **Léger** : Moins de composants
- **Déclaratif** : Tout en YAML
- **Extensible** : Via CRDs

### Jenkins X : GitOps pour pipelines

**Approche** : Pipeline as Code + GitOps

```yaml
apiVersion: jenkins.io/v1
kind: SourceRepository
metadata:
  name: my-app
spec:
  org: myorg
  repo: my-app
  provider: github
```

**Génère automatiquement** :
- Pipeline CI/CD
- Environnements preview
- Promotion automatique

## Mise en place d'un environnement GitOps

### Étape 1 : Préparation du repository

**Structure recommandée** :
```
📁 mon-repo-gitops/
├── 📁 clusters/           # Configuration clusters
│   ├── 📁 staging/
│   └── 📁 production/
├── 📁 apps/              # Applications
│   ├── 📁 base/          # Configs communes
│   ├── 📁 staging/       # Overrides staging
│   └── 📁 production/    # Overrides production
├── 📁 infrastructure/    # Infra transverse
│   ├── 📁 cert-manager/
│   ├── 📁 ingress-nginx/
│   └── 📁 monitoring/
├── 📁 policies/          # Politiques
└── 📁 docs/              # Documentation
```

### Étape 2 : Outil de synchronisation

**Installation ArgoCD** :
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

**Accès** :
```bash
# Mot de passe admin
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### Étape 3 : Application racine

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: root-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://git.example.com/gitops
    path: clusters/production  # Chemin vers la config
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

### Étape 4 : Applications enfants

**Pattern app-of-apps** :
```yaml
# clusters/production/apps.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: monitoring
spec:
  source:
    repoURL: https://git.example.com/gitops
    path: infrastructure/monitoring
  destination:
    server: https://kubernetes.default.svc
    namespace: monitoring
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
spec:
  source:
    repoURL: https://git.example.com/gitops
    path: apps/my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: production
```

## Workflows GitOps avancés

### 1. Environnements multiples

**Promotion via Git** :
```bash
# Staging validé → Production
git checkout main
git merge staging-approved
git push origin main  # Déclenche déploiement prod
```

**Avantages** :
- Pas de boutons "deploy to prod"
- Historique des promotions
- Rollback via Git revert

### 2. Preview environments

**Par pull request** :
```yaml
# Génère automatiquement un environnement
# URL: https://pr-123.myapp.com
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: pr-123-my-app
spec:
  source:
    repoURL: https://git.example.com/my-app
    path: .
    targetRevision: refs/pull/123/head
```

### 3. GitOps pour infrastructure

**Cluster as Code** :
```yaml
# Via Cluster API
apiVersion: cluster.x-k8s.io/v1beta1
kind: Cluster
metadata:
  name: my-cluster
spec:
  infrastructureRef:
    kind: AWSCluster
    name: my-cluster
```

## Sécurité GitOps

### RBAC ArgoCD

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: my-project
spec:
  clusterResourceWhitelist:
  - group: '*'
    kind: '*'
  destinations:
  - namespace: production
    server: https://kubernetes.default.svc
  sourceRepos:
  - https://git.example.com/gitops
```

### Audit trails

**Tout changement tracé** :
- Qui a fait quoi
- Quand et pourquoi
- Possibilité de rollback
- Conformité réglementaire

### Secret management

**SOPS avec GitOps** :
```yaml
# Secrets chiffrés dans Git
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  password: ENC[AES256_GCM,data:xyz...]  # Chiffré
```

## Monitoring GitOps

### Métriques ArgoCD

**Health des applications** :
- Sync status (Synced, OutOfSync)
- Health status (Healthy, Degraded)
- Operation state (Running, Failed)

### Alerting

**Prometheus rules** :
```yaml
groups:
- name: gitops
  rules:
  - alert: ArgoAppOutOfSync
    expr: argocd_app_info{sync_status="OutOfSync"} > 0
    for: 5m
    labels:
      severity: warning
```

## Migration vers GitOps

### Stratégie progressive

**Phase 1 : Audit** (1 semaine)
- Cartographier l'infrastructure existante
- Identifier les processus manuels
- Évaluer la maturité équipe

**Phase 2 : Pilote** (2-4 semaines)
- Choisir une application simple
- Setup ArgoCD/Flux
- Former l'équipe

**Phase 3 : Expansion** (1-3 mois)
- Migrer applications par applications
- Automatiser les déploiements
- Implémenter les garde-fous

**Phase 4 : Maturité** (continu)
- Multi-clusters
- GitOps pour infrastructure
- Automatisation complète

### Outils de migration

**ArgoCD Image Updater** :
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
spec:
  source:
    # Met à jour automatiquement les images
    kustomize:
      images:
      - my-app=latest
```

## Bénéfices business

### Vélocité
- Déploiement en minutes, pas en jours
- Tests automatisés des changements
- Rollback instantané

### Fiabilité
- Déploiements consistants
- Erreurs réduites (peer review)
- Auditabilité complète

### Gouvernance
- Conformité automatique
- Traçabilité des changements
- Contrôle d'accès granulaire

### Équipe
- Développeurs autonomes
- Ops focalisé sur valeur
- Culture DevOps renforcée

## Challenges et solutions

### Challenge 1 : Complexité initiale

**Solution** : Commencer petit, former progressivement

### Challenge 2 : Résistance culturelle

**Solution** : Démontrer la valeur, célébrer les victoires

### Challenge 3 : Secrets dans Git

**Solution** : Chiffrement (SOPS, Sealed Secrets)

### Challenge 4 : Multi-équipes

**Solution** : Projects, RBAC, code reviews

## L'avenir de GitOps

### Évolution attendue

**GitOps 2.0** :
- IA pour review automatique
- Multi-cloud natif
- Intégration supply chain
- Observabilité avancée

**Tendances** :
- **Policy as Code** : OPA + Gatekeeper
- **Supply Chain Security** : SLSA, attestations
- **Progressive Delivery** : Flagger, Argo Rollouts

### Positionnement actuel

GitOps n'est plus une tendance — c'est la norme pour les organisations matures.

**Adoption** : 70%+ des entreprises Kubernetes utilisent GitOps (2024)

**Outils** : ArgoCD dominant, Flux en croissance

**Maturité** : De "nice to have" à "table stakes"

## Conclusion

GitOps transforme la gestion d'infrastructure :

- **Avant** : Scripts, clics, documentation manquante
- **Après** : Code, automation, auditabilité parfaite

C'est l'aboutissement logique de DevOps : infrastructure aussi traitable que le code applicatif.

---

*"GitOps ne simplifie pas seulement les déploiements. Il transforme la culture DevOps elle-même."*

---

[Retour aux annexes](./README.md)
