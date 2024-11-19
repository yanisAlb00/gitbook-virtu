# Cheet Sheat Kube

**Vérifier le compte qu'on utilise sur le noeud :**

```
kubectl auth whoami
```

**Récupérer la liste des pods qui s'exécutent sur le cluster :**

```
Kubectl get pods -o wide -A
```

**Vérifier les configurations d'un pod :**

```
kubectl get pods -o yaml %name_pod% | less
```

\-> On peut voir si le pod est contrôlé par un Replicaset et également voir quels sont les labels utilisés par le Replicaset

\-> On peut voir également si le pod est associé à un nodeSelector ou une Toleration (mécanismes d'isolation)

**Créer un token associé à un compte**

```
kubectl create token admin \
--bound-object-kind Pod --bound-object-name aa --bound-object-id 123
```

\-> Si le nodeAuthorizer ne trouve pas de chemin entre notre noeud et le compte de service alors la création de token est refusée

