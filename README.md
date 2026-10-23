# AA1-NFS.md
Activitat 1 UD4 NFS

Fase 1. Preparació de l'entorn

Per fer l'activitat, utilitzarem dues màquines virtuals: Ubuntu Server 24.04 LTS com a servidor i Zorin OS 18 com a client. Totes dues tindran dues interfícies de xarxa: NAT per accedir a Internet i només-amfitrió per comunicar-se entre elles. Les instal·larem en espanyol, amb els requisits recomanats, activarem SSH al servidor i actualitzarem els dos sistemes.

Creació del client:
![Creació del client](/img/Captura%20de%20pantalla%202026-10-21%20183936.png)

Configuració xarxa client: 

![Configuracó xarxa](/img/Captura%20de%20pantalla%202026-10-21%20184337.png)


Configuració servidor:

![Config servidor](/img/Captura%20de%20pantalla%202026-10-21%20185414.png)


Fase 2: Preparació del servidor

Al servidor, crearem els grups devs i admins, i els usuaris dev01 i admin01, cadascun al seu grup. Després, crearem els directoris /srv/nfs/dev_projects i /srv/nfs/admin_tools, amb root com a propietari i permisos perquè cada grup controli el seu directori. Finalment, instal·larem i configurarem el servei NFS per compartir-los amb el client, on també replicarem els usuaris i grups amb els mateixos UID i GID.

Creació dels grups:
![grups](/img/Captura%20de%20pantalla%202026-10-23%20184618.png)

Creació usuari dev01:
![creació d'usuaris](/img/Captura%20de%20pantalla%202026-10-23%20185409.png)

Creació de directoris:
![creació de directoris](/img/Captura%20de%20pantalla%202026-10-23%20185942.png)

Permisos del servidor:
![permisos](/img/Captura%20de%20pantalla%202026-10-23%20190202.png)

Instal·lació de paquets NFS:

Fem un "sudo apt update" 
![instal·lació de paquets](/img/Captura%20de%20pantalla%202026-10-23%20191626.png)


Fase 3: L'Exportació d'Administració (El Dilema del root_squash)

El client necessita que el directori /srv/nfs/admin_tools sigui accessible per l'equip d'administradors. A vegades, l'usuari root del client (que sou vosaltres, els consultors) necessitarà escriure en aquest directori per instal·lar eines. Aquí mostrarem un error típic i la seva solució.

Prova 1:

Exportar el directori /srv/nfs/admin_tools amb les opcions 'rw,sync':
![exportació del directori](/img/Captura%20de%20pantalla%202026-10-23%20195444.png)

Des del client, muntar aquest recurs compartit a /mnt/admin_tools. Com a root del client, intentar crear un fitxer dins d'aquest directori muntat:

![pas 2](/img/Captura%20de%20pantalla%202026-10-23%20195557.png)


Verificar quin és el propietari del fitxer creat. Per què? Justificar la resposta amb l'explicació tècnica de 'root_squash'.

![pas3](/img/Captura%20de%20pantalla%202026-10-23%20195642.png)


El fitxer prova1.txt creat com a root del client apareix amb propietari nobody, no root.

Per què? Amb l'exportació rw,sync s'aplica per defecte root_squash. 




Prova 2:

Modificar l'exportació del directori /srv/nfs/admin_tools per incloure l'opció 'no_root_squash'.

![pas1 prova2](/img/Captura%20de%20pantalla%202026-10-23%20200127.png)

Al client, desmuntar i tornar a muntar el recurs compartit.

![pas 2 prova2](/img/Captura%20de%20pantalla%202026-10-23%20200339.png)

Com a root del client, intentar crear un fitxer dins d'aquest directori muntat. Observeu quin és el propietari del fitxer creat aquesta vegada. Ha canviat alguna cosa? Justificar la resposta amb l'explicació tècnica de 'no_root_squash'.

![pas 3 prova2](/img/Captura%20de%20pantalla%202026-10-23%20200523.png)



Aquesta vegada el fitxer prova2.txt té propietari root. Sí que ha canviat: a la Prova 1 era nobody.

Explicació tècnica: amb no_root_squash el servidor deixa de transformar l'UID 0 del client i el tracta com a root real, així que el fitxer es crea amb propietari root. 

Fase 4: L'Exportació de Desenvolupament (Permisos rw vs ro)

Editar /etc/exports per afegir dues exportacions per al mateix directori. El client vol que la xarxa d'administració (p.ex., 192.168.56.0/24) hi pugui escriure, però que la xarxa de consultors (simularem que és una altra IP, p.ex., 192.168.56.100) només pugui llegir.

![exportació](/img/Captura%20de%20pantalla%202026-10-23%20200821.png)

Des del client, muntar el recurs compartit a /mnt/dev_projects i provar d'escriure-hi com a usuari dev01. Hauria de funcionar.

![muntar el recurs](/img/Captura%20de%20pantalla%202026-10-23%20201003.png)

Desmuntar el recurs i canviar manualment la IP del client a 192.168.56.100. Tornar a provar d'escriure al directori muntat com a usuari dev01 i comprovar que només funciona la lectura.

![show enp0s8](/img/Captura%20de%20pantalla%202026-10-23%20201128.png)

![tornar a montar i escirure](/img/Captura%20de%20pantalla%202026-10-23%20201404.png)

Canvieu d'usuari al client a admin01 i torneu a provar d'escriure al directori muntat. Comproveu que no es pot escriure, ja que admin01 no és membre del grup devs (permisos locals del sistema de fitxers).

![canvi usuari](/img/Captura%20de%20pantalla%202026-10-23%20201842.png)
