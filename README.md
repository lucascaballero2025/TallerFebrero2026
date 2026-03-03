# TallerFebrero2026

Topología del Laboratorio
El lab consta de 4 máquinas virtuales. La idea es usar CentOS como servidor de archivos y Ubuntu como cliente que también sirve esos archivos por web

Hostname,SO,IP,Rol
centos01,CentOS Stream 9,192.168.10.11,Servidor NFS Principal
centos02,CentOS Stream 9,192.168.10.12,Nodo secundario
ubuntu01,Ubuntu 24.04,192.168.10.21,Cliente NFS + Servidor Web
ubuntu02,Ubuntu 24.04,192.168.10.22,Cliente NFS

Topología del Laboratorio

<img width="398" height="366" alt="image" src="https://github.com/user-attachments/assets/06fa10cb-bb7b-4985-b2e2-25d68b9a07fc" />

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

Verificar directorio exportado:Bashexportfs -v

Verificar reglas de firewall:Bashfirewall-cmd --list-services | egrep 'nfs|mountd|rpc-bind'

<img width="914" height="164" alt="image" src="https://github.com/user-attachments/assets/91539fe0-3c03-4d74-a395-4ce7be869e8f" />

Cliente NFS (app01 - Ubuntu)Verificar servicio autofs activo:Bashsystemctl is-active autofs

Verificar montaje automático:Bashls /mnt/shared
mount | grep /mnt/shared

Servicio Web (app01 - Ubuntu)Verificar estado del servicio Web:Bashsystemctl status shared-http --no-pager

Verificar acceso HTTP:Bashcurl http://localhost:8080/README-NFS.txt

<img width="1275" height="389" alt="image" src="https://github.com/user-attachments/assets/ffefa4f2-5aba-4f3d-a9a5-13630bf95081" />


Consideraciones de IAG Para este trabajo se utilizó Inteligencia Artificial Generativa (IAG) para la creación de la estructura del README y validación de comandos.Prompts: Generame una estructura para un readme con el archivo pdf que te acabo de pasar para yo rellenar con los datos y capturas correctas.
Tambien se pidio: comandos para actualizar un repositorio git en centos que ya esta cargado en mi perfil


En el cliente Ubuntu (ubuntu)
Entrar y ver si se montó la carpeta
ls /mnt/shared

Probar el servicio web
curl -I http://localhost:8080/
