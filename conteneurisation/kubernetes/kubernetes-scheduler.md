# Kubernetes Scheduler

## Définitions

Le Scheduler Kubernetes implémente le "**Controller Pattern**"

Controller Pattern : des clients surveillent en permanence l'API Kube et vont mettre à jour des objets afin d'atteindre un état souhaité dans un cluster.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## Exemple d'action réalisée par le Scheduler : Association d'un noeud à un pod

### Etapes

* Le Scheduler vérifie s'il y a des objets pods de l'api kube associé à aucun noeud

```json
nodeName: null
```

* Le Scheduler va chercher un noeud adéquat et effectuer une opération de mise à jour sur l'API Kube pour associé un noeud à ce pod

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

### Fonctionnement

Le Scheduler procède en 3 phases principales :&#x20;

1. **Filtering** : évaluation des noeuds "feasible" pour l'hébergement du pod au travers de plugins qui composent la logique du Scheduler
2. **Scoring** : séléction du meilleur noeud parmis la liste de noeuds "feasible"
3. **Binding** : Mise à jour de l'objet pod dans l'API Kube pour créer le lien entre le pod et le noeud

(Le Scheduler est une coquille vide et toute la logique est implémentée dans des plugins dont le code source est disponible sur github)

### Exemples de plugins utilisés pour le filtrage :&#x20;

**TaintTolerations**&#x20;

Ce plugin utilise 2 objets :&#x20;

* Les Taints sur les noeuds
* Les Tolerations sur les pods

Si un pod n'a pas une Toleration qui match la Taint d'un noeud, le Scheduler ne va pas les associer.

Exemple de TaintToleration : `security=high:NoSchedule`

\-> Les noeuds vont utiliser les TaintTolerations pour repousser des pods

#### NodeAffinities

Des objets de type "affinities" sont appliqués sur des pods et des labels sont appliqués sur les noeuds.

Le label appliqué sur un noeud est un requirement et si un pod ne possède pas le NodeSelector adéquat il ne pourra pas aller sur ce noeud.

Exemple de NodeSelector : `security=high`

\-> Les pods vont utiliser les NodeAffinities pour sélectionner des noeuds

### Domaines de faisabilité

Un domaine de faisabilité regroupe les noeuds qui vont potentiellement pouvoir être associé à un pod :&#x20;

* Ensemble des noeuds qui ne vont pas repousser le pod (TaintTolerations)
* Ensemble des noeuds que peut sélectionner le pod (nodeAffinities)

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
TaintTolerations et NodeAffinities ne sont pas les seuls plugins appliqués dans l'étape de filtering , il est possible que d'autres plugins soient appliqués et la liste des noeuds "feasible" sera un sous-ensemble du domaine de faisabilité.
{% endhint %}

Les domaines de faisabilités permettent d'étudier les mécanismes d'isolation dans un cluster:

* Si 2 pods ont des **domaines de faisabilité disjoints** alors ils sont considérés comme **isolés** car ils ne peuvent pas se retrouver sur le même noeud

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

* Si 2 pods ont des **domaines de faisabilité avec une intersection** non vide alors ils ne sont **pas isolés**

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

### Mauvaise configuration de l'isolation des Pods

Si le filtering se base uniquement sur le plugin TaintToleration, un noeud sur lequel aucune Taint est appliquée pourra alors hébérger un pod qui match la Taint A et un pod qui match la Taint B. Il n'y a donc plus d'isolation.

Si le filtering se base uniquement sur le plugin NodeAffinities, 2 pods où il n'y a pas de nodeSelector pourront se retrouver sur un même noeud et il n'y a plus d'isolation non plus.

La seule façon de bien faire de l'isolation est d'utiliser les 2 mécanismes TaintToleration + NodeAffinities.

On aura alors :

* Une zone A
* Une zone B
* Un domaine avec des noeuds et des pods sur lesquels aucune taint ni nodeSelector n'a été appliqué

\-> Aucun chevauchement = isolation

### Scoring

Les scores sont basés sur les statuts des noeuds.

Les comptes Kubelet ont les droits pour éditer les statuts des noeuds.

On peut donc modifier le score de notre noeud compromis pour attirer des pods puisque le Scheduler fait confiance aux status édités par le Kubelet.

### Binding

Le Binding se base sur le composant Node authorizer.

Ce composant construit un graph qui lui permet de valider l'association d'un noeud et d'un compte de service à un pod.

Les arêtes du graph sont construits à partir des attributs nodeName et AccountName dans les spécifications du pod.

Si le chemin existe dans le graph, la création du compte est possible :

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Si le chemin n'existe pas, création impossible :

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
