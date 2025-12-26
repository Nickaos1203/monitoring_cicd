## Mission 1 : Comprendre PromQL

### Questions à explorer :
**Quelle est la différence entre rate() et increase() ?**
rate() calcule le taux d'accroissement moyen par seconde de la série temporelle dans le vecteur de plage.
increase() calcule l'incrément de la série temporelle dans le vecteur de plage.

**Comment filtrer des métriques par label ?**
On utilise les accolades {} pour filtrer les métriques en fonction de label :
``
http_requests_total{job="api", status="200"}
``

**Que fait la fonction histogram_quantile() ?**
histogram_quantile() transforme des histogrammes (bucket counters) en percentiles exploitables.

## Mission 2 : Besty Practices Prometheus

### Questions :
**Comment nommer correctement une métrique ?**
Un nom de métriques doit : 
- se conformer au modèle de données pour les caractères valides;
- comporter un préfixe d'application (un seul mot) pertinent au domaine auquel appartient la métrique. Par exemple :
    - prometheus_notifications_total (spécifique au serveur Prometheus), 
    - process_cpu_seconds_total (exporté par de nombreuses bibliothèques clientes), 
    - http_request_duration_seconds (pour toutes les requêtes HTTP);
- avoir une seule unité (c'est-à-dire ne pas mélanger les secondes avec les millisecondes, ni les secondes avec les octets).
- comporter un suffixe décrivant l'unité, au pluriel. Par exemple : 
    - http_request_duration_seconds, node_memory_usage_bytes, 
    - http_requests_total (pour un décompte cumulatif sans unité);
- peut ordonner les composants de son nom de manière à faciliter le regroupement lorsqu’une liste de noms de métriques est triée par ordre lexicographique, à condition que toutes les autres règles soient respectées. Par exemple: 
    - prometheus_tsdb_head_truncations_closed_total
    - prometheus_tsdb_head_truncations_established_total
    - prometheus_tsdb_head_truncations_failed_total
- devrait représenter la même logique mesurée pour toutes les dimensions d'étiquettes (label).

**Quand utiliser des labels vs créer plusieurs métriques ?**
On utilise des labels lorsque l'on souhaite mesurer la même chose mais sous des variantes différentes. A l'inverse, on utilise plusieurs lorsque ce sont des choses différentes.


**Quels sont les dashboards anti-patterns à éviter ?**
- Trop de panels inutiles
- Dashboards sans objectif clair
- Absence d’unités et de seuils

### Quiz :
**❓ Monitoring vs Observabilité : quelle différence ?**
- Monitoring : permet de savoir quand un problème survient (service down, latence élevée, alerte déclenchée).
- Observabilité : permet de comprendre pourquoi le problème survient en analysant les métriques, logs et traces.

**❓ Citez les 3 piliers de l'observabilité**
- Métriques : valeurs numériques agrégées dans le temps (CPU, latence, requêtes/seconde)
- Logs : événements textuels horodatés (erreurs, messages applicatifs)
- Traces : suivi complet d’une requête à travers plusieurs services

**❓ Quel type de métrique pour compter les requêtes HTTP ?**
Un Counter ne fait qu’augmenter et sert à compter des événements comme :
- le nombre total de requêtes HTTP
- le nombre d’erreurs
- le nombre d’actions utilisateur

**❓ Quel type de métrique pour l'utilisation RAM actuelle ?**
Un Gauge représente une valeur instantanée qui peut monter ou descendre, comme :
- l’utilisation mémoire
- l’utilisation CPU
- le nombre de connexions actives

**❓ Que signifie P95 ?**
P95 (95e percentile) signifie que 95 % des requêtes ont un temps de réponse inférieur ou égal à cette valeur, et que 5 % sont plus lentes.

