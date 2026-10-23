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