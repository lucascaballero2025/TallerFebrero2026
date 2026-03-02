# TallerFebrero2026

Topología del Laboratorio
El lab consta de 4 máquinas virtuales. La idea es usar CentOS como servidor de archivos y Ubuntu como cliente que también sirve esos archivos por web

Hostname,SO,IP,Rol
centos01,CentOS Stream 9,192.168.10.11,Servidor NFS Principal
centos02,CentOS Stream 9,192.168.10.12,Nodo secundario
ubuntu01,Ubuntu 24.04,192.168.10.21,Cliente NFS + Servidor Web
ubuntu02,Ubuntu 24.04,192.168.10.22,Cliente NFS

Para que los playbooks funcionen bien, tener lo siguiente:

Tener Ansible instalado en la máquina Bastion (192.168.10.1)
Tener las llaves SSH listas para entrar a los nodos como usuario sysadmin sin pedir contraseña.
Que todas las máquinas tengan conectividad entre ellas en la red 192.168.10.0/24.


¿Qué funcion tiene cada archivo?
Carpetas files/ y templates/
auto.nfs y nfs.autofs: Configuran autofs en los clientes Ubuntu para montar automáticamente la carpeta compartida desde CentOS al entrar en ella

shared-http.service: Es el archivo que creé para que systemd levante un servidor web básico con Python en el puerto 8080 en ubuntu01

README-NFS.j2: Plantilla para generar el archivo de prueba que muestra info del servidor NFS

Inventario (inventories/host.ini)
Aquí se definen los grupos de máquinas (centos, ubuntu, fileserver) y sus IPs para que Ansible sepa dónde trabajar

Playbooks (playbooks/)
hardening.yaml: Hace un update general de paquetes y bloquea el login de root por SSH para mejorar la seguridad

nfs-server.yaml: Configura centos01 como servidor NFS. Instala lo necesario, crea la carpeta /srv/nfs/shared, la exporta y abre el firewall

nfs-client.yaml: Configura ubuntu01. Instala autofs y Python, copia las configuraciones para montar el NFS y activa el servicio web en systemd

Cómo correr los playbooks
Ejecuta los siguientes comandos desde la carpeta raíz que es TallerFebrero2025
Lo ideal es hacerlo en este orden para que todo quede bien configurado y funcionando correctamente...

# Paso 1: Actualizar y asegurar servidores
ansible-playbook -i inventories/host.ini playbooks/hardening.yaml

# Paso 2: Levantar el servidor NFS en CentOS
ansible-playbook -i inventories/host.ini playbooks/nfs-server.yaml

# Paso 3: Configurar el cliente Ubuntu y el servicio web
ansible-playbook -i inventories/host.ini playbooks/nfs-client.yaml

5. Pruebas de funcionamiento
En el servidor CentOS (centos)

# Ver que NFS está corriendo
systemctl is-active nfs-server
# Ver la carpeta exportada
exportfs -v

En el cliente Ubuntu (ubuntu)
# Entrar y ver si se montó la carpeta
ls /mnt/shared
# Probar el servicio web
curl -I http://localhost:8080/
