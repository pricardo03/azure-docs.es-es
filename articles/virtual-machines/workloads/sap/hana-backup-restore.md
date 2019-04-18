---
title: Copia de seguridad y restauración de HANA en SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Realización de la copia de seguridad y restauración de HANA en SAP HANA en Azure (instancias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/01/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 69417551c1c8d410f75e74a8164c8b8a223ab835
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58805336"
---
# <a name="backup-and-restore"></a>Copia de seguridad y restauración

>[!IMPORTANT]
>Esta documentación no sustituye a la documentación de administración de SAP HANA ni a las notas de SAP. Se espera que el lector tenga unos sólidos conocimientos de la administración y operaciones de SAP HANA, sobre todo en los temas relativos a la copia de seguridad, restauración, alta disponibilidad y recuperación ante desastres. En esta documentación, se muestran capturas de pantalla de SAP HANA Studio. El contenido, la estructura y la naturaleza de las pantallas de las herramientas de administración de SAP y de las herramientas en sí pueden cambiar de una versión a otra de SAP HANA.

Es importante que practique los pasos y procesos que se realizan en su entorno y con sus versiones de HANA. Algunos procesos descritos en esta documentación se han simplificado para mejorar la comprensión general y no están diseñados para utilizarse como pasos detallados en posibles manuales de uso. Si desea crear manuales de operaciones de sus configuraciones, debe probar y ejecutar los procesos, así como documentar aquellos relacionados con sus configuraciones concretas. 

Uno de los aspectos más importantes del funcionamiento de las bases de datos es protegerlas de eventos catastróficos. La causa de estos eventos pueden deberse a cualquier cosa, desde desastres naturales a simples errores del usuario.

La copia de seguridad de una base de datos, con la capacidad de realizar la restauración a cualquier momento dado (como antes de que alguien eliminara datos críticos), posibilita la restauración a un estado que sea lo más parecido posible al que había antes de que la interrupción.

Para que el resultado sea óptimo, es preciso realizar dos tipos de copias de seguridad:

- Copias de seguridad de bases de datos: completas, incrementales o diferenciales
- Copias de seguridad del registro de transacciones

Además de las copias de seguridad completas de la base de datos que se realizan en un nivel de aplicación, puede realizar copias de seguridad con instantáneas de almacenamiento. Las instantáneas de almacenamiento no reemplazan las copias de seguridad del registro de transacciones. Las copias de seguridad del registro de transacciones siguen siendo importantes para restaurar la base de datos a un momento dado o para eliminar las transacciones ya confirmadas de los registros. Sin embargo, las instantáneas de almacenamiento pueden acelerar la recuperación al proporcionar rápidamente una imagen de puesta al día de la base de datos. 

SAP HANA en Azure (instancias grandes) ofrece dos opciones de copia de seguridad y restauración:

- Hágalo Vd. mismo. Después de realizar los cálculos necesarios para asegurarse de que hay suficiente espacio en el mismo, realice copias de seguridad completas tanto de las bases de datos como del registro mediante uno de los siguientes métodos de copia de seguridad de disco. Puede hacer copias de seguridad directamente en los volúmenes conectados a las unidades de HANA (instancias grandes) o en los recursos compartidos de archivos de red (NFS) configurados en una máquina virtual (VM) de Azure. En el último caso, los clientes configuran una VM Linux en Azure, adjuntan Azure Storage a dicha VM y comparten el almacenamiento a través de un servidor NFS configurado en esa máquina virtual. Si se realiza la copia de seguridad en los volúmenes que directamente se adjuntan a unidades de instancia grande de HANA, necesita copiar las copias de seguridad a una cuenta de Azure Storage (una vez que configure una VM de Azure que exporte los recursos compartidos NFS). También puede usar un almacén de Azure Backup o el almacenamiento en frío de Azure. 

   Otra opción consiste en usar una herramienta de protección de datos de terceros para almacenar las copias de seguridad una vez que se copian en una cuenta Azure Storage. Esta opción de copia de seguridad DIY también podría ser necesaria para aquellos datos que necesita almacenar durante períodos más largos de tiempo con fines de conformidad y auditoría. En todos los casos, las copias de seguridad se copian en recursos compartidos NFS que se representan a través de una VM y Azure Storage.

- Funcionalidad de copia de seguridad y restauración de infraestructura. También puede usar la funcionalidad de copia de seguridad y restauración que proporciona la infraestructura subyacente de SAP HANA en Azure (instancias grandes). Esta opción cumple con la necesidad de copias de seguridad y restauraciones rápidas. En el resto de esta sección se trata la funcionalidad de copia de seguridad y restauración que se ofrece con las instancias grandes de HANA. Esta sección también trata la relación que la copia de seguridad y la restauración tienen con la funcionalidad de recuperación ante desastres que proporciona HANA (instancias grandes).

> [!NOTE]
>   La tecnología de instantáneas que utiliza la infraestructura subyacente de instancias grandes de HANA depende de las instantáneas de SAP HANA. En este momento, las instantáneas de SAP HANA no funcionan en conjunción con los distintos inquilinos de los contenedores de base de datos multiinquilino de SAP HANA. Si solo se implementa un inquilino, las instantáneas de SAP HANA funcionan y se puede usar este método.

## <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Uso de instantáneas de almacenamiento de SAP HANA en Azure (Instancias grandes)

La infraestructura de almacenamiento subyacente de SAP HANA en Azure (instancias grandes) admite instantáneas de almacenamiento de volúmenes. Se admite tanto la copia de seguridad como la restauración de volúmenes, pero es preciso tener en cuenta lo siguiente:

- En lugar de las copias de seguridad de bases de datos completas, se toman instantáneas del volumen de almacenamiento con frecuencia.
- Cuando se desencadena una instantánea sobre los volúmenes /hana/data y /hana/shared (incluye /usr/sap), la tecnología de la instantánea inicia una instantánea de SAP HANA antes de ejecutar la instantánea de almacenamiento. Esta instantánea de SAP HANA es el punto de configuración de las posibles restauraciones del registro después de la recuperación de la instantánea de almacenamiento. Para que la instantánea de HANA se realice correctamente necesita una instancia de HANA activa.  En un escenario HSR, la instantánea de almacenamiento no se admite en el nodo secundario actual en el que no se puede realizar la instantánea de HANA.
- Después de que la instantánea de almacenamiento se haya ejecutado correctamente, se elimina la instantánea de SAP HANA.
- Las copias de seguridad del registro de transacciones se realizan con frecuencia y se almacenan en el volumen /hana/logbackups o en Azure. Puede desencadenar el volumen /hana/logbackups que contiene las copias de seguridad del registro de transacciones para tomar una instantánea por separado. En ese caso, no es preciso ejecutar una instantánea de HANA.
- Si debe restaurar una base de datos a un momento dado, solicitar ese soporte técnico de Microsoft Azure (para una interrupción de producción) o SAP HANA en Azure de la restauración a una instantánea de almacenamiento determinada. Un ejemplo es una restauración planeada de un sistema de espacio aislado a su estado original.
- La instantánea de SAP HANA que se incluye en la instantánea de almacenamiento es un punto de desplazamiento para aplicar las copias de seguridad del registro de transacciones que se han ejecutado y almacenado después de que se tomara la instantánea de almacenamiento.
- Estas copias de seguridad del registro de transacciones se realizan para restaurar la base de datos a un momento dado.

Puede realizar instantáneas de almacenamiento dirigidas a tres clases de volúmenes:

- Una instantánea combinada en /hana/data y /hana/shared (incluye /usr/sap). Esta instantánea requiere la creación de una instantánea de SAP HANA como preparación para la instantánea de almacenamiento. La instantánea de SAP HANA se asegura de que la base de datos esté en un estado coherente desde un punto de vista de almacenamiento, y de que se encuentre en un punto configurable para el proceso de restauración.
- Una instantánea independiente en /hana/logbackups.
- Una partición del sistema operativo.

Obtenga los scripts de instantáneas más recientes y la documentación en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Cuando descargue el paquete de scripts de instantáneas desde [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts), también obtendrá la documentación en PDF de los scripts como parte del paquete de scripts. Cada paquete de script tiene su propia documentación en PDF.

## <a name="storage-snapshot-considerations"></a>Consideraciones de las instantáneas de almacenamiento

>[!NOTE]
>Las instantáneas de almacenamiento consumen espacio de almacenamiento que se ha asignado a las unidades de instancia grande de HANA. Por tanto, es necesario tener en cuenta los siguientes aspectos de la programación de instantáneas de almacenamiento y del número de instantáneas que se mantienen. 

Mecánica concreta de las instantáneas de almacenamiento de SAP HANA en Azure (instancias grandes):

- Una instantánea de almacenamiento concreta (en el momento en que se toma) consume poco espacio de almacenamiento.
- Cuando el contenido de datos cambia y el contenido de los archivos de datos de SAP HANA cambia en el volumen de almacenamiento, la instantánea debe almacenar el contenido del bloque original, así como los cambios de datos.
- El resultado es que la instantánea de almacenamiento aumenta de tamaño. Cuanto más tiempo dure la instantánea, mayor será la instantánea de almacenamiento.
- Cuantos más cambios se realicen en el volumen de la base de datos de SAP HANA a lo largo de la vigencia de una instantánea de almacenamiento, mayor será el consumo de espacio de dicha instantánea.

SAP HANA en Azure (instancias grandes) incluye tamaños de volumen fijos para los volúmenes de datos y de registro de SAP HANA. Realizar instantáneas de esos volúmenes consume el espacio del volumen. Debe determinar cuándo se deben programar las instantáneas de almacenamiento. También necesita supervisar el consumo de espacio de los volúmenes de almacenamiento, así como administrar el número de instantáneas que se almacenan. Puede deshabilitar las instantáneas de almacenamiento cuando importa masas de datos o realiza otros cambios importantes en la base de datos de HANA. 


En las secciones siguientes se proporciona información para realizar estas instantáneas, donde también se incluyen recomendaciones generales:

- Aunque el hardware puede admitir 255 instantáneas por volumen, es conveniente no acercarse a ese número. La recomendación es 250 o menos.
- Antes de realizar instantáneas de almacenamiento, supervise y realice un seguimiento del espacio disponible.
- Reduzca el número de instantáneas del almacenamiento en función del espacio libre. Puede reducir el número de instantáneas que mantiene, o ampliar los volúmenes. Puede solicitar más almacenamiento en unidades de un terabyte.
- Durante actividades como mover datos a SAP HANA con herramientas de migración de la plataforma SAP (R3load) o restaurar bases de datos de SAP HANA a partir de copias de seguridad, deshabilite las instantáneas de almacenamiento en el volumen /hana/data. 
- Durante reorganizaciones mayores de tablas de SAP HANA, debe evitarse la realización de instantáneas de mantenimiento, siempre que sea posible.
- Las instantáneas de almacenamiento son un requisito previo para sacar provecho de las funcionalidades de recuperación ante desastres de SAP HANA en Azure (instancias grandes).

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Requisitos previos para usar instantáneas de almacenamiento de autoservicio

Para tener la certeza de que el script de instantáneas se ejecuta correctamente, asegúrese de que Perl está instalado en el sistema operativo Linux en el servidor de HANA (instancias grandes). Perl viene preinstalado en la unidad de instancia grande de HANA. Para comprobar la versión de Perl, utilice la siguiente línea de comandos:

`perl -v`

![La clave pública se copia ejecutando este comando](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Configuración de instantáneas de almacenamiento

Para configurar las instantáneas de almacenamiento con HANA (instancias grandes), siga estos pasos:
1. Asegúrese de que Perl está instalado en el sistema operativo Linux en el servidor de instancias grandes de HANA.
1. Modifique /etc/ssh/ssh\_config para agregar la línea _MACs hmac-sha1_.
1. Cree una cuenta de usuario de copia de seguridad de SAP HANA en el nodo maestro de cada instancia de SAP HANA que se ejecute, si procede.
1. Instale el cliente HDB de SAP HANA en todos los servidores de instancias grandes de SAP HANA.
1. En el primer servidor de instancias grandes de SAP HANA de cada región, cree una clave pública para acceder a la infraestructura de almacenamiento subyacente que controla la creación de instantáneas.
1. Copie los scripts y el archivo de configuración de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) a la ubicación de **hdbsql** en la instalación de SAP HANA.
1. Modifique el archivo *HANABackupDetails.txt* tanto como sea necesario para ajustarse a las especificaciones apropiadas del cliente.

Obtenga los scripts de instantáneas más recientes y la documentación en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Cuando descargue el paquete de scripts de instantáneas desde [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts), también obtendrá la documentación en PDF de los scripts como parte del paquete de scripts. Cada paquete de script tiene su propia documentación en PDF.

### <a name="consideration-for-mcod-scenarios"></a>Consideraciones sobre los escenarios MCOD
Si ejecuta un [escenario MCOD](https://launchpad.support.sap.com/#/notes/1681092) con varias instancias de SAP HANA en una unidad HANA (instancias grandes), puede tener volúmenes de almacenamiento independientes aprovisionados para cada una de las instancias de SAP HANA. En la versión actual de la funcionalidad de automatización de instantáneas de autoservicio no se pueden iniciar instantáneas independientes en cada identificador del sistema de instancias de HANA (SID). La funcionalidad comprueba las instancias de SAP HANA registradas del servidor en el archivo de configuración (consúltese más adelante en este mismo artículo) y ejecuta una instantánea simultánea de los volúmenes de todas las instancias registradas en la unidad.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Paso 1: Instalar el cliente de HDB de SAP HANA

El sistema operativo Linux instalado en SAP HANA en Azure (instancias grandes) incluye las carpetas y los scripts necesarios para ejecutar las instantáneas de almacenamiento de SAP HANA para la realización de copias de seguridad y para la recuperación ante desastres. Busque las versiones más recientes en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). La versión más reciente de los scripts es 3.x. Los scripts podrían tener diferentes versiones secundarias dentro de la misma versión principal.

>[!IMPORTANT]
>Cuando se pasa de la versión 2.1 a la versión 3.x de los scripts, es preciso tener en cuenta que la estructura del archivo de configuración y una parte de la sintaxis ha cambiado. Vea las llamadas en las secciones concretas. 

Es responsabilidad suya instalar el cliente de HDB de SAP HANA en las unidades de instancia grande de HANA durante la instalación de SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Paso 2: Cambiar /etcetera/ssh/ssh\_config

Cambie `/etc/ssh/ssh_config` agregando la línea _MACs hmac-sha1_, como se muestra a continuación:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Paso 3: Crear una clave pública

Para habilitar el acceso a las interfaces de las instantáneas de almacenamiento del inquilino de HANA (instancia grande), es preciso establecer procedimiento de un inicio de sesión a través de una clave pública. En el primer servidor de SAP HANA en Azure (instancias grandes) del inquilino, cree una clave pública que se use para acceder a la infraestructura de almacenamiento. La clave pública garantiza que no se necesita una contraseña para iniciar sesión en las interfaces de instantánea de almacenamiento. La creación de una clave pública también significa que no es necesario mantener credenciales de contraseña. En Linux, en el servidor de instancias grandes de SAP HANA, ejecute el siguiente comando para generar la clave pública:
```
  ssh-keygen -t rsa –b 5120 -C ""
```

La nueva ubicación es **_/root/.ssh/id\_rsa.pub**. No escriba una contraseña real; de lo contrario, se le pedirá que la escriba cada vez que inicie sesión. En su lugar, seleccione la tecla **Intro** dos veces para quitar el requisito de especificación de contraseña al iniciar sesión.

Asegúrese de que la clave pública se corrigió según lo previsto cambiando las carpetas a **/root/.ssh/** y, después, ejecutando el comando `ls`. Si la clave está presente, puede copiarla ejecutando el comando siguiente:

![La clave pública se copia ejecutando este comando](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

En este momento, póngase en contacto con SAP HANA en Azure y proporcióneles la clave pública. El representante del servicio usa la clave pública para registrarla en la infraestructura de almacenamiento subyacente que se crea para el inquilino de instancia grande de HANA.

### <a name="step-4-create-an-sap-hana-user-account"></a>Paso 4: Crear una cuenta de usuario de SAP HANA

Para iniciar la creación de instantáneas de SAP HANA, es necesario crear una cuenta de usuario en SAP HANA, que los scripts de instantánea de almacenamiento puedan usar. Cree una cuenta de usuario de SAP HANA en SAP HANA Studio para esta finalidad. El usuario debe crearse en SYSTEMDB y NO en la base de datos de SID para MDC. En el entorno de contenedor único, el usuario se configura en la base de datos de inquilinos. Esta cuenta debe tener los siguientes privilegios: **Administrador de la copia de seguridad** y **Lectura del catálogo**. En este ejemplo, el nombre de usuario es **SCADMIN**. El nombre de la cuenta de usuario que se creó en HANA Studio distingue mayúsculas de minúsculas. Asegúrese de seleccionar **No** para solicitar al usuario que cambie la contraseña la próxima vez que inicie de sesión.

![Creación de un usuario en HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

Si usa implementaciones MCOD con varias instancias de SAP HANA en una unidad, es preciso repetir este paso en todas las instancias de SAP HANA.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Paso 5: Autorizar la cuenta de usuario de SAP HANA

En este paso se autoriza a la cuenta de usuario de SAP HANA que creó, de manera que los scripts no necesitan enviar contraseñas en tiempo de ejecución. El comando de SAP HANA `hdbuserstore` habilita la creación de una clave de usuario de SAP HANA, que se almacena en uno o varios nodos de SAP HANA. La clave de usuario permite que el usuario acceda a SAP HANA sin tener que administrar contraseñas mediante el proceso de scripting. El proceso de scripting se describe más adelante en este mismo artículo.

>[!IMPORTANT]
>Ejecute el siguiente comando como el usuario en el que se ha previsto que se ejecuten los scripts. De lo contrario, el script no funcionará correctamente.

Escriba el comando `hdbuserstore` de esta forma:

**Para la configuración de HANA sin MDC**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Para la configuración de HANA MDC**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

En el ejemplo siguiente, el usuario es **SCADMIN01**, el nombre del host es **lhanad01** y el número de instancia es **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Si usa una implementación MCOD de HANA con varias instancias de SAP HANA en una unidad, es preciso que repita el paso en todas las instancias de SAP HANA y en el usuario de copia de seguridad asociado de la unidad.

Si tiene una configuración de escalabilidad horizontal de SAP HANA, es preciso que administre todo el scripting desde un solo servidor. En este ejemplo, la clave de SAP HANA **SCADMIN01** debe modificarse en todos los hosts para que refleje qué host está relacionado con ella. Modifique la cuenta de copia de seguridad de SAP HANA con el número de instancia de la base de datos de HANA. La clave debe tener privilegios administrativos en el host al que se asigna y el usuario de copia de seguridad de las configuraciones del escalado horizontal debe tener derechos de acceso a todas las instancias de SAP HANA. Si los tres nodos de escalado horizontal se llaman **lhanad01**, **lhanad02** y **lhanad03**, la secuencia de comandos es la siguiente:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Paso 6: Obtener los scripts de instantánea, configurar las instantáneas y probar la configuración y la conectividad

Descargue la versión más reciente de los scripts de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Copie los scripts descargados y el archivo de texto en el directorio de trabajo de **hdbsql**. En el caso de las instalaciones actuales de HANA, este directorio tiene el siguiente formato: /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl
azure_hana_dr_failover.pl
azure_hana_test_dr_failover.pl 
HANABackupCustomerDetails.txt 
``` 

Cuando use scripts de Perl: 

- No los modifique, salvo que lo indique el equipo de Microsoft Operations.
- Cuando se le pida que modifique el script o un archivo de parámetros, use siempre un editor de texto de Linux, como "vi", no uno de Windows, como el Bloc de notas. El uso de un editor de Windows puede dañar el formato del archivo.
- Utilice siempre los scripts más recientes. Puede descargar la versión más reciente de GitHub.
- Utilice la misma versión de los scripts en todo el entorno.
- Pruebe los scripts y familiarícese con los parámetros necesarios y con la salida del script antes de usarlos directamente en el sistema de producción.
- No cambie el nombre del punto de montaje del servidor que aprovisionó el equipo de Microsoft Operations. Para que estos scripts funcionen, estos puntos de montaje estándar deben estar disponibles.


Esta es la finalidad de los distintos scripts y archivos:

- **azure\_hana\_backup.pl**: este script se programa con la utilidad Linux Cron Scheduling para ejecutar instantáneas de almacenamiento en los volúmenes compartidos o de datos de HANA, en el volumen /hana/logbackups o en el sistema operativo.
- **azure\_hana\_replication\_status.pl**: este script proporciona los detalles básicos del estado de la replicación desde el sitio de producción al sitio de recuperación ante desastres. El script realiza la supervisión para asegurarse de que se está llevando a cabo la replicación y muestra el tamaño de los elementos que se están replicando. También brinda una guía de lo que se debe hacer si la replicación tarda demasiado o si el vínculo no está activo.
- **azure\_hana\_snapshot\_details.pl**: este script proporciona una lista de los detalles básicos sobre todas las instantáneas, por volumen, que existen en el entorno. Este script se puede ejecutar en el servidor principal o en una unidad de servidor en la ubicación de recuperación ante desastres. El script proporciona la siguiente información desglosada por cada volumen que contiene instantáneas:
   * El tamaño de las instantáneas totales de un volumen
   * Los siguientes detalles de cada instantánea del volumen: 
      - Nombre de la instantánea 
      - Hora de creación 
      - Tamaño de la instantánea
      - Frecuencia de la instantánea
      - Identificador de copia de seguridad de HANA asociado con esa instantánea, si procede
- **azure\_hana\_snapshot\_delete.pl**: este script elimina una instantánea de almacenamiento o un conjunto de instantáneas. Puede usar el identificador de copia de seguridad de SAP HANA que se encuentra en HANA Studio o el nombre de la instantánea de almacenamiento. Actualmente, el identificador de la copia de seguridad solo está asociado a las instantáneas creadas para los volúmenes data/log/shared de HANA. En caso contrario, si se escribe el identificador de la instantánea, busca todas las instantáneas que coincidan con dicho identificador.  
- **testHANAConnection.pl**: este script prueba la conexión con la instancia de SAP HANA y es necesario para configurar las instantáneas de almacenamiento.
- **testStorageSnapshotConnection.pl**: este script tiene dos finalidades. En primer lugar, garantiza que la unidad de HANA (instancias grandes) que ejecuta los scripts tiene acceso a la máquina virtual de almacenamiento asignada y a la interfaz de la instantánea de almacenamiento de HANA (instancias grandes). La segunda finalidad es crear una instantánea temporal para la instancia de HANA que se prueba. Este script se debe ejecutar para cada instancia de HANA en un servidor a fin de garantizar que los scripts de copia de seguridad funcionan según lo previsto.
- **removeTestStorageSnapshot.pl**: este script elimina la instantánea de prueba que se creó con el script **testStorageSnapshotConnection.pl**.
- **azure\_hana\_dr\_failover.pl**: este script inicia una conmutación por error de recuperación ante desastres en otra región. El script se tiene que ejecutar en la unidad de HANA (instancias grandes) de la región de la recuperación ante desastres o en la unidad a la que se desea conmutar cuando se produce un error. Este script detiene la replicación del almacenamiento del lado principal al secundario, restaura la instantánea más reciente en los volúmenes de recuperación ante desastres y proporciona los puntos de montaje de los volúmenes de recuperación ante desastres.
- **azure\_hana\_test\_dr\_failover.pl**: este script realiza una conmutación por error de prueba en el sitio de recuperación ante desastres. A diferencia del script azure_hana_dr_failover.pl, esta ejecución no interrumpe la replicación de almacenamiento del principal al secundario. En su lugar, se crean clones de los volúmenes de almacenamiento replicados en el lado de la recuperación ante desastres y se proporcionan los puntos de montaje de los volúmenes clonados. 
- **HANABackupCustomerDetails.txt**: este es un archivo de configuración modificable que se debe modificar para adaptarlo a la configuración de SAP HANA. El archivo *HANABackupCustomerDetails.txt* es el archivo de control y configuración del script que ejecuta las instantáneas de almacenamiento. Ajuste el archivo según sus finalidades y la configuración. Recibirá el **nombre de la copia de seguridad de almacenamiento** y **dirección IP de almacenamiento** de SAP HANA en Azure cuando se implementan las instancias. No puede modificar la secuencia, el orden ni el espaciado de ninguna de las variables de este archivo. Si lo hace, los scripts no se ejecutan correctamente. Además, recibe la dirección IP del nodo escalado vertical o el nodo maestro (si el escalado horizontal) de SAP HANA en Azure. También conoce el número de instancia de HANA que se obtiene durante la instalación de SAP HANA. Ahora necesita agregar un nombre de copia de seguridad al archivo de configuración.

En el caso de una implementación de la escalabilidad vertical u horizontal, el archivo de configuración sería como el del ejemplo siguiente una vez que rellene el nombre del servidor de la unidad de HANA instancias grandes de Hana y la dirección IP del servidor. Rellene todos los campos necesarios de cada SID de SAP HANA del que desee hacer una copia de seguridad o recuperar.

También puede convertir en comentario las filas de instancias de las que no desea realizar una copia de seguridad durante un período de tiempo agregando un símbolo "#" delante de un campo obligatorio. Tampoco es preciso que especifique todas las instancias de SAP HANA que se encuentran en un servidor si no es preciso hacer una copia de seguridad o recuperar esa instancia concreta. El formato debe mantenerse para todos los campos o todos los scripts mostrarán un mensaje de error y el script terminará. Puede eliminar más filas necesarias de los detalles de la información del SID que no utilice después de la última instancia de SAP HANA en uso. Todas las filas se deben rellenar, eliminar o convertir en comentarios.

>[!IMPORTANT]
>La estructura del archivo cambió con el paso de la versión 2.1 a la 3.x. Si desea utilizar los scripts de la versión 3.x, debe adaptar la estructura del archivo de configuración. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

En todas las instancias que configure en la unidad de HANA (instancias grandes) o en la configuración de la escalabilidad horizontal es preciso definir los datos como se indica a continuación:

    
```
######***SID #1 Information***#####
SID1: h01
###Provided by Microsoft Operations###
SID1 Storage Backup Name: clt1h01backup
SID1 Storage IP Address: 172.18.18.11
######     Customer Provided    ######
SID1 HANA instance number: 00
SID1 HANA HDBuserstore Name: SCADMINH01
```
En las configuraciones de replicación del sistema de HANA y de la escalabilidad horizontal, repita esta configuración en todos los nodos. Esta medida garantiza que, en caso de error, las copias de seguridad y la posible replicación de almacenamiento siguen funcionando.   

Después de colocar todos los datos de configuración en el archivo *HANABackupCustomerDetails.txt* compruebe si las configuraciones son correctas para los datos de la instancia de HANA. Use el script `testHANAConnection.pl`, que es independiente de una configuración de escalado vertical o escalado horizontal de SAP HANA.

```
testHANAConnection.pl
```

Si se trata de una configuración de escalado horizontal de SAP HANA, asegúrese de que la instancia de HANA principal tiene acceso a todos los servidores e instancias de HANA necesarios. En el script de prueba no hay parámetros, pero es preciso agregar los datos en el archivo de configuración *HANABackupCustomerDetails.txt* para que el script se ejecute correctamente. Solo se devuelven los códigos de error del comando shell, por lo que el script no puede comprobar los errores en todas las instancias. Aun así, el script proporciona algunos comentarios útiles que debe repasar.

Para ejecutar el script, escriba el siguiente comando:
```
 ./testHANAConnection.pl
```
Si el script obtiene correctamente el estado de la instancia de HANA, se mostrará un mensaje que indica que la conexión con HANA se ha establecido con éxito.


El paso siguiente es comprobar la conectividad con el almacenamiento según los datos que proporciona en el archivo de configuración *HANABackupCustomerDetails.txt* y, después, ejecutar una instantánea de prueba. Antes de ejecutar el script `azure_hana_backup.pl`, debe ejecutar esta prueba. Si un volumen no contiene instantáneas, es imposible determinar si está vacío o si se produce algún error de SSH al obtener la información de la instantánea. Por este motivo, el script ejecuta dos pasos:

- Comprueba que es posible que los scripts accedan a las interfaces y a la máquina virtual de almacenamiento del inquilino a fin de ejecutar las instantáneas.
- Crea una instantánea de prueba, o ficticia, para cada volumen por instancia de HANA.

Por este motivo, la instancia de HANA se incluye como argumento. Si se produce un error en la ejecución, no es posible realizar la comprobación de errores para la conexión del almacenamiento. Incluso si no hay ninguna comprobación de errores, el script proporciona sugerencias útiles.

1. Ejecute la secuencia de comandos para realizar esta prueba:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   El nombre de usuario y la dirección IP de almacenamiento se proporcionan con la entrega de la unidad de instancia grande de HANA.

1. Ejecute el script de prueba:
   ```
    ./testStorageSnapshotConnection.pl
   ```

El script intenta iniciar sesión en el almacenamiento mediante la clave pública que se proporcionó en los pasos de la configuración anteriores y con los datos configurados en el archivo *HANABackupCustomerDetails.txt*. Si el inicio de sesión se realiza correctamente, se muestra el contenido siguiente:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Si aparece algún problema al conectarse con la consola de almacenamiento, la salida sería similar a esta:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Después de un inicio de sesión correcto en las interfaces de la máquina virtual de almacenamiento, el script pasa a la fase 2 y crea una instantánea de prueba. El resultado se muestra aquí para una configuración de escalado horizontal de tres nodos de SAP HANA:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Si la instantánea de prueba se ejecutó correctamente con el script, puede continuar con la configuración de las instantáneas de almacenamiento reales. Si el resultado no es satisfactorio, investigue los problemas antes de continuar. Se debe conservar la instantánea de prueba hasta que se hayan realizado las primeras instantáneas reales.


### <a name="step-7-perform-snapshots"></a>Paso 7: Realizar instantáneas

Cuando finalicen los pasos de preparación, puede empezar a configurar la instantánea de almacenamiento real. El script que se va a programar funciona con configuraciones de escalado vertical o escalado horizontal de SAP HANA. Para ejecutar el script de copia de seguridad de forma periódica, prográmelo mediante la utilidad cron. 

Puede crear tres tipos de copias de seguridad de instantáneas:
- **HANA**: copia de seguridad de la instantánea combinada en la que la instantánea coordinada abarca los volúmenes que contienen /hana/data y /hana/shared (que también contiene /usr/sap). Es posible realizar una sola restauración de archivos a partir de esta instantánea.
- **Registros**: copia de seguridad de la instantánea del volumen /hana/logbackups. Ninguna instantánea de HANA se desencadena para ejecutar esta instantánea de almacenamiento. Este volumen de almacenamiento está pensado para contener las copias de seguridad del registro de transacciones de SAP HANA. Estas se realizan más frecuentemente para limitar el crecimiento del registro y evitar la posible pérdida de datos. Es posible realizar una sola restauración de archivos a partir de esta instantánea. No reduzca la frecuencia a menos de 3 minutos.
- **Arranque**: instantánea del volumen que contiene el número de unidad lógica (LUN) de arranque de la instancia grande de HANA. La copia de seguridad de esta instantánea solo es posible con las SKU de tipo I de las instancias grandes de HANA. No es posible realizar restauraciones de archivos únicas a partir de la instantánea del volumen que contiene el LUN de arranque.


>[!NOTE]
> La sintaxis de llamada de estos tres tipos de instantáneas ha cambiado con el paso a los scripts de la versión 3.x, que ahora son compatibles con las implementaciones MCOD. Ya no hará falta especificar el SID de HANA de una instancia. Tiene que asegurarse de que las instancias de SAP HANA de una unidad estén configuradas en el archivo de configuración *HANABackupCustomerDetails.txt*.

>[!NOTE]
> La primera vez que ejecuta el script puede mostrar algunos errores inesperados en el entorno con varios SID. Volver a ejecutar el script corrige el problema.



La nueva sintaxis de llamada para ejecutar instantáneas de almacenamiento con el script *azure_hana_backup.pl* es como el siguiente:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

Estos son los detalles de los parámetros: 

- El primer parámetro caracteriza el tipo de la copia de seguridad de la instantánea. Los valores permitidos son **hana**, **logs** y **boot**. 
- El parámetro  **\<tipo de instancia grande de HANA >** es necesario para el arranque volumen solo copias de seguridad. Hay dos valores válidos con "TypeI" o "TypeII", en función de la unidad de instancia grande de HANA. Para averiguar el tipo de su unidad, consulte [Introducción y arquitectura de SAP HANA en Azure (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- El parámetro  **\<parámetro snapshot_prefix >** es una instantánea o etiqueta de copia de seguridad para el tipo de instantánea. Tiene dos objetivos: uno es darle un nombre, para que sepa de qué tratan estas instantáneas. El segundo es para el script *azure\_hana\_backup.pl* para determinar el número de instantáneas de almacenamiento que se conservan bajo esa etiqueta concreta. Si programa dos copias de seguridad de instantánea de almacenamiento del mismo tipo (como **hana**), con dos etiquetas diferentes y define que deben conservarse 30 instantáneas por cada una de ellas, terminará con 60 instantáneas de almacenamiento de los volúmenes afectados. Solo se admiten caracteres alfanuméricos ("A-z, a-z, 0-9"), guión bajo ("_") y guiones ("-"). 
- El parámetro  **\<snapshot_frequency >** está reservado para futuras desarrollos y no tiene ningún efecto. Establézcalo en ahora mismo en "3 min" al ejecutar copias de seguridad del tipo **registro** y en "15 min" al ejecutar los restantes tipos de copia de seguridad.
- El parámetro  **\<número de instantáneas guardadas >** define el período de retención de las instantáneas de manera indirecta al definir el número de instantáneas con el mismo prefijo de instantánea (etiqueta). Este parámetro es importante para las ejecuciones programada a través de cron. Si el número de instantáneas con el mismo parámetro snapshot_prefix supera el número que proporciona este parámetro, la instantánea más antigua se elimina antes de ejecutar una nueva instantánea de almacenamiento.

En caso de escalado horizontal, el script realiza comprobaciones adicionales para asegurarse de que puede acceder a todos los servidores de HANA. El script también comprueba que todas las instancias de HANA devuelven el estado adecuado de las instancias, antes de crear una instantánea de SAP HANA. La instantánea de SAP HANA va seguida de una instantánea de almacenamiento.

La ejecución del script `azure_hana_backup.pl` crea la instantánea de almacenamiento en las tres fases siguientes:

1. Ejecuta una instantánea de SAP HANA
1. Ejecuta una instantánea de almacenamiento
1. Quita la instantánea de SAP HANA que se creó antes de la ejecución de la instantánea de almacenamiento

Para ejecutar el script, llámelo desde la carpeta ejecutable de HDB en la que se copió. 

El período de retención se administra con el número de instantáneas que se envían como parámetro al ejecutar el script. La cantidad de tiempo que cubren las instantáneas de almacenamiento es una función del período de ejecución y del número de instantáneas que se envían como parámetro al ejecutar el script. Si el número de instantáneas que se mantienen supera el número del parámetro de la llamada del script, la instantánea de almacenamiento más antigua de la misma etiqueta, se eliminarán antes de ejecutar una nueva instantánea. El número que proporcione como último parámetro de la llamada es el que puede usar para controlar el número de instantáneas que se conservan. Con este número también puede controlar indirectamente el espacio en disco que se usa para las instantáneas. 

> [!NOTE]
>En cuanto cambie la etiqueta, el recuento se reinicia. El etiquetado se debe realizar de forma estricta, con el fin de que las instantáneas no se eliminen por accidente.

## <a name="snapshot-strategies"></a>Estrategias de instantánea
La frecuencia de las instantáneas de los distintos tipos depende de si usa, o no, la funcionalidad de recuperación ante desastres de HANA (instancias grandes). Esta funcionalidad confía en instantáneas de almacenamiento, lo que podría requerir recomendaciones especiales en cuanto a la frecuencia y los períodos de ejecución de las instantáneas de almacenamiento. 

En las consideraciones y recomendaciones siguientes, se supone que *no* se usa la funcionalidad de recuperación ante desastres que ofrece HANA (instancias grandes). En su lugar, las instantáneas de almacenamiento se usan como medio para tener copias de seguridad y poder proporcionar la recuperación a un momento dado durante los últimos 30 días. Dadas las limitaciones del número de instantáneas y el espacio, los clientes han considerado los siguientes requisitos:

- El tiempo de recuperación para una recuperación a un momento dado.
- El espacio utilizado.
- Los objetivos de punto de recuperación y tiempo de recuperación para una posible recuperación en caso de desastre.
- La ejecución eventual de copias de seguridad completas de la base de datos de HANA en discos. Cada vez que se realiza una copia de seguridad completa de la base de datos en discos o la interfaz de **backint**, se produce un error en la ejecución de las instantáneas de almacenamiento. Si planea ejecutar copias de seguridad de la base de datos completa sobre las instantáneas del almacenamiento, asegúrese de que la ejecución de dichas instantáneas está deshabilitada durante este tiempo.
- El número de instantáneas por volumen (está limitado a 250).


En el caso de los clientes que no usan la funcionalidad de recuperación ante desastres de las instancias grandes de HANA, el período de la instantánea es menos frecuente. En dichos casos, los clientes realizan las instantáneas combinadas en /hana/data y /hana/shared (incluye /usr/sap) en períodos de 12 o 24 horas y conservan las instantáneas durante un mes. Lo mismo ocurre con las instantáneas del volumen de copia de seguridad del registro. Sin embargo, la ejecución de las copias de seguridad del registro de transacciones de SAP HANA en el volumen de la copia de seguridad del registro tiene lugar en períodos de cinco a quince minutos.

Las instantáneas de almacenamiento programadas se realizan mejor mediante cron. Use el mismo script para todas sus necesidades de copia de seguridad y recuperación ante desastres, y modifique los datos de entrada del script con los distintos tiempos de copia de seguridad necesarios. Todas estas instantáneas se programan de forma diferente en cron en función de su tiempo de ejecución: cada hora, cada 12 horas, a diario o semanalmente. 

Este es un ejemplo de una programación de cron en /etc/crontab:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup.pl logs regularlogback 3min 28
22 12 * * *  ./azure_hana_backup.pl logs dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
En el ejemplo anterior, se trata de una instantánea combinada por hora que abarca los volúmenes que contienen las ubicaciones /hana/data y /hana/shared (incluye /usr/sap). Use este tipo de instantánea para una agilizar la recuperación a un momento dado en los dos últimos días. Además, en esos volúmenes hay una instantánea diaria. Por tanto, tiene dos días de cobertura por instantáneas por hora más cuatro semanas de cobertura por instantáneas diarias. Además, se realizar una copia de seguridad diaria del volumen de la copia de seguridad del registro de transacciones. Estas copias de seguridad también se conservan durante cuatro semanas. Como se ve en la tercera línea de crontab, la copia de seguridad del registro de transacciones de HANA está programada para ejecutarse cada 5 minutos. Las horas de inicio de los distintos trabajos de cron que ejecutan las instantáneas de almacenamiento están escalonadas, por lo que las instantáneas no se ejecutan todas a la vez en un momento dado. 

En el ejemplo siguiente, cada hora se realiza una instantánea combinada que abarca los volúmenes que contienen las ubicaciones /hana/data y /hana/shared (incluido /usr/sap). Las instantáneas se conservan durante dos días. Las instantáneas de los volúmenes de copia de seguridad del registro de transacciones se ejecutan cada 5 minutos y se conservan durante 4 horas. Como antes, la copia de seguridad del archivo de registro de transacciones de HANA está programada para ejecutarse cada cinco minutos. La instantánea del volumen de la copia de seguridad del registro de transacciones se realiza con un retraso de 2 minutos una vez que se ha iniciado la copia de seguridad del registro de transacciones. En circunstancias normales, la copia de seguridad del registro de transacciones de SAP HANA debería finalizar en esos dos minutos. Como antes, una instantánea de almacenamiento crea una copia de seguridad al día del volumen que contiene el LUN de arranque y se conserva durante cuatro semanas.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup.pl logs regularlogback 3min 28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl logs logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

En el siguiente gráfico se ilustran las secuencias del ejemplo anterior, excepto el LUN de arranque:

![Relación entre copias de seguridad e instantáneas](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA realiza escrituras normales en el volumen /hana/log para documentar los cambios confirmados en la base de datos. De manera regular, SAP HANA escribe un punto de retorno en el volumen /hana/data. Tal como se especifica en crontab, se ejecuta una copia de seguridad del registro de transacciones de SAP HANA cada 5 minutos. También se ve que una instantánea de SAP HANA se ejecuta cada hora como resultado del desencadenamiento de una instantánea de almacenamiento combinada sobre los volúmenes /hana/data y /hana/shared. Una vez que la instantánea de HANA se realiza correctamente, se ejecuta la instantánea de almacenamiento combinada. Tal como se indica en crontab, la instantánea de almacenamiento del volumen /hana/logbackup se ejecuta cada 5 minutos, alrededor de 2 minutos después de la copia de seguridad del registro de transacciones de HANA.

> 

>[!IMPORTANT]
> El uso de instantáneas de almacenamiento para las copias de seguridad de SAP HANA solo resulta valioso cuando las instantáneas se realizan junto con las copias de seguridad del registro de transacciones de SAP HANA. Es preciso que dichas copias de seguridad del registro de transacciones cubran los períodos entre las instantáneas de almacenamiento. 

Si ha adquirido un compromiso a los usuarios de una recuperación a un momento dado de 30 días, necesita lo siguiente:

- En casos extremos, acceder a una instantánea de almacenamiento combinada en /hana/data y /hana/shared, que tenga una antigüedad de 30 días.
- Tenga copias de seguridad del registro de transacciones contiguas que cubran el tiempo entre cualquiera de las instantáneas del almacenamiento combinado. Por tanto, la instantánea de mayor antigüedad del volumen de copia de seguridad del registro de transacciones debe tener 30 días. Este no es el caso si copia las copias de seguridad del registro de transacciones en otro recurso compartido NFS ubicado en Azure Storage. En ese caso, podría extraer las copias de seguridad del registro de transacciones anteriores de dicho recurso compartido.

Para beneficiarse de las instantáneas de almacenamiento y la eventual replicación de almacenamiento de las copias de seguridad del registro de transacciones, es preciso cambiar la ubicación en la que SAP HANA escribe las copias de seguridad del registro de transacciones. Puede realizar este cambio en HANA Studio. Si bien SAP HANA crea automáticamente copias de seguridad de segmentos de registros completos, se debe especificar un intervalo de copia de seguridad de registros para que sea determinista. Esto debe hacerse así especialmente cuando se usa la opción de recuperación ante desastres, ya que lo habitual es que desee ejecutar las copias de seguridad del registro de transacciones con un período determinista. En el caso siguiente, se tomó un intervalo de copia de seguridad de registros de 15 minutos.

![Programe los registros de copia de seguridad de SAP HANA en SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

También puede elegir que las copias de seguridad se realicen con una frecuencia superior a 15 minutos. A menudo este valor se usa en conjunción con la funcionalidad de recuperación ante desastres de HANA (instancias grandes). Algunos clientes realizan copias de seguridad del registro de transacciones cada 5 minutos.  

Si nunca se realizó una copia de seguridad de la base de datos, el último paso es crear una copia de seguridad en archivo para crear una entrada de copia de seguridad individual que debe existir en el catálogo de copias de seguridad. En caso contrario, SAP HANA no podrá iniciar las copias de seguridad del registro especificadas.

![Realizar una copia de seguridad en archivo para crear una entrada de copia de seguridad individual](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Después de que se hayan ejecutado correctamente las primeras instantáneas de almacenamiento, puede eliminar la instantánea de prueba que se ejecutó en el paso 6. Para ello, ejecute el script `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl
```

A continuación puede ver un ejemplo de la salida del script:
```
Checking Snapshot Status for h80
**********************Checking access to Storage**********************
Storage Snapshot Access successful.
**********************Getting list of volumes that match HANA instance specified**********************
Collecting set of volumes hosting HANA matching pattern *h80* ...
Volume show completed successfully.
Adding volume hana_data_h80_mnt00001_t020_vol to the snapshot list.
Adding volume hana_log_backups_h80_t020_vol to the snapshot list.
Adding volume hana_shared_h80_t020_vol to the snapshot list.
**********************Adding list of snapshots to volume list**********************
Collecting set of snapshots for each volume hosting HANA matching pattern *h80* ...
**********************Displaying Snapshots by Volume**********************
hana_data_h80_mnt00001_t020_vol
Test_HANA_Snapshot.2018-02-06_1753.3
Test_HANA_Snapshot.2018-02-06_1815.2
….
Command completed successfully.
Exiting with return code: 0
Command completed successfully.
```


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Supervisión del número y tamaño de instantáneas del volumen de disco

En un volumen de almacenamiento concreto se puede supervisar tanto el número de instantáneas como el consumo de almacenamiento de dichas instantáneas. El comando `ls` no muestra los archivos o el directorio de instantáneas. Sin embargo, el comando `du` del sistema operativo Linux muestra detalles sobre esas instantáneas de almacenamiento debido a que se almacenan en los mismos volúmenes. El comando se puede usar con las opciones siguientes:

- `du –sh .snapshot`: esta opción proporciona el total de todas las instantáneas del directorio de instantáneas.
- `du –sh --max-depth=1`: esta opción enumera todas las instantáneas que se guardan en la carpeta **.snapshot** y el tamaño de cada una de ellas.
- `du –hc`: esta opción proporciona el tamaño total que usan todas las instantáneas.

Utilice estos comandos para asegurarse de que las instantáneas que tomadas y almacenadas no consumen todo el almacenamiento de los volúmenes.

>[!NOTE]
>Las instantáneas del LUN de arranque no son visibles con los comandos anteriores.

### <a name="getting-details-of-snapshots"></a>Detalles de las instantáneas
Para más detalles sobre las instantáneas, también puede utilizar el script `azure_hana_snapshot_details.pl`. Este script se puede ejecutar en cualquier ubicación si hay un servidor activo en la ubicación de recuperación ante desastres. El script proporciona la siguiente salida desglosada por cada volumen que contiene instantáneas: 
   * El tamaño de las instantáneas totales de un volumen
   * Los siguientes detalles de cada instantánea del volumen: 
      - Nombre de la instantánea 
      - Hora de creación 
      - Tamaño de la instantánea
      - Frecuencia de la instantánea
      - Identificador de copia de seguridad de HANA asociado con esa instantánea, si procede

A continuación puede ver un ejemplo de la sintaxis de la ejecución del script:

```
./azure_hana_snapshot_details.pl 
```

Dado que el script intenta recuperar el identificador de copia de seguridad de HANA, necesita conectarse a la instancia de SAP HANA. Esta conexión requiere que se establezca correctamente el archivo de configuración *HANABackupCustomerDetails.txt*. Una salida de dos instantáneas en un volumen podría ser similar a la siguiente:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```



### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Reducción del número de instantáneas en un servidor

Como ya se ha explicado, es posible reducir el número de etiquetas concretas almacenadas. Los dos últimos parámetros del comando para iniciar una instantánea son la etiqueta y el número de instantáneas que se desean conservar.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

En el ejemplo anterior, la etiqueta d instantánea es **dailyhana** y el número de instantáneas con esta etiqueta que se conservarán es **28**. A medida que responda al consumo de espacio en el disco es posible que desee reducir el número de instantáneas que se almacenan. La manera más sencilla de reducir el número de instantáneas a 15, por ejemplo, es ejecutar el script con el último parámetro establecido en **15**:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Si ejecuta el script con la configuración anterior, se reducen las instantáneas, con lo que el número de ellas, incluida la nueva instantánea del almacenamiento, es 15. Se conservan las 15 instantáneas más recientes y se eliminan las 15 más antiguas.

 >[!NOTE]
 > Este script reduce el número de instantáneas solo si hay instantáneas que tienen más de una hora de antigüedad. Este script no elimina las instantáneas que tienen menos de una hora de antigüedad. Estas restricciones están relacionadas con la funcionalidad opcional de recuperación ante desastres que se ofrece.

Si ya no desea mantener un conjunto de instantáneas con la etiqueta de copia de seguridad **hanadaily** en los ejemplos de sintaxis, puede ejecutar el script con **0** como número de retención. Todas las instantáneas que coincidan con esa etiqueta se eliminan. Sin embargo, la eliminación de todas las instantáneas puede afectar a las funcionalidades de recuperación ante desastres de HANA (instancias grandes).

Otra opción para eliminar instantáneas específicas es usar el script `azure_hana_snapshot_delete.pl`. Este script está diseñado para eliminar una instantánea o un conjunto de ellas, ya sea mediante el identificador de la copia de seguridad de HANA como aparece en HANA Studio, o bien a través del propio nombre de la instantánea. Actualmente, el identificador de la copia de seguridad solo está asociado a las instantáneas creadas para el tipo de instantánea **hana**. Las copias de seguridad de instantáneas del tipo **logs** y **boots** no realizan una instantánea de SAP HANA, por lo que no se puede encontrar un identificador de copia de seguridad de dichas instantáneas. Si se escribe el nombre de la instantánea, busca todas las instantáneas en los distintos volúmenes que coincidan con el nombre de instantánea que se escribió. 

Llame al script que necesita para especificar el SID de la instancia de HANA, para lo que debe usar la sintaxis de llamada del script:

```
./azure_hana_snapshot_delete.pl <SID>

```

Ejecute el script como usuario **raíz**.

Si selecciona una instantánea, puede eliminar cada instantánea individualmente. Primero suministra el volumen que contiene la instantánea y luego el nombre de la instantánea. Si la instantánea está en ese volumen y tiene más de una hora de antigüedad, se elimina. Puede ejecutar el script `azure_hana_snapshot_details` para buscar los nombres de los volúmenes y los nombres de las instantáneas. 

>[!IMPORTANT]
>Si hay datos que solo existen en la instantánea que se va a eliminar, cuando la instantánea se elimine se perderán para siempre.

  
## <a name="file-level-restore-from-a-storage-snapshot"></a>Restauración de nivel de archivo desde una instantánea de almacenamiento
En el caso de los tipos de instantánea **hana** y **logs**, es posible acceder a las instantáneas directamente en los volúmenes del directorio **.snapshot**. Hay un subdirectorio para cada una de las instantáneas. Puede copiar cada archivo en que estaba en el momento de la instantánea desde ese subdirectorio a la estructura de directorios real. En la versión actual del script, **NO** se proporciona script de restauración para la restauración de instantáneas como una característica de autoservicio (aunque la restauración de instantáneas se puede realizar como parte de los scripts de recuperación ante desastres de autoservicio en el sitio de recuperación ante desastres durante la conmutación por error). Debe ponerse en contacto con el equipo de operaciones de Microsoft; para ello, abra una solicitud de servicio para restaurar la instantánea deseada entre las instantáneas disponibles existentes.

>[!NOTE]
>Una sola restauración de archivos no funciona con las instantáneas del LUN de arranque, que es independiente del tipo de las unidades de instancias grandes de HANA. El directorio **.snapshot** no se expone en el LUN de arranque. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>Recuperación a la instantánea más reciente de HANA

En un escenario de bajada de producción, el proceso de recuperación a partir de una instantánea de almacenamiento se puede iniciar como un incidente de cliente en el Soporte técnico de Microsoft Azure. Es una cuestión muy urgente si los datos se eliminaran en un sistema de producción y la única forma de volver a obtenerlos fuera a través de la restauración de la base de datos de producción.

En otra situación, una recuperación a un momento dado podría ser poco urgente y se podría planear con días de antelación. Puede planear la recuperación con SAP HANA en Azure en lugar de generar una marca de alta prioridad. Por ejemplo, podría estar pensando en actualizar el software de SAP mediante la aplicación de un nuevo paquete de mejora. A continuación, necesita volver a una instantánea que representa el estado antes de la actualización del paquete mejora.

Antes de enviar la solicitud, necesita prepararse. SAP HANA en el equipo de Azure, a continuación, puede controlar la solicitud y proporcionar los volúmenes restaurados. A partir de ese momento, restaurará la base de datos de HANA con las instantáneas. 

A continuación se muestra cómo prepararse para la solicitud:

>[!NOTE]
>La interfaz de usuario podría variar con respecto a las capturas de pantalla siguientes, en función de la versión de SAP HANA que use.

1. Decida qué instantánea desea restaurar. Salvo que se indique lo contrario, solo se restaura el volumen de datos/HANA. 

1. Cierre la instancia de HANA.

   ![Cerrar la instancia de HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Desmonte los volúmenes de datos de todos los nodos de la base de datos de HANA. Si los volúmenes de datos siguen montados en el sistema operativo, se produce un error en la restauración de la instantánea.
   ![Desmontar los volúmenes de datos de todos los nodos de la base de datos de HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Abra una solicitud de soporte técnico de Azure e incluya instrucciones acerca de la restauración de una instantánea concreta.

   - Durante la restauración: SAP HANA en Azure podría pedirle que asista a una llamada de conferencia para garantizar la coordinación, comprobación y confirmación de que se restaure la instantánea de almacenamiento correcta. 

   - Después de la restauración: SAP HANA en Azure Service le avisa cuando se ha restaurado la instantánea de almacenamiento.

1. Una vez que el proceso de restauración se haya completado, vuelva a montar todos los volúmenes de datos.

   ![Volver a montar todos los volúmenes de datos](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

1. Seleccione las opciones de recuperación en SAP HANA Studio, en caso de que no aparezcan automáticamente al volver a conectarse a base de datos de HANA a través de HANA Studio. En el siguiente ejemplo se muestra una operación de restauración a la instantánea de HANA más reciente. Una instantánea de almacenamiento inserta una instantánea de HANA. Si restaura a la instantánea de almacenamiento más reciente, debe ser la instantánea de HANA más reciente. (Si restaura a una instantánea de almacenamiento más antigua, será preciso que encuentre la instantánea de HANA por la hora en que se tomó la instantánea de almacenamiento.)

   ![Seleccionar las opciones de recuperación en SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

1. Seleccione **Recover the database to a specific data backup or storage snapshot** (Recuperar la base en una instantánea del almacenamiento o de copia de seguridad de datos concreta).

   ![Ventana Specify Recovery Type (Especificar tipo de recuperación)](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

1. Seleccione **Specify backup without catalog** (Especificar copia de seguridad sin catálogo).

   ![Ventana Specify Backup Location (Especificar ubicación de copia de seguridad)](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

1. En la lista **Destination Type** (Tipo de destino), seleccione **Snapshot** (Instantánea).

   ![Ventana Specify the Backup to Recover (Especificar la copia de seguridad que se recuperará)](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

1. Seleccione **Finish** (Finalizar) para iniciar el proceso de recuperación.

    ![Seleccionar "Finish" (Finalizar) para iniciar el proceso de recuperación](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

1. La base de datos HANA se restaura y recupera en la instantánea de HANA incluida en la instantánea del almacenamiento.

    ![La base de datos HANA se restaura y recupera en la instantánea de HANA.](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>Recuperación al estado más reciente

El proceso siguiente restaura la instantánea de HANA que se incluye en la de almacenamiento. Luego, se restauran las copias de seguridad del registro de transacciones al estado más reciente de la base de datos antes de restaurar la instantánea de almacenamiento.

>[!IMPORTANT]
>Antes de continuar, asegúrese de que dispone de una cadena completa y contigua de copias de seguridad del registro de transacciones. Sin estas copias de seguridad, no podrá restaurar el estado actual de la base de datos.

1. Complete los pasos 1 a 6 de Recuperación a la instantánea más reciente de HANA.

1. Elija **Recover the database to its most recent state** (Recuperar la base de datos a su estado más reciente).

   ![Seleccione "Recover the database to its most recent state" (Recuperar la base de datos a su estado más reciente).](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

1. Especifique la ubicación de las copias de seguridad de registros de HANA más recientes. La ubicación tiene que contener todas las copias del registro de transacciones de HANA desde la instantánea de HANA al estado más reciente.

   ![Especifique la ubicación de las copias de seguridad de registros de HANA más recientes.](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

1. Elija una copia de seguridad como base desde la que recuperar la base de datos. En este ejemplo, la instantánea de HANA que aparece en la captura de pantalla es la instantánea de HANA que se incluyó en la instantánea de almacenamiento. 

   ![Elija una copia de seguridad como base desde la que recuperar la base de datos.](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

1. Desactive la casilla **Use Delta Backups** (Usar copias de seguridad diferenciales) si no existe entre el momento de la instantánea de HANA y el estado más reciente.

   ![Desactive la casilla "Use Delta Backups" (Usar copias de seguridad diferenciales) si no hay datos diferenciales.](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

1. En la pantalla de resumen, seleccione **Finish** (Finalizar) para iniciar el procedimiento de restauración.

   ![Haga clic en "Finish" (Finalizar) en la pantalla de resumen.](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Recuperación a otro momento dado
Para realizar la recuperación a un momento dado entre la instantánea de HANA (incluida en la instantánea del almacenamiento) y una que sea posterior a la recuperación a un momento dado de la instantánea de HANA, siga estos pasos:

1. Asegúrese de que dispone de todas las copias de seguridad del registro de transacciones desde la instantánea de HANA para la hora a la que desea recuperar.
1. Inicie el procedimiento de Recuperación al estado más reciente.
1. En el paso 2, en la ventana **Specify Recovery Type** (Especificar tipo de recuperación), seleccione **Recover the database to the following point in time** (Recuperar la base de datos al siguiente momento dado) y especifique el momento dado. 
1. Complete los pasos 3 a 6.

## <a name="monitor-the-execution-of-snapshots"></a>Supervisión de la ejecución de instantáneas

Cuando se usan las instantáneas de almacenamiento de HANA (instancias grandes), también es preciso supervisar su ejecución. El script que ejecuta una instantánea del almacenamiento escribe la salida en un archivo y, luego, lo guarda en la misma ubicación que los scripts Perl. Se escribe un archivo independiente para cada instantánea de almacenamiento. La salida de cada archivo muestra las distintas fases que ejecuta el script de la instantánea:

1. Busca los volúmenes que necesitan crear una instantánea.
1. Busca las instantáneas tomadas de estos volúmenes.
1. Elimina eventuales instantáneas existentes para ajustarse al número de instantáneas especificado.
1. Crea una instantánea de SAP HANA.
1. Cree la instantánea de almacenamiento en los volúmenes.
1. Elimina la instantánea de SAP HANA.
1. Cambia el nombre de la instantánea más reciente a **.0**.

La parte más importante del script cab identificada es esta parte:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
En este ejemplo puede ver la forma en que el script registra la creación de la instantánea de HANA. En el caso del escalado horizontal, este proceso se inicia en el nodo maestro. El nodo maestro inicia la creación sincrónica de las instantáneas de SAP HANA en cada uno de los nodos de trabajado. En ese momento se toma la instantánea de almacenamiento. Después de la ejecución correcta de las instantáneas del almacenamiento, se elimina la instantánea de HANA. La eliminación de la instantánea HANA se inicia desde el nodo maestro.


## <a name="next-steps"></a>Pasos siguientes
- Consulte [principios de la recuperación ante desastres y la preparación](hana-concept-preparation.md).
