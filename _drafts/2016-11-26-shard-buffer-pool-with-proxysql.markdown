


docker-compose up node01
docker-compose scale nodeN=2
docker-compose scale proxysql=2
docker pull quay.io/coreos/etcd
docker run -d --name discovery -p 2379:2379 -p 2380:2380 -v /usr/share/ca-certificates/:/etc/ssl/certs --net host quay.io/coreos/etcd:v2.0.9 -name discovery -initial-advertise-peer-urls http://${IP}:2380,http://${IP}:7001 -listen-peer-urls http://${IP}:2380,http://${IP}:7001 -initial-cluster discovery=http://${IP}:2380,discovery=http://${IP}:7001 -advertise-client-urls http://${IP}:2379,http://${IP}:4001 -listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001

docker run -d --name discovery -p 2379:2379 -p 2380:2380 --net host quay.io/coreos/etcd -name discovery -initial-advertise-peer-urls http://${IP}:2380,http://${IP}:7001 -listen-peer-urls http://${IP}:2380,http://${IP}:7001 -initial-cluster discovery=http://${IP}:2380,discovery=http://${IP}:7001 -advertise-client-urls http://${IP}:2379,http://${IP}:4001 -listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001




https://github.com/percona/proxysql-admin-tool
https://coreos.com/etcd/docs/latest/v2/docker_guide.html



ETCD_HOST=${ETCD_HOST:-10.20.2.4:2379}
docker run -d -v /usr/share/ca-certificates/:/etc/ssl/certs -p 4001:4001 -p 2380:2380 -p 2379:2379 \
 --name etcd quay.io/coreos/etcd \
 -name etcd0 \
 -advertise-client-urls http://${ETCD_HOST}:2379,http://${ETCD_HOST}:4001 \
 -listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
 -initial-advertise-peer-urls http://${ETCD_HOST}:2380 \
 -listen-peer-urls http://0.0.0.0:2380 \
 -initial-cluster-token etcd-cluster-1 \
 -initial-cluster etcd0=http://${ETCD_HOST}:2380 \
 -initial-cluster-state new

