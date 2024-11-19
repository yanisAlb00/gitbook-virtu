# Hyperviseurs

## Hyperviseurs de type 1

L'hyperviseur est directement installé sur la couche matérielle du serveur.

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption><p><a href="https://www.it-connect.fr/les-types-dhyperviseurs/">https://www.it-connect.fr/les-types-dhyperviseurs/</a></p></figcaption></figure>

\-> Utilisé dans un environnement de production pour la mise en place de machines virtuelles

## Hyperviseurs de type 2

L'hyperviseur est installé et exécuté sur un OS déjà en place

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption><p><a href="https://www.it-connect.fr/les-types-dhyperviseurs/">https://www.it-connect.fr/les-types-dhyperviseurs/</a></p></figcaption></figure>

\-> Utilisé dans un environnement de test sur une même machine hôte

Perte de performance car l'hyperviseur est obligé de passer par l'OS racine avant d'interagir avec le matériel.

Intéressant pour gérer plusieurs hyperviseurs sur une même machine mais attention aux confilts à gérer.
