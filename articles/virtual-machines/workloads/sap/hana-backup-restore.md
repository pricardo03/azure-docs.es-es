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
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c03a7e5763f580bf1e17232a5850064710c8227
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098327"
---
# <a name="backup-and-restore"></a>Copia de seguridad y restauración

>[!IMPORTANT]
>Esta documentación no sustituye a la documentación de administración de SAP HANA ni a las notas de SAP. Se espera que el lector tenga unos sólidos conocimientos de la administración y operaciones de SAP HANA, sobre todo en los temas relativos a la copia de seguridad, restauración, alta disponibilidad y recuperación ante desastres. En esta documentación, se muestran capturas de pantalla de SAP HANA Studio. El contenido, la estructura y la naturaleza de las pantallas de las herramientas de administración de SAP y de las herramientas en sí pueden cambiar de una versión a otra de SAP HANA.

Es importante que practique los pasos y procesos que se realizan en su entorno y con sus versiones de HANA. Algunos procesos descritos en esta documentación se han simplificado para mejorar la comprensión general y no están diseñados para utilizarse como pasos detallados en posibles manuales de uso. Si desea crear manuales de operaciones de sus configuraciones, debe probar y ejecutar los procesos, así como documentar aquellos relacionados con sus configuraciones concretas. 

Uno de los aspectos más importantes del funcionamiento de las bases de datos es protegerlas de eventos catastróficos. La causa de estos eventos pueden deberse a cualquier cosa, desde desastres naturales a simples errores del usuario.

La copia de seguridad de una base de datos, con la capacidad de realizar la restauración a cualquier momento dado (como antes de que alguien eliminara datos críticos), posibilita la restauración a un estado que sea lo más parecido posible al que había antes de que la interrupción.

Dos tipos de copias de seguridad se deben realizar para lograr una capacidad de restauración:

- Copias de seguridad de bases de datos: completas, incrementales o diferenciales
- Copias de seguridad del registro de transacciones

Además de las copias de seguridad completas de la base de datos que se realizan en un nivel de aplicación, puede realizar copias de seguridad con instantáneas de almacenamiento. Las instantáneas de almacenamiento no reemplazan las copias de seguridad del registro de transacciones. Las copias de seguridad del registro de transacciones siguen siendo importantes para restaurar la base de datos a un momento dado o para eliminar las transacciones ya confirmadas de los registros. Sin embargo, las instantáneas de almacenamiento pueden acelerar la recuperación al proporcionar rápidamente una imagen de puesta al día de la base de datos. 

SAP HANA en Azure (instancias grandes) ofrece dos opciones de copia de seguridad y restauración:

- Hágalo Vd. mismo. Después de asegurarse de que hay suficiente espacio en disco, realizar copias de seguridad completas de base de datos y registro mediante uno de los siguientes métodos de copia de seguridad de disco. Puede hacer copias de seguridad directamente en los volúmenes conectados a las unidades de HANA (instancias grandes) o en los recursos compartidos de archivos de red (NFS) configurados en una máquina virtual (VM) de Azure. En el último caso, los clientes configuran una VM Linux en Azure, adjuntan Azure Storage a dicha VM y comparten el almacenamiento a través de un servidor NFS configurado en esa máquina virtual. Si se realiza la copia de seguridad en los volúmenes que directamente se adjuntan a unidades de instancia grande de HANA, necesita copiar las copias de seguridad a una cuenta de Azure Storage (una vez que configure una VM de Azure que exporte los recursos compartidos NFS). También puede usar un almacén de Azure Backup o el almacenamiento en frío de Azure. 

   Otra opción consiste en usar una herramienta de protección de datos de terceros para almacenar las copias de seguridad una vez que se copian en una cuenta Azure Storage. Esta opción de copia de seguridad DIY también podría ser necesaria para aquellos datos que necesita almacenar durante períodos más largos de tiempo con fines de conformidad y auditoría. En todos los casos, las copias de seguridad se copian en recursos compartidos NFS que se representan a través de una VM y Azure Storage.

- Funcionalidad de copia de seguridad y restauración de infraestructura. También puede usar la funcionalidad de copia de seguridad y restauración que proporciona la infraestructura subyacente de SAP HANA en Azure (instancias grandes). Esta opción cumple con la necesidad de copias de seguridad y restauraciones rápidas. En el resto de esta sección se trata la funcionalidad de copia de seguridad y restauración que se ofrece con las instancias grandes de HANA. Esta sección también trata la relación que la copia de seguridad y la restauración tienen con la funcionalidad de recuperación ante desastres que proporciona HANA (instancias grandes).

> [!NOTE]
>   La tecnología de instantáneas que utiliza la infraestructura subyacente de instancias grandes de HANA depende de las instantáneas de SAP HANA. En este momento, las instantáneas de SAP HANA no funcionan en conjunción con los distintos inquilinos de los contenedores de base de datos multiinquilino de SAP HANA. Si solo se implementa un inquilino, las instantáneas de SAP HANA funcionan y se puede usar este método.

## <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Uso de instantáneas de almacenamiento de SAP HANA en Azure (Instancias grandes)

La infraestructura de almacenamiento subyacente de SAP HANA en Azure (instancias grandes) admite instantáneas de almacenamiento de volúmenes. Se admite tanto la copia de seguridad como la restauración de volúmenes, pero es preciso tener en cuenta lo siguiente:

- En lugar de las copias de seguridad de bases de datos completas, se toman instantáneas del volumen de almacenamiento con frecuencia.
- Cuando se desencadena una instantánea sobre los volúmenes /hana/data y /hana/shared (incluye /usr/sap), la tecnología de la instantánea inicia una instantánea de SAP HANA antes de ejecutar la instantánea de almacenamiento. Esta instantánea de SAP HANA es el punto de configuración de las posibles restauraciones del registro después de la recuperación de la instantánea de almacenamiento. Instantánea de HANA se realice correctamente, necesita una instancia HANA activa.  En un escenario HSR, la instantánea de almacenamiento no se admite en el nodo secundario actual en el que no se puede realizar la instantánea de HANA.
- Después de que la instantánea de almacenamiento se haya ejecutado correctamente, se elimina la instantánea de SAP HANA.
- Las copias de seguridad del registro de transacciones se realizan con frecuencia y se almacenan en el volumen /hana/logbackups o en Azure. Puede desencadenar el volumen /hana/logbackups que contiene las copias de seguridad del registro de transacciones para tomar una instantánea por separado. En ese caso, no es preciso ejecutar una instantánea de HANA.
- Si debe restaurar una base de datos a un momento dado, solicitar ese soporte técnico de Microsoft Azure (para una interrupción de producción) o SAP HANA en Azure de la restauración a una instantánea de almacenamiento determinada. Un ejemplo es una restauración planeada de un sistema de espacio aislado a su estado original.
- La instantánea de SAP HANA que se incluye en la instantánea de almacenamiento es un punto de desplazamiento para aplicar las copias de seguridad del registro de transacciones que se han ejecutado y almacenado después de que se tomara la instantánea de almacenamiento.
- Estas copias de seguridad del registro de transacciones se realizan para restaurar la base de datos a un momento dado.

Puede realizar instantáneas de almacenamiento dirigidas a tres clases de volúmenes:

- Una instantánea combinada en /hana/data y /hana/shared (incluye /usr/sap). Esta instantánea requiere la creación de una instantánea de SAP HANA como preparación para la instantánea de almacenamiento. La instantánea de SAP HANA se asegura de que la base de datos esté en un estado coherente desde un punto de vista de almacenamiento, y de que se encuentre en un punto configurable para el proceso de restauración.
- Una instantánea independiente en /hana/logbackups.
- Una partición del sistema operativo.

Obtenga los scripts de instantáneas más recientes y la documentación en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Cuando descargue el paquete de secuencia de comandos de instantánea de la [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0), obtendrá tres archivos de los cuales uno es una documentación en formato PDF para la funcionalidad proporcionada. Asegúrese de que el usuario continua a lo largo de la instrucción en el capítulo 'Obtención de las herramientas de la instantánea' cuando descarga el conjunto de herramientas.

## <a name="storage-snapshot-considerations"></a>Consideraciones de las instantáneas de almacenamiento

>[!NOTE]
>Las instantáneas de almacenamiento consumen espacio de almacenamiento que se ha asignado a las unidades de instancia grande de HANA. Por tanto, es necesario tener en cuenta los siguientes aspectos de la programación de instantáneas de almacenamiento y del número de instantáneas que se mantienen. 

Mecánica concreta de las instantáneas de almacenamiento de SAP HANA en Azure (instancias grandes):

- Una instantánea de almacenamiento concreta (en el momento en que se toma) consume poco espacio de almacenamiento.
- Cuando el contenido de datos cambia y el contenido de los archivos de datos de SAP HANA cambia en el volumen de almacenamiento, la instantánea debe almacenar el contenido del bloque original, así como los cambios de datos.
- El resultado es que la instantánea de almacenamiento aumenta de tamaño. Cuanto más tiempo dure la instantánea, mayor será la instantánea de almacenamiento.
- Cuantos más cambios se realicen en el volumen de la base de datos de SAP HANA a lo largo de la vigencia de una instantánea de almacenamiento, mayor será el consumo de espacio de dicha instantánea.

SAP HANA en Azure (instancias grandes) incluye tamaños de volumen fijos para los volúmenes de datos y de registro de SAP HANA. Realizar instantáneas de esos volúmenes consume el espacio del volumen. Deberá determinar cuándo se deben programar las instantáneas de almacenamiento. También necesita supervisar el consumo de espacio de los volúmenes de almacenamiento, así como administrar el número de instantáneas que se almacenan. Puede deshabilitar las instantáneas de almacenamiento cuando importa masas de datos o realiza otros cambios importantes en la base de datos de HANA. 


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
1. Copie los scripts y el archivo de configuración de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) a la ubicación de **hdbsql** en la instalación de SAP HANA.
1. Modifique el archivo *HANABackupDetails.txt* tanto como sea necesario para ajustarse a las especificaciones apropiadas del cliente.

Obtenga los scripts de instantáneas más recientes y la documentación en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Para obtener pasos detallados enumerados anteriormente, consulte [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf)

### <a name="consideration-for-mcod-scenarios"></a>Consideraciones sobre los escenarios MCOD
Si ejecuta un [escenario MCOD](https://launchpad.support.sap.com/#/notes/1681092) con varias instancias de SAP HANA en una unidad HANA (instancias grandes), puede tener volúmenes de almacenamiento independientes aprovisionados para cada una de las instancias de SAP HANA. Para obtener más información sobre MDC y otras consideraciones, consulte [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) capítulo **'Cosas importantes que recordar'**.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Paso 1: Instalar el cliente de HDB de SAP HANA

El sistema operativo Linux instalado en SAP HANA en Azure (instancias grandes) incluye las carpetas y los scripts necesarios para ejecutar las instantáneas de almacenamiento de SAP HANA para la realización de copias de seguridad y para la recuperación ante desastres. Busque las versiones más recientes en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). La versión más reciente de las secuencias de comandos es 4.0. Los scripts podrían tener diferentes versiones secundarias dentro de la misma versión principal.

Es responsabilidad suya instalar el cliente de HDB de SAP HANA en las unidades de instancia grande de HANA durante la instalación de SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Paso 2: Cambiar /etcetera/ssh/ssh\_config

Este paso se describe detalladamente en la comprobación de las versiones más recientes en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) en el capítulo **"Habilitar la comunicación con el almacenamiento"**


### <a name="step-3-create-a-public-key"></a>Paso 3: Crear una clave pública

Para habilitar el acceso a las interfaces de las instantáneas de almacenamiento del inquilino de HANA (instancia grande), es preciso establecer procedimiento de un inicio de sesión a través de una clave pública. En el primer servidor de SAP HANA en Azure (instancias grandes) del inquilino, cree una clave pública que se use para acceder a la infraestructura de almacenamiento. La clave pública garantiza que no se necesita una contraseña para iniciar sesión en las interfaces de instantánea de almacenamiento. La creación de una clave pública también significa que no es necesario mantener credenciales de contraseña. Cómo generar el público exactamente los pasos clave que se describe en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) en el capítulo **'Habilitar comunicación con el almacenamiento'**


### <a name="step-4-create-an-sap-hana-user-account"></a>Paso 4: Crear una cuenta de usuario de SAP HANA

Para iniciar la creación de instantáneas de SAP HANA, es necesario crear una cuenta de usuario en SAP HANA, que los scripts de instantánea de almacenamiento puedan usar. Cree una cuenta de usuario de SAP HANA en SAP HANA Studio para esta finalidad. El usuario debe crearse en SYSTEMDB y NO en la base de datos de SID para MDC. En el entorno de contenedor único, el usuario se crea en la base de datos de inquilino. Esta cuenta debe tener los siguientes privilegios: **Administrador de la copia de seguridad** y **Lectura del catálogo**. Para que conocer los pasos exactos configurar el usuario y usar el usuario, lea [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) en el capítulo **'Habilitar la comunicación con SAP HANA'**


### <a name="step-5-authorize-the-sap-hana-user-account"></a>Paso 5: Autorizar la cuenta de usuario de SAP HANA

En este paso se autoriza a la cuenta de usuario de SAP HANA que creó, de manera que los scripts no necesitan enviar contraseñas en tiempo de ejecución. El comando de SAP HANA `hdbuserstore` habilita la creación de una clave de usuario de SAP HANA, que se almacena en uno o varios nodos de SAP HANA. La clave de usuario permite que el usuario acceda a SAP HANA sin tener que administrar contraseñas mediante el proceso de scripting. El proceso de scripting se describe más adelante en este mismo artículo.

>[!IMPORTANT]
>Ejecute estos comandos de configuración con los comandos de instantánea se ejecutan en el mismo contexto de usuario. En caso contrario, los comandos de instantánea no funcionan correctamente.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Paso 6: Obtener los scripts de instantánea, configurar las instantáneas y probar la configuración y la conectividad

Descargue la versión más reciente de los scripts de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). La manera en que las secuencias de comandos que se van a instalar ha cambiado majorly con la versión 4.0 de las secuencias de comandos. Para obtener los detalles exactos, lea [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) en el capítulo **'Habilitar la comunicación con SAP HANA'**

Para la secuencia exacta de comandos, lea el capítulo **'Facilita la instalación de herramientas de instantánea (valor predeterminado)'** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Se recomienda el uso de la instalación predeterminada. Si desea actualizar desde versión 3.x a 4.0, consulte la sección **'Actualizar una instalación existente'** de [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Para desinstalar el conjunto de 4.0 herramientas, siga las instrucciones de **'De la desinstalación de las herramientas de la instantánea'** en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

No olvide ejecutar los pasos descritos en **'Completar la instalación de herramientas de la instantánea'** de [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

El propósito de los distintos scripts y archivos porque tiene instaladas se muestra y se detallan en **'¿cuáles son estas herramientas de la instantánea'?** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Antes de configurar las herramientas de la instantánea, asegúrese de que también configurado las ubicaciones de copia de seguridad de HANA y la configuración correctamente como se describe en **'Configuración de SAP HANA'** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

La configuración del conjunto de herramientas de instantánea se describe detalladamente en **'Archivo Config - HANABackupCustomerDetails.txt'** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="testing-connectivity-with-sap-hana"></a>Probando la conectividad con SAP HANA

Después de colocar todos los datos de configuración en el archivo *HANABackupCustomerDetails.txt* compruebe si las configuraciones son correctas para los datos de la instancia de HANA. Use el script `testHANAConnection`, que es independiente de una configuración de escalado vertical o escalado horizontal de SAP HANA.

Para obtener información detallada, consulte **'Comprobar la conectividad con SAP HANA - testHANAConnection'** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf)

#### <a name="testing-storage-connectivity"></a>Probando la conectividad de almacenamiento

El paso siguiente es comprobar la conectividad con el almacenamiento según los datos que proporciona en el archivo de configuración *HANABackupCustomerDetails.txt* y, después, ejecutar una instantánea de prueba. Antes de ejecutar el `azure_hana_backup` comando, debe ejecutar esta prueba. Aparece en la secuencia de comandos para esta prueba **'Comprobar la conectividad con almacenamiento - testStorageSnapshotConnection'** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Después de un inicio de sesión correcto en las interfaces de la máquina virtual de almacenamiento, el script pasa a la fase 2 y crea una instantánea de prueba. El resultado se muestra aquí para una configuración de escalado horizontal de tres nodos de SAP HANA:

Si la instantánea de prueba se ha ejecutado correctamente con la secuencia de comandos, puede continuar con la programación de las instantáneas de almacenamiento real. Si el resultado no es satisfactorio, investigue los problemas antes de continuar. Se debe conservar la instantánea de prueba hasta que se hayan realizado las primeras instantáneas reales.


### <a name="step-7-perform-snapshots"></a>Paso 7: Realizar instantáneas

Cuando haya terminado los pasos de preparación, puede empezar a configurar y programar las instantáneas de almacenamiento real. El script que se va a programar funciona con configuraciones de escalado vertical o escalado horizontal de SAP HANA. Para ejecutar el script de copia de seguridad de forma periódica, prográmelo mediante la utilidad cron. 

La sintaxis de comando exacto y la funcionalidad, lea **'Realizar copia de seguridad instantánea - azure_hana_backup'** del documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).  

La ejecución del script `azure_hana_backup` crea la instantánea de almacenamiento en las tres fases siguientes:

1. Ejecuta una instantánea de SAP HANA
1. Ejecuta una instantánea de almacenamiento
1. Quita la instantánea de SAP HANA que se creó antes de la ejecución de la instantánea de almacenamiento

Para ejecutar el script, llámelo desde la carpeta ejecutable de HDB en la que se copió. 

El período de retención se administra con el número de instantáneas que se envían como parámetro al ejecutar el script. La cantidad de tiempo que cubren las instantáneas de almacenamiento es una función del período de ejecución y del número de instantáneas que se envían como parámetro al ejecutar el script. Si el número de instantáneas que se mantienen supera el número del parámetro de la llamada del script, la instantánea de almacenamiento más antigua de la misma etiqueta, se eliminarán antes de ejecutar una nueva instantánea. El número que proporcione como último parámetro de la llamada es el que puede usar para controlar el número de instantáneas que se conservan. Con este número también puede controlar indirectamente el espacio en disco que se usa para las instantáneas. 


## <a name="snapshot-strategies"></a>Estrategias de instantánea
La frecuencia de las instantáneas de los distintos tipos depende de si usa, o no, la funcionalidad de recuperación ante desastres de HANA (instancias grandes). Esta funcionalidad confía en instantáneas de almacenamiento, lo que podría requerir recomendaciones especiales en cuanto a la frecuencia y los períodos de ejecución de las instantáneas de almacenamiento. 

En las consideraciones y recomendaciones siguientes, se supone que *no* se usa la funcionalidad de recuperación ante desastres que ofrece HANA (instancias grandes). En su lugar, las instantáneas de almacenamiento se usan como medio para tener copias de seguridad y poder proporcionar la recuperación a un momento dado durante los últimos 30 días. Dadas las limitaciones del número de instantáneas y el espacio, los clientes han considerado los siguientes requisitos:

- El tiempo de recuperación para una recuperación a un momento dado.
- El espacio utilizado.
- Los objetivos de punto de recuperación y tiempo de recuperación para una posible recuperación en caso de desastre.
- La ejecución eventual de copias de seguridad completas de la base de datos de HANA en discos. Cada vez que se realiza una copia de seguridad completa de la base de datos en discos o la interfaz de **backint**, se produce un error en la ejecución de las instantáneas de almacenamiento. Si planea ejecutar copias de seguridad de la base de datos completa sobre las instantáneas del almacenamiento, asegúrese de que la ejecución de dichas instantáneas está deshabilitada durante este tiempo.
- El número de instantáneas por volumen (está limitado a 250).

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

En el caso de los clientes que no usan la funcionalidad de recuperación ante desastres de las instancias grandes de HANA, el período de la instantánea es menos frecuente. En dichos casos, los clientes realizan las instantáneas combinadas en /hana/data y /hana/shared (incluye /usr/sap) en períodos de 12 o 24 horas y conservan las instantáneas durante un mes. Lo mismo ocurre con las instantáneas del volumen de copia de seguridad del registro. Sin embargo, la ejecución de copias de seguridad del registro de transacciones de SAP HANA en el volumen de copia de seguridad del registro tiene lugar en períodos de cinco a 15 minutos.

Las instantáneas de almacenamiento programadas se realizan mejor mediante cron. Use el mismo script para todas sus necesidades de copia de seguridad y recuperación ante desastres, y modifique los datos de entrada del script con los distintos tiempos de copia de seguridad necesarios. Todas estas instantáneas se programan de forma diferente en cron en función de su tiempo de ejecución: cada hora, cada 12 horas, a diario o semanalmente. 

Este es un ejemplo de una programación de cron en /etc/crontab:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
En el ejemplo anterior, hay una instantánea combinada por hora que abarca los volúmenes que contienen el/hana/data y /hana/shared/SID (incluye/usr/sap) ubicaciones. Use este tipo de instantánea para una agilizar la recuperación a un momento dado en los dos últimos días. Además, en esos volúmenes hay una instantánea diaria. Por tanto, tiene dos días de cobertura por instantáneas por hora más cuatro semanas de cobertura por instantáneas diarias. Además, se realizar una copia de seguridad diaria del volumen de la copia de seguridad del registro de transacciones. Estas copias de seguridad también se conservan durante cuatro semanas. Como se ve en la tercera línea de crontab, la copia de seguridad del registro de transacciones de HANA está programada para ejecutarse cada 5 minutos. Las horas de inicio de los distintos trabajos de cron que ejecutan las instantáneas de almacenamiento están escalonadas, por lo que las instantáneas no se ejecutan todas a la vez en un momento dado. 

En el ejemplo siguiente, se realiza una instantánea combinada que abarca los volúmenes que contienen los/hana/data y /hana/shared/SID (incluido/usr/sap) ubicaciones cada hora. Las instantáneas se conservan durante dos días. Las instantáneas de los volúmenes de copia de seguridad del registro de transacciones se ejecutan cada 5 minutos y se conservan durante 4 horas. Como antes, la copia de seguridad del archivo de registro de transacciones de HANA está programada para ejecutarse cada cinco minutos. La instantánea del volumen de la copia de seguridad del registro de transacciones se realiza con un retraso de 2 minutos una vez que se ha iniciado la copia de seguridad del registro de transacciones. En circunstancias normales, la copia de seguridad del registro de transacciones de SAP HANA debería finalizar en esos dos minutos. Como antes, una instantánea de almacenamiento crea una copia de seguridad al día del volumen que contiene el LUN de arranque y se conserva durante cuatro semanas.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

En el siguiente gráfico se ilustran las secuencias del ejemplo anterior, excepto el LUN de arranque:

![Relación entre copias de seguridad e instantáneas](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA realiza escrituras normales en el volumen /hana/log para documentar los cambios confirmados en la base de datos. De manera regular, SAP HANA escribe un punto de retorno en el volumen /hana/data. Tal como se especifica en crontab, se ejecuta una copia de seguridad del registro de transacciones de SAP HANA cada 5 minutos. También verá que una instantánea de SAP HANA se ejecuta cada hora como resultado del desencadenamiento de una instantánea de almacenamiento combinada sobre los volúmenes/Hana/Data y /hana/shared/SID. Una vez que la instantánea de HANA se realiza correctamente, se ejecuta la instantánea de almacenamiento combinada. Tal como se indica en crontab, la instantánea de almacenamiento del volumen /hana/logbackup se ejecuta cada 5 minutos, alrededor de 2 minutos después de la copia de seguridad del registro de transacciones de HANA.

> 

>[!IMPORTANT]
> El uso de instantáneas de almacenamiento para las copias de seguridad de SAP HANA solo resulta valioso cuando las instantáneas se realizan junto con las copias de seguridad del registro de transacciones de SAP HANA. Es preciso que dichas copias de seguridad del registro de transacciones cubran los períodos entre las instantáneas de almacenamiento. 

Si ha adquirido un compromiso a los usuarios de una recuperación a un momento dado de 30 días, necesita lo siguiente:

- En casos extremos, acceder a un instantánea a través de/Hana/Data y /hana/shared/SID que es de 30 días de antigüedad del almacenamiento combinado.
- Tenga copias de seguridad del registro de transacciones contiguas que cubran el tiempo entre cualquiera de las instantáneas del almacenamiento combinado. Por tanto, la instantánea de mayor antigüedad del volumen de copia de seguridad del registro de transacciones debe tener 30 días. Este no es el caso si copia las copias de seguridad del registro de transacciones en otro recurso compartido NFS ubicado en Azure Storage. En ese caso, podría extraer las copias de seguridad del registro de transacciones anteriores de dicho recurso compartido.

Para beneficiarse de las instantáneas de almacenamiento y la eventual replicación de almacenamiento de las copias de seguridad del registro de transacciones, es preciso cambiar la ubicación en la que SAP HANA escribe las copias de seguridad del registro de transacciones. Puede realizar este cambio en HANA Studio. Si bien SAP HANA crea automáticamente copias de seguridad de segmentos de registros completos, se debe especificar un intervalo de copia de seguridad de registros para que sea determinista. Esto debe hacerse así especialmente cuando se usa la opción de recuperación ante desastres, ya que lo habitual es que desee ejecutar las copias de seguridad del registro de transacciones con un período determinista. En el caso siguiente, se tomó un intervalo de copia de seguridad de registros de 15 minutos.

![Programe los registros de copia de seguridad de SAP HANA en SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

También puede elegir que las copias de seguridad se realicen con una frecuencia superior a 15 minutos. A menudo este valor se usa en conjunción con la funcionalidad de recuperación ante desastres de HANA (instancias grandes). Algunos clientes realizan copias de seguridad del registro de transacciones cada 5 minutos.  

Si nunca se realizó una copia de seguridad de la base de datos, el último paso es crear una copia de seguridad en archivo para crear una entrada de copia de seguridad individual que debe existir en el catálogo de copias de seguridad. En caso contrario, SAP HANA no podrá iniciar las copias de seguridad del registro especificadas.

![Realizar una copia de seguridad en archivo para crear una entrada de copia de seguridad individual](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Después de ejecutar las primeras instantáneas de almacenamiento realizada correctamente, deberá eliminar la instantánea de prueba que se ejecutó en el paso 6. Lectura **'Quitar pruebas instantáneas - removeTestStorageSnapshot'** en el documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) para obtener más información. 


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Supervisión del número y tamaño de instantáneas del volumen de disco

En un volumen de almacenamiento concreto se puede supervisar tanto el número de instantáneas como el consumo de almacenamiento de dichas instantáneas. El comando `ls` no muestra los archivos o el directorio de instantáneas. Sin embargo, el comando `du` del sistema operativo Linux muestra detalles sobre esas instantáneas de almacenamiento debido a que se almacenan en los mismos volúmenes. El comando se puede usar con las opciones siguientes:

- `du –sh .snapshot`: esta opción proporciona el total de todas las instantáneas del directorio de instantáneas.
- `du –sh --max-depth=1`: esta opción enumera todas las instantáneas que se guardan en la carpeta **.snapshot** y el tamaño de cada una de ellas.
- `du –hc`: esta opción proporciona el tamaño total que usan todas las instantáneas.

Utilice estos comandos para asegurarse de que las instantáneas que tomadas y almacenadas no consumen todo el almacenamiento de los volúmenes.

>[!NOTE]
>Las instantáneas del LUN de arranque no son visibles con los comandos anteriores.

### <a name="getting-details-of-snapshots"></a>Detalles de las instantáneas
Para más detalles sobre las instantáneas, también puede utilizar el script `azure_hana_snapshot_details`. Este script se puede ejecutar en cualquier ubicación si hay un servidor activo en la ubicación de recuperación ante desastres. El script proporciona la siguiente salida desglosada por cada volumen que contiene instantáneas: 
   * El tamaño de las instantáneas totales de un volumen
   * Los siguientes detalles de cada instantánea del volumen: 
      - Nombre de la instantánea 
      - Hora de creación 
      - Tamaño de la instantánea
      - Frecuencia de la instantánea
      - Identificador de copia de seguridad de HANA asociado con esa instantánea, si procede

Para conocer la sintaxis de la comprobación de comando y salidas **'Enumerar instantáneas - azure_hana_snapshot_details para'** en el documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 



### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Reducción del número de instantáneas en un servidor

Como ya se ha explicado, es posible reducir el número de etiquetas concretas almacenadas. Los dos últimos parámetros del comando para iniciar una instantánea son la etiqueta y el número de instantáneas que se desean conservar.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

En el ejemplo anterior, la etiqueta d instantánea es **dailyhana** y el número de instantáneas con esta etiqueta que se conservarán es **28**. A medida que responda al consumo de espacio en el disco es posible que desee reducir el número de instantáneas que se almacenan. La manera más sencilla de reducir el número de instantáneas a 15, por ejemplo, es ejecutar el script con el último parámetro establecido en **15**:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Si ejecuta el script con la configuración anterior, se reducen las instantáneas, con lo que el número de ellas, incluida la nueva instantánea del almacenamiento, es 15. Se conservan las 15 instantáneas más recientes y se eliminan las 15 más antiguas.

 >[!NOTE]
 > Este script reduce el número de instantáneas solo si hay instantáneas que tienen más de una hora de antigüedad. Este script no elimina las instantáneas que tienen menos de una hora de antigüedad. Estas restricciones están relacionadas con la funcionalidad opcional de recuperación ante desastres que se ofrece.

Si ya no desea mantener un conjunto de instantáneas con el prefijo de copia de seguridad **dailyhana** en los ejemplos de sintaxis, puede ejecutar el script con **0** como el número de retención. Todas las instantáneas que coincidan con esa etiqueta se eliminan. Sin embargo, la eliminación de todas las instantáneas puede afectar a las funcionalidades de recuperación ante desastres de HANA (instancias grandes).

Otra opción para eliminar instantáneas específicas es usar el script `azure_hana_snapshot_delete`. Este script está diseñado para eliminar una instantánea o un conjunto de ellas, ya sea mediante el identificador de la copia de seguridad de HANA como aparece en HANA Studio, o bien a través del propio nombre de la instantánea. Actualmente, el identificador de la copia de seguridad solo está asociado a las instantáneas creadas para el tipo de instantánea **hana**. Las copias de seguridad de instantáneas del tipo **logs** y **boots** no realizan una instantánea de SAP HANA, por lo que no se puede encontrar un identificador de copia de seguridad de dichas instantáneas. Si se escribe el nombre de la instantánea, busca todas las instantáneas en los distintos volúmenes que coincidan con el nombre de instantánea que se escribió. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

Para obtener más información sobre la secuencia de comandos, consulte **'Eliminar una instantánea - azure_hana_snapshot_delete'** en el documento [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Ejecute el script como usuario **raíz**.

>[!IMPORTANT]
>Si hay datos que solo existen en la instantánea que se va a eliminar, cuando la instantánea se elimine se perderán para siempre.

  
## <a name="file-level-restore-from-a-storage-snapshot"></a>Restauración de nivel de archivo desde una instantánea de almacenamiento

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
En el caso de los tipos de instantánea **hana** y **logs**, es posible acceder a las instantáneas directamente en los volúmenes del directorio **.snapshot**. Hay un subdirectorio para cada una de las instantáneas. Puede copiar cada archivo en que estaba en el momento de la instantánea desde ese subdirectorio a la estructura de directorios real. En la versión actual de la secuencia de comandos, no hay **n** restaurar script proporcionado para la restauración de instantáneas como una característica de autoservicio (aunque la restauración de instantáneas se puede realizar como parte de la recuperación ante desastres de autoservicio de scripts en el sitio de recuperación ante desastres durante la conmutación por error). Debe ponerse en contacto con el equipo de operaciones de Microsoft; para ello, abra una solicitud de servicio para restaurar la instantánea deseada entre las instantáneas disponibles existentes.

>[!NOTE]
>Una sola restauración de archivos no funciona con las instantáneas del LUN de arranque, que es independiente del tipo de las unidades de instancias grandes de HANA. El directorio **.snapshot** no se expone en el LUN de arranque. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>Recuperación a la instantánea más reciente de HANA

En un escenario de bajada de producción, el proceso de recuperación a partir de una instantánea de almacenamiento se puede iniciar como un incidente de cliente en el Soporte técnico de Microsoft Azure. Es una cuestión muy urgente si los datos se eliminaran en un sistema de producción y la única forma de volver a obtenerlos fuera a través de la restauración de la base de datos de producción.

En otra situación, una recuperación a un momento dado podría ser poco urgente y se podría planear con días de antelación. Puede planear la recuperación con SAP HANA en Azure en lugar de generar una marca de alta prioridad. Por ejemplo, podría estar pensando en actualizar el software de SAP mediante la aplicación de un nuevo paquete de mejora. A continuación, necesita volver a una instantánea que representa el estado antes de la actualización del paquete mejora.

Antes de enviar la solicitud, necesita prepararse. SAP HANA en el equipo de Azure, a continuación, puede controlar la solicitud y proporcionar los volúmenes restaurados. A partir de ese momento, restaurará la base de datos de HANA con las instantáneas.

Las posibilidades para obtener una instantánea restaurada con el nuevo conjunto de herramientas se documentan en la sección **'Cómo restaurar una instantánea'** del documento [Guía de Manual de recuperación para SAP HANA en Azure desde la instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

A continuación se muestra cómo prepararse para la solicitud:

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



Otra posibilidad de obtener, por ejemplo, archivos de datos de SAP HANA se recuperó de una instantánea de almacenamiento está documentada en el paso 7 del documento [Guía de Manual de recuperación para SAP HANA en Azure desde la instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

El documento [Guía de Manual de recuperación para SAP HANA en Azure desde la instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf) ilustra la secuencia de restauración de copia de seguridad de instantánea. Use esa documentación para la ejecución de una restauración. 

>[!Note]
>Paso 7 no es necesario que se ejecutarán si obtuvo la instantánea puede restaurar las operaciones de Microsoft.


### <a name="recover-to-another-point-in-time"></a>Recuperación a otro momento dado
El documento [Guía de Manual de recuperación para SAP HANA en Azure desde la instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf) ilustra la secuencia de restauración a un momento dado en la sección **'Recuperar la base de datos en el siguiente punto dado'**. Use esa documentación para la ejecución de una restauración a un momento dado. 


## <a name="next-steps"></a>Pasos siguientes
- Consulte [principios de la recuperación ante desastres y la preparación](hana-concept-preparation.md).
