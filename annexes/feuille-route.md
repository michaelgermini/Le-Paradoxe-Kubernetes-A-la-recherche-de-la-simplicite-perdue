# Annexe G — Feuille de route d'apprentissage 2025

> *"Apprendre Kubernetes n'est pas une course. C'est un voyage de découverte de soi."*

## Vue d'ensemble de l'apprentissage

Kubernetes est un écosystème complexe avec une courbe d'apprentissage raide. Cette feuille de route vous guide pas à pas, en fonction de votre profil et objectifs.

### Les 4 profils d'apprenants

#### 1. Le Débutant Absolu
**Profil** : Nouveau dans DevOps/conteneurs
**Objectif** : Comprendre les bases
**Durée estimée** : 3-6 mois

#### 2. Le Développeur
**Profil** : Code mais pas ops
**Objectif** : Déployer ses apps
**Durée estimée** : 2-4 mois

#### 3. L'Ops Traditionnel
**Profil** : Serveurs physiques/virtels
**Objectif** : Moderniser ses pratiques
**Durée estimée** : 4-6 mois

#### 4. L'Architecte
**Profil** : Expérience cloud/distribué
**Objectif** : Design de plateformes
**Durée estimée** : 6-12 mois

## Phase 1 : Les Fondamentaux (Semaines 1-4)

### Objectifs
- Comprendre les concepts de base
- Installer et utiliser kubectl
- Déployer une première application

### Ressources recommandées

**Cours gratuits** :
- [Kubernetes Fundamentals](https://kube.academy/courses/kubernetes-fundamentals) - Interatif
- [Katacoda Kubernetes scenarios](https://katacoda.com/courses/kubernetes) - Pratique
- [Kubernetes.io Getting Started](https://kubernetes.io/docs/getting-started/) - Docs

**Livres** :
- "The Kubernetes Book" (Nigel Poulton) - Accessible
- "Kubernetes in Action" (Marko Luksa) - Pratique

### Pratique
```bash
# Installation locale
# Minikube (recommandé débutant)
minikube start
kubectl get nodes

# Premier déploiement
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get services
```

### Checkpoint
- Expliquer pod, service, deployment
- Déployer une app web simple
- Comprendre kubectl basics

## Phase 2 : Conteneurs et Orchestration (Semaines 5-8)

### Objectifs
- Maîtriser Docker et les conteneurs
- Comprendre l'orchestration
- Gérer les ressources

### Ressources recommandées

**Conteneurs** :
- "Docker Deep Dive" (Nigel Poulton)
- [Play with Docker](https://labs.play-with-docker.com/)

**Orchestration** :
- [Kubernetes Bootcamp](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- Labs Katacoda avancés

### Pratique avancée

**Multi-conteneurs** :
```yaml
# Déploiement avec sidecar
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-with-sidecar
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: app
        image: myapp:v1
        ports:
        - containerPort: 8080
      - name: sidecar
        image: nginx
        ports:
        - containerPort: 80
```

**Scaling et resources** :
```bash
# HPA
kubectl autoscale deployment nginx --cpu-percent=70 --min=1 --max=10

# Resource quotas
kubectl create namespace team-a
kubectl apply -f quota.yaml
```

### Checkpoint
- Construire des images multi-stage
- Configurer HPA et resource limits
- Déboguer des pods en échec

## Phase 3 : Production-Ready (Semaines 9-16)

### Objectifs
- Sécurité et conformité
- Observabilité complète
- CI/CD et GitOps

### Ressources recommandées

**Sécurité** :
- [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/)
- "Kubernetes Security" (Liz Rice)

**Observabilité** :
- [Monitoring Kubernetes with Prometheus](https://prometheus.io/docs/guides/kubernetes/)
- ELK stack tutorials

**GitOps** :
- [ArgoCD Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/)
- Flux documentation

### Pratique production

**RBAC et sécurité** :
```yaml
# ServiceAccount avec RBAC
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-sa
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-binding
roleRef:
  kind: Role
  name: app-role
subjects:
- kind: ServiceAccount
  name: app-sa
```

**Monitoring stack** :
```yaml
# Prometheus + Grafana
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: prometheus
spec:
  replicas: 2
  serviceAccountName: prometheus
  serviceMonitorSelector: {}
---
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: app-monitor
spec:
  selector:
    matchLabels:
      app: my-app
  endpoints:
  - port: metrics
```

### Checkpoint
- Cluster sécurisé avec RBAC
- Monitoring complet opérationnel
- Pipeline CI/CD automatisé

## Phase 4 : Expertise et Architecture (Semaines 17-32)

### Objectifs
- Multi-cluster et hybrid cloud
- Service mesh et microservices avancés
- Plateforme design et gouvernance

### Ressources recommandées

**Multi-cluster** :
- [Kubernetes Federation](https://kubernetes.io/docs/concepts/cluster-administration/federation/)
- Crossplane documentation

**Service Mesh** :
- [Istio Service Mesh](https://istio.io/latest/docs/)
- Linkerd tutorials

**Platform Engineering** :
- [Kubernetes Patterns](https://www.redhat.com/en/resources/kubernetes-patterns)
- CNCF landscape

### Pratique avancée

**Service Mesh avec Istio** :
```yaml
# Gateway et VirtualService
apiVersion: networking.istio.io/v1beta1
kind: Gateway
metadata:
  name: app-gateway
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"
---
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: app-routing
spec:
  hosts:
  - "*"
  gateways:
  - app-gateway
  http:
  - match:
    - uri:
        prefix: "/api"
    route:
    - destination:
        host: api-service
```

**Operator pattern** :
```yaml
# CRD personnalisé
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: myapps.example.com
spec:
  group: example.com
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              replicas:
                type: integer
              version:
                type: string
```

### Checkpoint
- Architecture multi-cluster
- Service mesh opérationnel
- Operators personnalisés
- Plateforme self-service

## Parcours spécialisés

### Pour DevOps/SRE
**Focus** : Fiabilité, scaling, incident response
**Ajout** : Chaos engineering, on-call rotation

### Pour Développeurs
**Focus** : Inner loop, local development
**Outils** : Skaffold, Tilt, Telepresence

### Pour Architects
**Focus** : Design patterns, gouvernance
**Ajout** : Platform engineering, policy as code

## Ressources par niveau

### Débutant
- **Cours** : Linux Academy, A Cloud Guru (basique)
- **Pratique** : Katacoda, Play with Kubernetes
- **Communauté** : r/kubernetes, Kubernetes Slack (#kubernetes-novice)

### Intermédiaire
- **Cours** : CKA/CKAD preparation
- **Labs** : Killer.sh, KodeKloud
- **Communauté** : KubeCon, meetups locaux

### Avancé
- **Cours** : CKS, multi-cloud architectures
- **Recherche** : Papers, RFCs Kubernetes
- **Communauté** : SIGs Kubernetes, CNCF

## Chronologie recommandée

### Mois 1-2 : Bases solides
- Concepts fondamentaux
- Installation et configuration
- Premiers déploiements

### Mois 3-4 : Production basics
- Sécurité et networking
- Storage et persistence
- Monitoring de base

### Mois 5-6 : CI/CD et GitOps
- Automatisation des déploiements
- Tests et validation
- Environnements multiples

### Mois 7-12 : Expertise avancée
- Multi-cluster
- Service mesh
- Platform engineering

## Évaluation de progression

### Niveau 1 (Débutant)
- ✅ Expliquer pod/service/deployment
- ✅ kubectl basics
- ✅ Déploiement simple

### Niveau 2 (Intermédiaire)
- ✅ RBAC et sécurité
- ✅ Monitoring/Logging
- ✅ CI/CD pipeline

### Niveau 3 (Avancé)
- ✅ Service mesh
- ✅ Multi-cluster
- ✅ Operators personnalisés

### Niveau 4 (Expert)
- ✅ Platform design
- ✅ Policy as code
- ✅ Contribution communauté

## Erreurs communes à éviter

### Erreur 1 : Sauter les bases
**Problème** : Aller trop vite aux concepts avancés
**Solution** : Maîtriser les fondamentaux d'abord

### Erreur 2 : Théorie sans pratique
**Problème** : Lire beaucoup, pratiquer peu
**Solution** : Labs pratiques quotidiens

### Erreur 3 : Outils sans concepts
**Problème** : Apprendre YAML sans comprendre pourquoi
**Solution** : Théorie d'abord, outils ensuite

### Erreur 4 : Apprendre seul
**Problème** : Pas de feedback, mauvaises habitudes
**Solution** : Communauté, mentors, pair learning

## Motivation et persévérance

### Les 3 phases émotionnelles

**Phase 1 (0-2 mois)** : Excitation
- "C'est incroyable !"
- Motivation naturelle

**Phase 2 (2-6 mois)** : Frustration
- "C'est trop complexe !"
- Risque d'abandon

**Phase 3 (6+ mois)** : Maîtrise
- "Je comprends maintenant !"
- Satisfaction profonde

### Conseils de motivation

**Définir des objectifs clairs** :
- Court terme : "Maîtriser les pods"
- Moyen terme : "Déployer en prod"
- Long terme : "Designer des plateformes"

**Célébrer les victoires** :
- Chaque déploiement réussi
- Chaque problème résolu
- Chaque concept maîtrisé

**Trouver un buddy** :
- Étudier avec un collègue
- Participer à des study groups
- Rejoindre des communautés

## Certification recommandée

### Progression logique
1. **CKA** (Administrator) - Operations
2. **CKAD** (Developer) - Développement
3. **CKS** (Security) - Sécurité
4. **KCNA** (Associate) - Vue d'ensemble

### Préparation
- **Officiel** : CNCF training
- **Pratique** : Killer.sh labs
- **Communauté** : Reddit, Slack

## Conclusion

Apprendre Kubernetes est un investissement majeur, mais rentable :

**Temps** : 6-12 mois pour expertise
**Retour** : Carrière DevOps solide
**Satisfaction** : Maîtrise d'une technologie transformative

Le voyage est difficile, mais la destination en vaut la peine.

---

*"Chaque expert était débutant un jour. Chaque maître était élève. Le chemin fait partie de la récompense."*

---

[Retour aux annexes](./README.md)
