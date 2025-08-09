# OAI for Rayhunter

- https://github.com/OPENAIRINTERFACE/openairinterface5g/blob/develop/radio/BLADERF/README
- https://github.com/OPENAIRINTERFACE/openairinterface5g
- https://github.com/OPENAIRINTERFACE/openairinterface5g/blob/develop/doc/BUILD.md


## Build

build error logger.cmake


>
> I also had same problem. but I finally build this with develop tag.
>
> How I built it:
>
> Delete cache of openair-spgwu-tiny with using command $docker builder prune
> Clone the repository https://github.com/OPENAIRINTERFACE/openair-spgwu-tiny
> Move to this repository and build it with using this command $ docker buld --target oai-spgwu-tiny --tag oai-spgwu-tiny:develop --file docker/Dockerfile.ubuntu --build-arg EURECOM_PROX="http://proxy.eurecom.fr:8080" .
>
> In this way, you can also build develop version.
>
https://github.com/OPENAIRINTERFACE/openair-epc-fed/issues/36

### HSS

add apt sudo pkg

rm clone for oai asn1c

```
  155  git clone https://github.com/OPENAIRINTERFACE/openair-epc-fed.git opeair-epc-fed-dev
  156  mv opeair-epc-fed-dev openair-epc-fed-dev
  157  cd openair-epc-fed-dev
  158  git fetch --prune
  159  git checkout master
  160  git rebase origin/master
  161  ./scripts/syncComponents.sh --hss-branch develop \\n                              --spgwc-branch develop --spgwu-tiny-branch develop
  162  git submodule deinit --all --force
  163  git submodule init
  164  git submodule update
  165  docker build --target oai-hss --tag oai-hss:production \\n               --file component/oai-hss/docker/Dockerfile.ubuntu18.04 component/oai-hss
  166  git rebase origin/master
  167  git checkout -f v1.2.0
  168  \n
  169  ./scripts/syncComponents.sh
  170  ./scripts/syncComponents.sh --hss-branch develop \\n                              --spgwc-branch develop --spgwu-tiny-branch develop
  171  docker build --target oai-hss --tag oai-hss:production \\n               --file component/oai-hss/docker/Dockerfile.ubuntu18.04 component/oai-hss
```

### SPGW-C

add apt sudo pkg


rm clone for oai asn1c


```
docker build --target oai-spgwc --tag oai-spgwc:production \
               --file component/oai-spgwc/docker/Dockerfile.ubuntu18.04 component/oai-spgwc
```



### SPGW-U

have to replace with upsteam dev

```
   87  cd /opt/gits
   88  git clone https://github.com/OPENAIRINTERFACE/openair-spgwu-tiny
   89  cd openair-spgwu-tiny
   90  docker buld --target oai-spgwu-tiny --tag oai-spgwu-tiny:develop --file docker/Dockerfile.ubuntu .
   91  docker build --target oai-spgwu-tiny --tag oai-spgwu-tiny:develop --file docker/Dockerfile.ubuntu .
```


### magma

had to tweak liblfds 


## Deploy

```
cd openair-epc-fed
cd docker-compose
cd magma-mme-demo
docker compose up -d db_init
docker network ls
docker network inspect demo-oai-public-net
docker network inspect demo-oai-private-net
docker network ls
docker network inspect bridge
ip a
ip route add 192.168.61.128/26 via 172.20.5.5 dev wlan0
ip r
docker compose up -d db_init
docker logs demo-db-init --follow
docker rm -f demo-db-init
docker logs demo-cassandra
docker compose up -d oai_spgwu
docker images
vi docker-compose.yml
docker compose up -d oai_spgwu
vi docker-compose.yml
docker compose up -d oai_spgwu
docker ps -a
```
