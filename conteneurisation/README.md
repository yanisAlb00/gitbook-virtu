# Conteneurisation

{% file src="../.gitbook/assets/20180604-IN2P3-intro-conteneurs.pdf" %}
[https://indico.in2p3.fr/event/17124/contributions/61042/attachments/48554/61402/20180604-IN2P3-intro-conteneurs.pdf](https://indico.in2p3.fr/event/17124/contributions/61042/attachments/48554/61402/20180604-IN2P3-intro-conteneurs.pdf)
{% endfile %}

Principe d'un conteneur : isoler l'exécution d'ensembles de processus sur un même hôte

\-> Le noyau et l'accès au matériel reste partagé entre les conteneurs (contrairement aux VM)

Un conteneur Linux est :&#x20;

* Un RootFS
* Une configuration Cgroups
* Un ensemble de namespaces
* Un ensemble de capabilities

## Rootfs : jeu de bibliothèques séparées de l'hôte

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## Cgroups

Les Control groups (Cgroups) permettent la limitation des ressources, la compatibilité, le contrôle et la priorisation.

Disponibles depuis la version Kernel 2.6.24

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

## Namespaces

Les Namespaces permettent de compartimenter les ressources du noyau et de ne montrer que ce que l'on souhaite aux processus de notre choix.

Depuis la version 5.6 du noyau, il existe 8 Namespaces différents :

* MNT (Mount) : Permet à chaque conteneur de posséder de son propre espace de nom avec une racine réécrite. Tous les conteneurs ont une vision unique et partagée du système de fichiers.
* PID (Process ID) : Un nouvel espace de nom PID permet de créer des Processus ID avec des valeurs décorrélées des PID de l'espace parent en recommençant à partir de 1. Les processus ID du namespace enfant ne peuvent pas voir les PID de l'espace parent tandis que les PID de l'espace parent verront les identifiants correspondant à ceux de l'espace parent.
* NET (Network) : Le namespace NET virtualise une pile réseau  vierge par défaut de toute interface excepté l'interface loopback 127.0.0.1
* UTS (Unix Time-Sharing) : Les processus appartenant à un nouvel espace de nom peuvent se voir attribuer un nom d'hôte et un domaine différent de celui du système hôte
* IPC (Inter-process Communication) : Permet d'empêcher les communications interprocessus System V.
* UID (User ID) : Comme pour les processus, cet espace de nom permet de décorrélé la vision des UID aux processus associés. Un UID 0 (root) dans un namespace peut en fait correspondre à un UID 1000 dans le nampesace parent
* Cgroup : Les Cgroups sont un mécanismes d'isolation des ressources systèmes et le namespace Cgroup permet de cacher aux processus la hiérarchie Cgroup à laquelle ils appartiennent (CPU, I/O, mémoire vive, ...)\


<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>



## Capabilities

Les Capabilities sur Linux sont des appels Kernel privilégiés qui constituent le privilège d'un super-utilisateur.

L'intérêt des Capabilities est de donner à un process/binaire (ou un conteneur) le droit d'appelé certaines opérations privilégiées sans pour autant avoir les droits root sur le système (principe de moindre privilège).

La liste des capabilities associées à un process est définie par un masque de 8 octets.

L'utilitaire `capsh` permet de lister les Capabilities lisiblement pour un être humain.

```
cat /proc/9491/status | grep Cap
CapInh:    0000000000000000
CapPrm:    0000000000003000
CapEff:    0000000000000000
CapBnd:    0000003fffffffff
CapAmb:    0000000000000000

capsh --decode=0000000000003000
0x0000000000003000=cap_net_admin,cap_net_raw
```

La Capabilities `CAP_SYS_ADMIN` est la plus forte dans la mesure où elle permet des s'assigner des Capabilities et donc de toute les obtenir ce qui équivaut à avoir les droits Root.
