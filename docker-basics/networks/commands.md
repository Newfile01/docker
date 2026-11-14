```bash
#### 🌉 BRIDGE
# Création Network
docker network create --driver bridge --subnet 192.168.0.0/24 my-bridge-01
docker network ls


# 🎛️ Création 2 conteneurs & connexion au réseau
docker run -it --name ub-01 --network my-bridge-01 -d busybox
docker run -it --name ub-02 --network my-bridge-01 -d busybox
# Busybox contient les utilitaires réseau de base

# Vérif état conteneurs
docker ps
docker inspect ub-01

# Tester le réseau depuis ub-01
docker exec -it ub-01 /bin/sh
# ↓↓ Dans conteneur ↓↓
ping ub-02

# Tester le réseau depuis ub-02
docker exec -it ub-02 /bin/sh
# ↓↓ Dans conteneur ↓↓
ping ub-01

### 🐳 AVEC DOCKERFILE
docker -t net-tools-light:v1 .

docker run --rm --network my-bridge-01 --name net net-tools-light:v1 ping ub-01
docker run --rm --network my-bridge-01 --name net net-tools-light:v1 ping ub-02


#### 💻 HOST
docker run -it --rm --network host --name ub-host busybox sh

# ↓↓ Dans conteneur ↓↓
# Interfaces réseau
ip addr

# Routes
ip route

# Adresse IP
hostname -i

# Connectivité
ping -c 3 8.8.8.8

# Effectuer les mêmes commandes sur l'hôte
# → même résultats


#### 🚫 NONE
docker run -it --rm --network none --name ub-none busybox sh

# Effectuer les mêmes commandes que pour HOST
# → loopback seule
# → pas de route
# → pas de nslookup
# → pas de ping vers l'extérieur


#### 📲↔📲 MACVLAN
# 1) récupérer les infos de l'hôte
ip link
# ...
# eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
#     link/ether 00:15:5d:9c:27:65 brd ff:ff:ff:ff:ff:ff
#     altname enx00155d9c2765
# ...

docker network create \
  --driver macvlan \
  --subnet 192.168.1.0/24 \
  --gateway 192.168.1.1 \
  --ip-range 192.168.1.224/28 \
  -o parent=eth0 \
  macvlan-lab

docker run -dit \
  --name ub-mac-01 \
  --network macvlan-lab \
  --ip 192.168.1.225 \
  busybox

docker run -dit \
  --name ub-mac-02 \
  --network macvlan-lab \
  --ip 192.168.1.226 \
  busybox

docker exec -it ub-mac-01 sh

ip addr
ip route
ping -c 3 192.168.1.226
ping -c 3 192.168.1.1
```