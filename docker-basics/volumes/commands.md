# Commandes de tests exécutées

```bash
docker volume create my_first_volume
docker run -dit --name ub01 -v my_first_volume:/mounted_volume ubuntu
docker run -dit --name ub02 -v my_first_volume:/mounted_volume ubuntu

docker ps
docker exec -it ub01 /bin/bash
ls
cd mounted_volume
touch test.txt
exit

docker exec -it ub02 /bin/bash
ls
cd mounted_volume
echo "Bonjour de "$(hostname)" [ub02]" > test.txt
exit

docker exec -it ub01 /bin/bash
ls
cd mounted_volume
cat test.txt
exit
```

Dans cet exemple :

1) Création du volume et des conteneurs en indiquant où monter le volume dans le conteneur
2) Création d'un fichier dans le dossier où est monté dans le conteneur 01
3) Vérification de l'existance de ce fichier dans le point de montage du conteneur 02 et modification de son contenu
4) Vérification que la modification est bien entérinée sur le fichier, retrouvé dans le conteneur 01

Par cette manipulation on peut illustrer le fonctionnement du volume partagé entre plusieurs conteneurs. Il s'agit ici d'un volume de type volume (par défaut)