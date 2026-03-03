# TallerFebrero2026

Topología del Laboratorio
El lab consta de 4 máquinas virtuales. La idea es usar CentOS como servidor de archivos y Ubuntu como cliente que también sirve esos archivos por web

Hostname,SO,IP,Rol
centos01,CentOS Stream 9,192.168.10.11,Servidor NFS Principal
centos02,CentOS Stream 9,192.168.10.12,Nodo secundario
ubuntu01,Ubuntu 24.04,192.168.10.21,Cliente NFS + Servidor Web
ubuntu02,Ubuntu 24.04,192.168.10.22,Cliente NFS

Topología del Laboratorio

[INSERTAR AQUÍ IMAGEN O DIAGRAMA DE LA TOPOLOGÍA]

Hostname,SO,IP,Rol
nfs01,CentOS Stream 9,[192.168.10.11 192.168.10.12],Servidor NFS Principal 
app01,Ubuntu 24.04,[192.168.10.21 192.168.10.22],Cliente NFS + Servidor Web 

Requisitos previosPara ejecutar este proyecto se necesita:

Ansible instalado en la máquina de control.
Acceso SSH sin contraseña entre la máquina de control y los nodos.
Red interna funcional entre nodos (192.168.10.0/24)


Cómo ejecutarEjecutar los playbooks en orden desde la máquina de control:Bash# 1. Configurar Servidor NFS (nfs01)
ansible-playbook -i inventory/hosts.ini playbooks/nfs-server.yml

Configurar Cliente NFS y Web (app01)
ansible-playbook -i inventory/hosts.ini playbooks/nfs-client.yml


Cómo verificar (Evidencia funcional)
Servidor NFS (nfs01 - CentOS)Verificar servicio nfs-server activo:Bashsystemctl is-active nfs-server


[INSERTAR CAPTURA DE PANTALLA AQUÍ]

Verificar directorio exportado:Bashexportfs -v
[INSERTAR CAPTURA DE PANTALLA AQUÍ]

Verificar reglas de firewall:Bashfirewall-cmd --list-services | egrep 'nfs|mountd|rpc-bind'
[INSERTAR CAPTURA DE PANTALLA AQUÍ]

Cliente NFS (app01 - Ubuntu)Verificar servicio autofs activo:Bashsystemctl is-active autofs
[INSERTAR CAPTURA DE PANTALLA AQUÍ]

Verificar montaje automático:Bashls /mnt/shared
mount | grep /mnt/shared


[INSERTAR CAPTURA DE PANTALLA AQUÍ]

Servicio Web (app01 - Ubuntu)Verificar estado del servicio Web:Bashsystemctl status shared-http --no-pager
[INSERTAR CAPTURA DE PANTALLA AQUÍ]

Verificar acceso HTTP:Bashcurl http://localhost:8080/README-NFS.txt
[INSERTAR CAPTURA DE PANTALLA AQUÍ]


Consideraciones de IAG Para este trabajo se utilizó Inteligencia Artificial Generativa (IAG) para la creación de la estructura del README y validación de comandos.Prompts: Generame una estructura para un readme con el archivo pdf que te acabo de pasar para yo rellenar con los datos y capturas correctas.
Tambien se pidio: comandos para actualizar un repositorio git en centos que ya esta cargado en mi perfil


En el cliente Ubuntu (ubuntu)
Entrar y ver si se montó la carpeta
ls /mnt/shared

Probar el servicio web
curl -I http://localhost:8080/
