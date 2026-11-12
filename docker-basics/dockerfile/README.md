# Dockerfile coding

→ Un `Dockerfile` contient les différentes couches d'une image sur laquelle s'appuiera le **Docker Engine** pour créer des conteneurs.
Le nom est important pour permettre au moteur de conteneur de retrouver le fichier dans l'arborescence.
→ Une image Docker poura être optimisé grâce au **multi-layer builds** qui repart de la précédente couche pour construire la suivante. Ceci permet notamment de construire une application dans un langage donné, la tester puis la compiler, ne conserver que les binaires/artefacts finaux et enfin l'exposer d'une certaine manière.

🔗Documentation : [Doc Dockerfile](https://docs.docker.com/reference/dockerfile/)