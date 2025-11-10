# Annexe C — Exemples de YAML poétiques et pédagogiques

> *"Le YAML n'est pas que du code. C'est un langage poétique qui décrit l'âme de vos applications."*

## Introduction à la poésie YAML

Le YAML de Kubernetes est souvent vu comme verbeux et complexe. Mais derrière chaque manifest se cache une histoire, une intention, une architecture. Voici comment transformer le technique en poétique.

### Principes de la poésie YAML

1. **Lisibilité** : Chaque ligne raconte une histoire
2. **Structure** : L'indentation révèle l'architecture
3. **Commentaires** : Explications du pourquoi, pas du comment
4. **Naming** : Noms qui évoquent le rôle, pas la technique

## Exemple 1 : Le pod comme poème d'amour

### Version technique (banale)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-server
spec:
  containers:
  - name: nginx
    image: nginx:1.21
    ports:
    - containerPort: 80
```

### Version poétique (évocatrice)
```yaml
# 🌟 L'essence d'un serveur web : accueillir et servir
apiVersion: v1
kind: Pod
metadata:
  name: digital-host  # Celui qui accueille les visiteurs du web
  labels:
    purpose: welcome   # Sa raison d'être : accueillir
    nature: stateless  # Sans mémoire des passages précédents
spec:
  # Le cœur du pod : son conteneur unique
  containers:
  - name: nginx-gateway  # La porte d'entrée du domaine numérique
    image: nginx:1.21   # Le gardien éternel des pages web
    ports:
    - containerPort: 80  # Le port sacré du HTTP
      name: http-portal  # L'entrée officielle du royaume web
    resources:
      requests:
        memory: "64Mi"   # Souvenir minimum pour fonctionner
        cpu: "100m"      # Un cœur pour battre
      limits:
        memory: "128Mi"  # Limite pour ne pas envahir
        cpu: "200m"      # Pouvoir sans excès
    # Gardien de la santé
    livenessProbe:
      httpGet:
        path: /health    # Le pouls du serveur
        port: 80
      initialDelaySeconds: 30  # Temps pour s'éveiller
      periodSeconds: 10        # Rythme cardiaque
```

## Exemple 2 : Le deployment comme symphonie orchestrale

### Version technique
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      labels:
        app: api
    spec:
      containers:
      - name: api
        image: myapi:v1.2
        ports:
        - containerPort: 3000
```

### Version poétique : La symphonie des APIs
```yaml
# 🎼 La symphonie des APIs : harmonie de trois voix identiques
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-symphony      # L'œuvre musicale des services
  namespace: production   # La grande salle de concert
spec:
  # Le nombre sacré : trois pour la résilience
  replicas: 3
  # Le chef d'orchestre qui reconnaît ses musiciens
  selector:
    matchLabels:
      symphony: api       # Partie de la même œuvre
      movement: backend   # Mouvement orchestral
  template:
    metadata:
      labels:
        symphony: api     # Identité musicale
        movement: backend
        version: v1.2     # Numéro d'opus
    spec:
      # Les trois musiciens identiques
      containers:
      - name: api-virtuoso  # Le virtuose de l'API
        image: myapi:v1.2   # La partition gravée
        ports:
        - containerPort: 3000   # La fréquence harmonique
          name: api-melody     # La mélodie des données
        env:
        - name: LOG_LEVEL
          value: "INFO"        # Volume de l'orchestre
        # Le métronome interne
        readinessProbe:
          httpGet:
            path: /ready      # Prêt à jouer ?
            port: 3000
          initialDelaySeconds: 10  # Temps d'accordage
          periodSeconds: 5         # Rythme de vérification
        # Le chef veille sur ses musiciens
        livenessProbe:
          httpGet:
            path: /health     # Le pouls musical
            port: 3000
          initialDelaySeconds: 30  # Échauffement terminé
          periodSeconds: 30        # Vérification périodique
```

## Exemple 3 : Le service comme réseau social

### Version technique
```yaml
apiVersion: v1
kind: Service
metadata:
  name: api-service
spec:
  selector:
    app: api
  ports:
  - port: 80
    targetPort: 3000
  type: ClusterIP
```

### Version poétique : Le club exclusif des APIs
```yaml
# 👥 Le club exclusif : où les clients rencontrent les APIs
apiVersion: v1
kind: Service
metadata:
  name: api-gathering     # Le rassemblement des APIs
  labels:
    visibility: internal  # Club privé, membres seulement
spec:
  # Qui peut rejoindre ce club ?
  selector:
    role: api-servant     # Les serviteurs des données
    status: ready         # Prêts à accueillir
  # Les portes d'entrée du club
  ports:
  - name: http-entrance   # L'entrée principale
    port: 80              # Le port officiel du club
    targetPort: 3000      # Où trouver les membres à l'intérieur
    protocol: TCP         # Langage parlé au club
  # Règles du club
  type: ClusterIP         # Club privé du cluster
  sessionAffinity: None   # Pas de favoritisme, tous égaux
```

## Exemple 4 : Le configmap comme livre de recettes

### Version technique
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_URL: "postgres://..."
  REDIS_URL: "redis://..."
```

### Version poétique : Le grimoire des secrets de famille
```yaml
# 📖 Le grimoire ancestral : recettes transmises de génération en génération
apiVersion: v1
kind: ConfigMap
metadata:
  name: family-recipes     # Les recettes de la famille applicative
  labels:
    category: secrets      # Savoir sacré, ne pas partager
    inheritance: immutable # Tradition immuable
data:
  # 🔮 Les ingrédients magiques
  DATABASE_URL: "postgres://db:5432/myapp"  # L'autel des données
  REDIS_URL: "redis://cache:6379"           # Le cristal mémoriel
  API_SECRET: "s3cr3t_k3y"                  # La clé du royaume
  LOG_LEVEL: "INFO"                         # Le volume de la narration
  FEATURE_FLAGS: "new_ui=true,beta_feature=false"  # Les sorts expérimentaux
```

## Exemple 5 : Le job comme quête épique

### Version technique
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: data-migration
spec:
  template:
    spec:
      containers:
      - name: migrate
        image: migrate:v1
      restartPolicy: Never
```

### Version poétique : La quête du data-mage
```yaml
# ⚔️ La quête sacrée : migrer les anciens rouleaux vers le nouveau temple
apiVersion: batch/v1
kind: Job
metadata:
  name: data-pilgrimage   # Le pèlerinage des données anciennes
  labels:
    quest: migration      # Nature de la quête
    urgency: high         # Importance sacrée
    reward: enlightenment # Prix : illumination des données
spec:
  # Une seule tentative sacrée
  backoffLimit: 1         # Pas de réessai, destin unique
  template:
    metadata:
      labels:
        role: data-mage   # Le mage des données
        task: pilgrimage  # La tâche sacrée
    spec:
      # Le mage solitaire
      containers:
      - name: migration-wizard  # Le sorcier des migrations
        image: migrate:v1       # Le bâton magique gravé
        command: ["migrate", "up"]  # L'incantation suprême
        env:
        - name: DB_URL
          value: "postgres://old:5432/legacy"  # L'ancien temple
        - name: NEW_DB_URL
          value: "postgres://new:5432/modern"  # Le nouveau sanctuaire
        resources:
          requests:
            memory: "256Mi"    # Mémoire pour les sorts
            cpu: "500m"        # Pouvoir magique
      # Destin scellé : réussite ou échec définitif
      restartPolicy: Never      # Pas de résurrection
```

## Principes pour écrire du YAML poétique

### 1. Commentaires comme métaphores
- Au lieu de `# Database configuration`
- Préférez `# Le cristal qui garde les souvenirs anciens`

### 2. Noms évocateurs
- Au lieu de `app-config`
- Préférez `family-recipes` ou `ancestral-wisdom`

### 3. Valeurs avec contexte
- Au lieu de `port: 80`
- Préférez `port: 80  # Le port sacré du HTTP`

### 4. Structure narrative
Organisez le YAML comme un récit :
- Introduction (metadata)
- Développement (spec)
- Résolution (status implicite)

### 5. Cohérence thématique
Un cluster entier peut partager une métaphore :
- **Musicale** : symphonie, concerto, mouvement
- **Architecturale** : temple, sanctuaire, portail
- **Naturelle** : forêt, rivière, montagne
- **Sociale** : club, famille, communauté

## Avantages pédagogiques

### Pour l'apprentissage
- Le YAML devient mémorable
- Les concepts s'ancrent dans l'imagination
- La complexité semble naturelle

### Pour la maintenance
- Les intentions sont claires
- Les modifications sont intuitives
- La documentation est intégrée

### Pour l'équipe
- Culture commune autour des métaphores
- Communication facilitée
- Fun dans le travail technique

## Mise en garde poétique

*"Le YAML poétique est un art, pas une obligation. Utilisez-le pour éclairer, pas pour obscurcir. La beauté doit servir la fonction, pas la remplacer."*

---

*"Dans le YAML poétique, nous ne codons pas des applications. Nous composons des symphonies numériques."*

---

[Annexe suivante : Ressources pour aller plus loin](./ressources.md) | [Annexe précédente : Outils cités](./outils-cites.md)
