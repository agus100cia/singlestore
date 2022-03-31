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

wget https://release.memsql.com/production/rpm/x86_64/singlestore-client-1.0.6-c3803db03b.x86_64.rpm
wget https://release.memsql.com/production/rpm/x86_64/singlestoredb-toolbox-1.13.6-289f33fe76.x86_64.rpm
wget https://release.memsql.com/production/rpm/x86_64/singlestoredb-studio-4.0.4-9aeb214606.x86_64.rpm
wget https://release.memsql.com/production/rpm/x86_64/singlestoredb-server7.6.13-39da2f5c72.x86_64.rpm

```

Instalación:

```sh
rpm -i singlestore-client-1.0.6-c3803db03b.x86_64.rpm
rpm -i singlestoredb-toolbox-1.13.6-289f33fe76.x86_64.rpm
rpm -i singlestoredb-studio-4.0.4-9aeb214606.x86_64.rpm


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

El usuario memsql no tiene shell, el usuario memsql se crea automaticamente al instalar singlestoredb-studio

La creación manual de un usuario memsql y un grupo solo se recomienda en un entorno sin sudo cuando se realiza una implementación basada en tarball de SingleStore DB. Para ejecutar los comandos de SingleStore DB Toolbox en un clúster, este usuario memsql creado manualmente debe configurarse para que pueda conectarse mediante SSH a cada host del clúster.

SingleStore ha sido diseñado para implementarse al menos con 2 nodos.

En el nodo maestro, ejecute la interfaz de usuario

```ssh
sdb-deploy ui

``` 

Este comando mostrará un enlace con un token seguro que puede usar para implementar SingleStore DB a través de la interfaz de usuario.

### Implementar la base de datos SingleStore.


En este paso debe proporcionar la ruta de singlestoredb-server cuando se le solicite.

### Monitoreo

En su máquina de implementación principal, ejecute el siguiente comando para usar SingleStore DB Studio para monitorear e interactuar con su clúster.

```sh

cd /home/admin/singlestore/memsqlstudio/

nohup ./singlestoredb-studio > studio.stdout 2> studio.stderr < /dev/null &

```

## Instalacion

```sh

sudo yum-config-manager --add-repo https://release.memsql.com/production/rpm/x86_64/repodata/memsql.repo && \
sudo yum install -y singlestore-client singlestoredb-toolbox singlestoredb-studio

``` 

Levantar UI

```sh
sdb-deploy ui
``` 

Licencia:

```sh
BGRhZWI4MzA2OTQ0NDQ0NTg5MDZjNGRhZjQyOTY5ZGU3AAAAAAAAAAAEAAAAAAAAAAwwNAIYKNlup/E65cfL1x+URvxOSr0skBhrQH5RAhhgStdR3wUb0YCOGzcCuCzWsP0NfY25l+UAAA==

``` 

## Instacion

Afinar

https://docs.singlestore.com/db/v7.6/en/reference/configuration-reference/cluster-configuration/system-requirements-and-recommendations.html

```sh

sudo yum -y install ntp
sudo yum -y install numactl

sudo nano /etc/sysctl.conf
vm.swappiness=10
vm.max_map_count=1000000000
vm.min_free_kbytes=658096
net.core.rmem_max=212992
net.core.wmem_max=212992

Para todos los discos en todos los nodos
sudo chown memsql:memsql -R /disk01

``` 

Crear Swap

```sh
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB
ls -lh /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
swapon -s
sudo nano /etc/fstab
/swapfile   swap    swap    sw  0   0
``` 


Instalacion

```sh
sdb-deploy setup-cluster \
	  --version 7.6.7 \
      --master-host azureuser@10.0.0.4:22 \
      --leaf-hosts 10.0.0.5,10.0.0.6 \
      --identity-file /home/azureuser/keysinglestore.pem \
      --license BGRhZWI4MzA2OTQ0NDQ0NTg5MDZjNGRhZjQyOTY5ZGU3AAAAAAAAAAAEAAAAAAAAAAwwNAIYKNlup/E65cfL1x+URvxOSr0skBhrQH5RAhhgStdR3wUb0YCOGzcCuCzWsP0NfY25l+UAAA== \
      --password Ecuador01*
      
y
y
y
y
sdb-admin restart-node --all

 ``` 
 
 Ver hosts
 
 ```sh
 sdb-toolbox-config list-hosts
 ``` 
 
 Eliminar hosts
 ```sh
 sdb-toolbox-config unregister-host --all
 ``` 
 
 En Mysql Workbench => Advanced => useSSL=0
 
 
 ### SingleStore studio
 
 ```sh
 sudo yum-config-manager --add-repo https://release.memsql.com/production/rpm/x86_64/repodata/memsql.repo
 sudo yum repolist
 sudo yum install -y singlestoredb-studio
 sudo systemctl start singlestoredb-studio
 sudo systemctl enable singlestoredb-studio.service
 
 ```
  
 
 ## Backup
 
 Se puede obtener respaldos totales o incrementales. Los respaldos totales escriben datos en un sistema de archivos local file, s3, azure, Google Cloud.
 
 SingleStore no soporta un restore de base de datos desde una version nueva a una antigua.
 
 ```sh
 sdb-admin create-backup -r file:///path database1
 
 ``` 
 
 
 ## Agregar un Azure Blob Storage a Linux
 
 Para agregar una unidad de Azure Storage a un servidor VM Linux es necesario:
 
 1.- Crear una carpeta compartida 
 
 Azure portal => Storage accounts (Crear) ==> Seleccionar el Storage account ==> File shares (crear)
 
 Dentro de Storage accounts => Key (Copiar la Key)
 
 -	Storage accounts = almacenamientoss
 -	File shares = compartido
 -	Key = *******


Con estos datos vamos a la maquina virtual y vamos a montar la unidad de almacenamiento:

```sh
sudo mkdir -p /mnt/compartido
sudo mount -t cifs //almacenamientoss.file.core.windows.net/compartido /mnt/compartido -o vers=3.0,username=almacenamientoss,password=*********,dir_mode=0777,file_mode=0777,serverino


sudo mount -t cifs //<storage-account>.file.core.windows.net/compartido /mnt/<file-shared> -o vers=3.0,username=<storage-account>,password=<key>,dir_mode=0777,file_mode=0777,serverino

``` 


 ## Cambiar el directorio de datos.
 
 REF: https://support.singlestore.com/hc/en-us/articles/360057955312-Change-default-data-directory-path
 
 Planeamos cambiar el directorio de datos en el nodo maestro y mover de 
 
 /var/lib/memsql/cecd8d51-6570-416f-bd6d-9aab34d26a9c/data   a   /disk01
 
 ```sh
 ## Detiene el cluster
 sdb-admin stop-node --all
 
 ## Detener un nodo
 sdb-admin stop-node --memsql-id <NODE-ID>
 
 ## Verifica el estado
 sdb-admin list-nodes
 
 ## Mueve los datos
 sudo cp -R -p <source_data_directory> <new_data_directory_path>
 sudo cp -R -p /var/lib/memsql/cecd8d51-6570-416f-bd6d-9aab34d26a9c/data /disk01
 
 ## Asegurate que el disco tenga permisos a memsql
 sudo chown -R memsql:memsql /disk01
 
 ## Actualiza la metadata con la carpeta data
 sdb-admin update-config --key datadir --value <Path to data> --memsql-id <Node Memsql ID>
 sdb-admin update-config --key datadir --value /disk01/data --memsql-id 783C28B24F
 
 ## Verifica
 sdb-admin describe-node --memsql-id <Node Memsql ID>
 sdb-admin describe-node --memsql-id 783C28B24F
 
 ## Levanta los servicios
 sdb-admin start-node --all
 
 ``` 

## Backup & Restore

Para poder obtener un backup es necesario tener un directorio compartido o un Storage Container, S3, etc.


```sh

sudo mkdir -p /respaldo
sudo groupadd sambashare 
sudo chgrp sambashare /respaldo
sudo useradd -M -d /respaldo/externo -s /usr/sbin/nologin -G sambashare externo

``` 

```sh

sudo mkdir -p /respaldo/externo
sudo chown externo:sambashare /respaldo/externo
sudo chmod 2770 /respaldo/externo
## Crea la clave
sudo smbpasswd -a externo
## Habilita al usuario
sudo smbpasswd -e externo

``` 

```sh

sudo useradd -M -d /respaldo/users -s /usr/sbin/nologin -G sambashare sadmin
sudo smbpasswd -a sadmin
sudo smbpasswd -e sadmin
sudo mkdir /respaldo/users
sudo chown sadmin:sambashare /respaldo/users
sudo chmod 2770 /respaldo/users

``` 

```sh
sudo nano /etc/samba/smb.conf

[users]
    path = /respaldo/users
    browseable = yes
    read only = no
    force create mode = 0660
    force directory mode = 2770
    valid users = @sambashare @sadmin

[externo]
    path = /respaldo/externo
    browseable = yes
    read only = no
    force create mode = 0660
    force directory mode = 2770
    valid users = externo @sadmin
    
```

```sh

sudo systemctl restart smb
sudo systemctl restart nmb

Comprobar:
smbclient -L localhost

``` 


Montar la carpeta compartida en los nodos remotos

```sh

sudo mkdir -p /datarespaldo

sudo mount -t cifs -o username=sadmin //10.0.0.6/respaldo /datarespaldo

Añadir en fstab

//10.0.0.6//respaldo /datarespaldo cifs username=sadmin,password=pass 0 0


``` 


Desmontar la unidad

```sh
umount -l /PATH/OF/BUSY-DEVICE
umount -f /PATH/OF/BUSY-NFS (NETWORK-FILE-SYSTEM)
``` 
