---
description: >-
  https://www.sstic.org/2024/presentation/getting_ahead_of_the_schedule_manipulating_the_k8s_scheduler_to_perform_lateral_moves_in_a_cluster
  /
---

# Kubernetes

## Généralités

**Nœud Kubernetes** = Machine qui héberge un process "**kubelet**"

Le process "kubelet" se connecte à l'**api-server** pour récupérer la liste des pods qu'il doit exécuter

**Pod** = un ensmeble de conteneurs

Kubelet transmet les informations des conteneurs à exécuter au docker runtime de son nœud :

* Docker runtime
* Containerd
* Podman

Les Pods sont associés à des identités "**service-account**"

\
![](<../../.gitbook/assets/image (5).png>)\


## Exemple d'attaque : Container Escape

En cas de mauvaise configuration d'un pod, il est possible de s'échapper d'un pod pour compromettre le noeud hôte.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Une fois le Container Escape réalisé, il est également possible de compromettre d'autres service-account utilisés par d'autres pod.\
\
Ces service-account peuvent permettre de donner des accès à des ressources Cloud ou à l'api-server (possibilité d'extraire des secrets) voire même de devenir admin du cluster.\


<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
