# Networks

> Gèrent la ou les connexions des conteneurs entre eux ou avec l'extérieur
## Drivers

![Docker Networks](docker_networks.png)

---

![Docker Networks](docker-networks3.png)


## En détail

![Bridge](bridge-docker-network.png)

---

![Macvlan](macvlan-docker-networks.png)

---

![Host](host-docker-networks.png)

---

![Overlay](overlay-docker-networks.png)

---

![None](none-docker-networks.png)

---

# BONUS : Création Dockerfile Net-tools

> Création d'un petit Dockerfile basique pour accueillir et déployer rapidement des outils réseau de base

→ Le conteneur est basé sur Alpine permettant d'étendre un peu plus les fonctionnalités de `busybox` utilisé dans `commands.md`<br/>

> [!NOTE]
> **Astuce**  - Surcharge conteneur
>
> On démarre le conteneur avec `CMD` plutôt qu'`ENTRYPOINT` pour permettre la surcharge au démarrage
>
> Avec `CMD ["sleep", "infinity"]` + `docker run --rm net-tools ping 8.8.8.8` → le conteneur exécute directement `ping X.X.X.X`
> Avec `ENTRYPOINT ["sleep", "infinity"]` + `docker run --rm net-tools ping 8.8.8.8` → exécute `sleep infinity ping 8.8.8.8`
> 
> La surcharge ne fonctionnerait pas correctement, la commande de base serait toujours jouée avant

Les outils réseau installés sont les suivants :  <br/><br/>

`ping`	Tester la connectivité ICMP  <br/>
`traceroute`	Identifier le chemin réseau  <br/>
`nslookup`	Tester la résolution DNS  <br/>
`dig`	Interroger les serveurs DNS  <br/>
`curl`	Tester les connexions HTTP/HTTPS  <br/>
`ip addr`	Afficher les interfaces réseau  <br/>
`ip route`	Afficher la table de routage  <br/>
`nc`	Tester les connexions TCP/UDP  <br/>
