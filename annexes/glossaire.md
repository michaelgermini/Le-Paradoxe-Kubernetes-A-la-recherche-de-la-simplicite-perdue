# Annexe A — Glossaire DevOps 2025

> *"Dans le DevOps comme dans la vie, nommer les choses, c'est commencer à les maîtriser."*

## Concepts fondamentaux

### Conteneur (Container)
Unité logicielle légère et portable qui contient tout le nécessaire pour exécuter une application : code, runtime, dépendances, configurations.

**Analogie** : Une boîte Tupperware® qui contient un repas complet, prêt à être réchauffé n'importe où.

### Orchestration (Orchestration)
Gestion automatique du cycle de vie des conteneurs : déploiement, scaling, networking, health checks, récupération d'erreur.

**Dans la vraie vie** : Un chef d'orchestre qui coordonne musiciens, instruments, et public pour créer une symphonie harmonieuse.

### Cluster
Ensemble de machines (physiques ou virtuelles) travaillant ensemble comme une seule entité pour exécuter des applications conteneurisées.

**Analogie** : Une fourmilière où chaque fourmi a un rôle spécifique, mais l'ensemble fonctionne comme un organisme unique.

## Acronymes essentiels

### K8s
Abréviation de Kubernetes (K + 8 lettres + s = K8s). Le système d'orchestration de conteneurs le plus populaire.

### YAML
Yet Another Markup Language. Format de sérialisation lisible par l'humain utilisé pour les configurations Kubernetes.

**Pourquoi YAML ?** Parce que JSON est trop verbeux, et XML trop complexe. YAML est le compromis parfait entre lisibilité et structure.

### CI/CD
Continuous Integration / Continuous Deployment. Pratiques de développement où le code est automatiquement testé et déployé.

**En français** : Intégration Continue / Déploiement Continu.

### IaC
Infrastructure as Code. Gestion et provisionnement de l'infrastructure via des fichiers de configuration plutôt que des clics manuels.

## Concepts avancés

### Pod
Unité de base de Kubernetes : un ou plusieurs conteneurs qui partagent le même contexte d'exécution (réseau, stockage, IPC).

**Analogie** : Une famille dans une maison. Les conteneurs sont les membres, le pod est la maison.

### Service
Abstraction qui définit un ensemble logique de pods et une politique d'accès stable à ces pods.

**Dans la pratique** : Comment vos applications se trouvent et communiquent entre elles dans le cluster.

### Ingress
Objet Kubernetes qui gère l'accès externe aux services dans un cluster, typiquement HTTP/HTTPS.

**Analogie** : Le portier d'un hôtel qui dirige les visiteurs vers les bonnes chambres.

### Operator
Extension de Kubernetes qui utilise des ressources personnalisées pour gérer des applications complexes.

**Exemple** : Un operator PostgreSQL qui gère automatiquement backups, scaling, et mises à jour.

## Patterns et architectures

### Microservices
Approche architecturale où une application est composée de petits services indépendants communiquant via APIs.

**Avantages** : Scalabilité, maintenabilité, résilience.
**Inconvénients** : Complexité de gestion, debugging distribué.

### Sidecar Pattern
Déploiement d'un conteneur auxiliaire (sidecar) avec le conteneur principal pour ajouter des fonctionnalités transversales.

**Exemple** : Un conteneur sidecar pour la gestion des logs, la sécurité, ou le monitoring.

### Service Mesh
Infrastructure dédiée pour gérer la communication service-to-service de manière fiable et sécurisée.

**Outils populaires** : Istio, Linkerd, Consul.

## État d'esprit DevOps

### Infrastructure as Code (IaC)
Toute infrastructure doit être décrite dans le code pour être versionnée, testée, et reproduite.

### GitOps
Utilisation de Git comme source unique de vérité pour les déploiements et configurations.

### Observability
Capacité à comprendre l'état interne d'un système via ses outputs externes (logs, métriques, traces).

### Chaos Engineering
Pratique consistant à injecter des pannes contrôlées pour tester la résilience des systèmes.

---

*"Un glossaire n'est jamais complet. Il évolue avec la technologie et notre compréhension d'elle."*

---

[Annexe suivante : Les outils cités](./outils-cites.md) | [Retour aux annexes](./README.md)
