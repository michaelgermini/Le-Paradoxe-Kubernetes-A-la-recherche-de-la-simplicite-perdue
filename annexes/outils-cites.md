# Annexe B — Les grands outils cités

> *"Les outils ne sont que des moyens. Leur choix révèle notre philosophie DevOps."*

## Classification des outils

### Par approche DevOps

#### Traditionnel : Infrastructure as Code
- **Terraform** : Provisionnement d'infrastructure multi-cloud
- **Ansible** : Configuration management
- **Packer** : Création d'images machines

#### Conteneurisé : Orchestration
- **Docker** : Conteneurisation d'applications
- **Podman** : Alternative daemon-less à Docker
- **Buildah** : Construction d'images OCI

#### Cloud-native : Orchestration avancée
- **Kubernetes** : Orchestrateur de conteneurs de référence
- **Nomad** : Orchestrateur workload-agnostic
- **Docker Swarm** : Orchestrateur intégré à Docker

#### PaaS/Serverless : Abstraction maximale
- **Render** : PaaS moderne avec déploiement git
- **Fly.io** : Déploiement global automatisé
- **Railway** : PaaS pour développeurs
- **Vercel** : Serverless pour le frontend

### Par fonction

#### Développement
- **Skaffold** : Développement local Kubernetes
- **Tilt** : Développement multi-services
- **DevSpace** : Environnements de développement cloud

#### Déploiement
- **ArgoCD** : GitOps pour Kubernetes
- **Flux** : GitOps continu
- **Jenkins X** : CI/CD natif cloud

#### Monitoring & Observabilité
- **Prometheus** : Métriques et alerting
- **Grafana** : Visualisation de données
- **ELK Stack** : Logs centralisés (Elasticsearch, Logstash, Kibana)
- **Jaeger** : Tracing distribué

#### Sécurité
- **Trivy** : Scan de vulnérabilités
- **Falco** : Runtime security monitoring
- **OPA/Gatekeeper** : Policy as Code

## Outils de gestion Kubernetes

### Interfaces graphiques
- **Lens** : IDE pour Kubernetes (meilleur choix)
- **Octant** : Dashboard officiel Kubernetes
- **K9s** : CLI avec interface ncurses (terminal)
- **Portainer** : Gestion multi-clusters

### Outils CLI
- **kubectl** : Interface officielle
- **kubectx/kubens** : Changement rapide de contexte/namespace
- **kube-ps1** : Prompt shell avec info cluster
- **stern** : Log tailing multi-pods

### Gestion de manifests
- **Helm** : Package manager pour Kubernetes
- **Kustomize** : Personnalisation de manifests YAML
- **Jsonnet** : Génération programmatique de configs

## Alternatives populaires et leurs cas d'usage

### Pour les petits projets (1-5 services)
```
Préférence : Docker Compose + Portainer
Pourquoi : Simple, rapide, suffisant
Migration future : Facile vers Kubernetes
```

### Pour les équipes moyennes (5-20 services)
```
Préférence : Kubernetes (K3s) + Lens + ArgoCD
Pourquoi : Puissance sans complexité excessive
Migration : Déjà sur Kubernetes
```

### Pour les grandes entreprises (20+ services)
```
Préférence : Kubernetes + opérateurs + service mesh
Pourquoi : Écosystème mature, gouvernance
Migration : Plateforme établie
```

### Pour les startups en croissance rapide
```
Préférence : Render/Fly.io → Kubernetes
Pourquoi : Démarrage rapide, évolution contrôlée
Migration : Quand la scalabilité devient critique
```

## Évaluation des alternatives

### Critères d'évaluation

#### 1. Courbe d'apprentissage
- **Faible** : Render, Railway, Docker Compose
- **Moyenne** : Nomad, Docker Swarm
- **Élevée** : Kubernetes, Istio

#### 2. Puissance et flexibilité
- **Limitée** : PaaS comme Render
- **Moyenne** : Nomad, Swarm
- **Illimitée** : Kubernetes

#### 3. Maturité de l'écosystème
- **Émergent** : Nouvelles plateformes
- **Mature** : Kubernetes, Docker
- **Dominant** : AWS, GCP, Azure

#### 4. Vendor lock-in
- **Élevé** : PaaS propriétaires
- **Moyen** : Services managés cloud
- **Faible** : Kubernetes open-source

### Matrice de décision

| Cas d'usage | Petit projet | Équipe tech | Enterprise | Startup scale |
|-------------|-------------|-------------|------------|---------------|
| **Débutant** | Docker Compose | Nomad | Kubernetes managé | Render |
| **Intermédiaire** | Nomad | Kubernetes | Kubernetes + opérateurs | Fly.io → K8s |
| **Expert** | Kubernetes | Kubernetes | Kubernetes + mesh | Kubernetes |

## Outils complémentaires essentiels

### Pour le développement
- **kind** : Cluster Kubernetes local
- **minikube** : Alternative à kind
- **k3d** : Kubernetes dans Docker

### Pour le debugging
- **kubectl-debug** : Debug pods en cours d'exécution
- **kube-capacity** : Analyse d'utilisation ressources
- **kubectl-tree** : Visualisation des dépendances

### Pour la sécurité
- **kube-bench** : Audit de sécurité CIS
- **checkov** : Scan de configs Infrastructure as Code
- **kyverno** : Policy engine Kubernetes

## Évolution recommandée

### Parcours d'apprentissage suggéré
1. **Local** : Docker Compose + Portainer
2. **Petit cluster** : K3s + Lens
3. **Production** : Kubernetes + monitoring complet
4. **Échelle** : Multi-cluster + GitOps

### Signes qu'il faut évoluer
- Déploiement manuel devient pénible
- Scaling manuel cause des outages
- Debugging distribué prend trop de temps
- Équipe passe plus de temps sur l'infra que sur le code

## Philosophie de choix d'outils

*"Choisissez l'outil qui fait le job avec le moins de friction cognitive, tout en permettant la croissance future."*

- **Pour apprendre** : Commencez simple, migrez quand nécessaire
- **Pour produire** : Privilégiez la stabilité sur la nouveauté
- **Pour scaler** : Optez pour les standards ouverts
- **Pour durer** : Choisissez des outils avec communauté active

---

*"Les outils sont comme les langues : certains sont plus faciles à apprendre, d'autres plus puissants à maîtriser."*

---

[Annexe suivante : Exemples YAML poétiques](./yaml-poetique.md) | [Annexe précédente : Glossaire](./glossaire.md)
