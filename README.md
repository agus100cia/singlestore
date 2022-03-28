# Singlestore
## Instalación

La instalación de Singlestore en cualquier ambiente (bare-meta o cloud) se lo puede hacer por medio de management tools. En esta guia, vamos a desplegar SingleStore cluster en maquinas fisicas que conforman parte de un cluster y que son monitoreadas.

Lo recomendado es tener un clúster de de al menos 4 nodos con propiedades distribuidas con alta disponibilidad.

### Requisitos

- 4 nodos, cada nodo con al menos 4 vCPU y 8 Gb en RAM y un máximo de 8 vCPU con 32Gb en RAM
- SO RHEL/CentOS 6+, Debian 8+ de 64 bits con kernel 3.10+
- Puertos libres en 3306, 8080, 443, 22
- Un usuario root en todos los nodos
- Acceso ssh a todos los nodos
- No duplicar HOSTS
- Todos los hosts deben usar distintas claves SSH

### SingleStore Tools

SingleStore DB Studio, debe ser instalado en un host que reciba el trafico de la red local y no este expuesto a internet.

Descargar e instalar SingleStore Tools en uno de los hosts, este host sera designado como el prinicipal.

Descargar:

- singlestore-client
- singlestoredb-toolbox
- singlestoredb-studio
- singlestoredb-server

```sh
curl https://release.memsql.com/production/index/<singlestore-file>/latest.json

mkdir -p /home/admin/singlestore/memsqlclient/
mkdir -p /home/admin/singlestore/memsqltoolbox/
mkdir -p /home/admin/singlestore/memsqlstudio/
mkdir -p /home/admin/singlestore/singlestoredbserver/

curl https://release.memsql.com/production/index/memsqlclient/latest.json >> /home/admin/singlestore/memsqlclient/latest.json
curl https://release.memsql.com/production/index/memsqltoolbox/latest.json >> /home/admin/singlestore/memsqltoolbox/latest.json
curl https://release.memsql.com/production/index/memsqlstudio/latest.json >> /home/admin/singlestore/memsqlstudio/latest.json
curl https://release.memsql.com/production/index/singlestoredbserver/latest.json >> /home/admin/singlestore/singlestoredbserver/latest.json

```

Archivos Tarball, los archivos se completan con los json anteriores

```ssh
wget https://release.memsql.com/production/tar/x86_64/<singlestore-file>-<version>-<commit-hash>.x86_64.tar.gz

wget https://release.memsql.com/production/tar/x86_64/singlestoredb-toolbox-1.13.6-289f33fe76.x86_64.tar.gz
wget https://release.memsql.com/production/tar/x86_64/singlestoredb-studio-4.0.4-9aeb214606.x86_64.tar.gz
wget https://release.memsql.com/production/tar/x86_64/singlestore-client-1.0.6-c3803db03b.x86_64.tar.gz
wget https://release.memsql.com/production/tar/x86_64/singlestoredb-server-7.6.13-39da2f5c72.x86_64.tar.gz

RPM 

wget https://release.memsql.com/production/tar/x86_64/singlestore-client-1.0.6-c3803db03b.x86_64.rpm
wget https://release.memsql.com/production/tar/x86_64/singlestoredb-toolbox-1.13.6-289f33fe76.x86_64.rpm
wget https://release.memsql.com/production/tar/x86_64/singlestoredb-studio-4.0.4-9aeb214606.x86_64.rpm
wget https://release.memsql.com/production/tar/x86_64/singlestoredb-server7.6.13-39da2f5c72.x86_64.rpm

```

Instalación:

```sh
sh singlestore-client-1.0.6-c3803db03b.x86_64.rpm
sh singlestoredb-toolbox-1.13.6-289f33fe76.x86_64.rpm
sh singlestoredb-studio-4.0.4-9aeb214606.x86_64.rpm


``` 
 
 Estos archivos ubicalos en /home/admin/singlestore o /opt/singlestore
 
 Desempaqueta los archivos. No es necesario desempaquetar el singlestoredb-server ya que se lo utiliza luego.
 
 ```sh
tar xzvf singlestore-client-1.0.6-c3803db03b.x86_64.tar.gz
tar xzvf singlestoredb-toolbox-1.13.6-289f33fe76.x86_64.tar.gz 
tar xzvf singlestoredb-studio-4.0.4-9aeb214606.x86_64.tar.gz
 
 ``` 


### Despliegue

El usuario que despliega SingleStoreDB mediante SingleStore DB Toolbox debe poder acceder a todos los nodos via SSH.

Cuando usted instalar singlestoredb-server via RPM o Debian pack, entonces se crea un grupo y usuario llamado memsql en cada nodo.

El usuario memsql no tiene shell

La creación manual de un usuario memsql y un grupo solo se recomienda en un entorno sin sudo cuando se realiza una implementación basada en tarball de SingleStore DB. Para ejecutar los comandos de SingleStore DB Toolbox en un clúster, este usuario memsql creado manualmente debe configurarse para que pueda conectarse mediante SSH a cada host del clúster.

SingleStore ha sido diseñado para implementarse al menos con 2 nodos.

En el nodo maestro, ejecute la interfaz de usuario

```ssh


``` 

