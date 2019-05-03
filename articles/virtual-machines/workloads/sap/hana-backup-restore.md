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
ms.openlocfilehash: 21232e5a678d6deed920e57cd0433a3b85ca4fdc
ms.sourcegitcommit: 60606c5e9a20b2906f6b6e3a3ddbcb6c826962d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2019
ms.locfileid: "64987913"
---
# <a name="backup-and-restore"></a>Copia de seguridad y restauración

>[!IMPORTANT]
>En este artículo no es un reemplazo para la documentación de administración de SAP HANA o notas de SAP. Esperamos que tienen un conocimiento sólido de y experiencia en administración de SAP HANA y las operaciones, especialmente para la copia de seguridad, restauración, alta disponibilidad y recuperación ante desastres. En este artículo, se muestran capturas de pantalla de SAP HANA Studio. El contenido, la estructura y la naturaleza de las pantallas de las herramientas de administración de SAP y de las herramientas en sí pueden cambiar de una versión a otra de SAP HANA.

Es importante que practique los pasos y procesos que se realizan en su entorno y con sus versiones de HANA. Algunos procesos descritos en este artículo se han simplificado para mejorar la comprensión general. No están diseñados para utilizarse como pasos detallados para posibles manuales de uso. Si desea crear manuales de uso para sus configuraciones, probar y ejecutar los procesos y documentar los procesos relacionados con sus configuraciones concretas. 

Uno de los aspectos más importantes del funcionamiento de las bases de datos es protegerlas de eventos catastróficos. La causa de estos eventos pueden deberse a cualquier cosa, desde desastres naturales a simples errores del usuario.

Copia de seguridad de una base de datos, con la capacidad de restaurar a cualquier punto de tiempo, como antes de que alguien eliminados datos críticos, habilita la restauración a un estado que sea lo más parecido posible a la forma en que estaba antes de la interrupción.

Dos tipos de copias de seguridad se deben realizar para lograr la capacidad de restaurar:

- Copias de seguridad de bases de datos: completas, incrementales o diferenciales
- Copias de seguridad del registro de transacciones

Además de las copias de seguridad completas de la base de datos que se realizan en un nivel de aplicación, puede realizar copias de seguridad con instantáneas de almacenamiento. Las instantáneas de almacenamiento no reemplazan las copias de seguridad del registro de transacciones. Las copias de seguridad del registro de transacciones siguen siendo importantes para restaurar la base de datos a un momento dado o para eliminar las transacciones ya confirmadas de los registros. Las instantáneas de almacenamiento pueden acelerar la recuperación al proporcionar rápidamente una imagen de puesta al día de la base de datos. 

SAP HANA en Azure (instancias grandes) ofrece dos opciones de copia de seguridad y restauración:

- **Hágalo usted mismo (implementación personal).** Después de asegurarse de que hay suficiente espacio en disco, realizar copias de seguridad de registro y base de datos completa mediante uno de los siguientes métodos de copia de seguridad de disco. Puede hacer copias de seguridad directamente en los volúmenes conectados a las unidades de instancia grande de HANA o a recursos compartidos NFS que se configuran en una máquina virtual (VM) de Azure. En el último caso, los clientes configuran una VM Linux en Azure, adjuntan Azure Storage a dicha VM y comparten el almacenamiento a través de un servidor NFS configurado en esa máquina virtual. Si realiza la copia de seguridad en los volúmenes que directamente se adjuntan a unidades de instancia grande de HANA, copie las copias de seguridad en una cuenta de almacenamiento de Azure. Puede hacerlo después de configurar una máquina virtual de Azure que exporte los recursos compartidos NFS que se basan en el almacenamiento de Azure. También puede usar un almacén de copia de seguridad de Azure o el almacenamiento en frío de Azure. 

   Otra opción es usar una herramienta de protección de datos de terceros para almacenar las copias de seguridad una vez que se copian en una cuenta de almacenamiento de Azure. La opción de copia de seguridad DIY también podría ser necesaria para los datos que necesita almacenar durante períodos más largos de tiempo para fines de auditoría y cumplimiento de normas. En todos los casos, las copias de seguridad se copian en recursos compartidos NFS que se representan a través de una VM y Azure Storage.

- **Infraestructura de copia de seguridad y restaurar la funcionalidad.** También puede usar la copia de seguridad y restaurar la funcionalidad que proporciona la infraestructura subyacente de SAP HANA en Azure (instancias grandes). Esta opción cumple con la necesidad de copias de seguridad y restauraciones rápidas. En el resto de esta sección se trata la funcionalidad de copia de seguridad y restauración que se ofrece con las instancias grandes de HANA. Esta sección también trata la relación de copia de seguridad y restauración al desastre que ofrece funcionalidad de recuperación de instancias grandes de HANA.

> [!NOTE]
>   La tecnología de instantáneas que se usa la infraestructura subyacente de instancias grandes de HANA tiene una dependencia en las instantáneas de SAP HANA. En este momento, las instantáneas de SAP HANA no funcionan en conjunción con varios inquilinos de contenedores de base de datos multiinquilino de SAP HANA. Si solo se implementa un inquilino, las instantáneas de SAP HANA funcionan y se puede usar este método.

## <a name="use-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Usar instantáneas de almacenamiento de SAP HANA en Azure (instancias grandes)

La infraestructura de almacenamiento subyacente de SAP HANA en Azure (instancias grandes) admite instantáneas de almacenamiento de volúmenes. Se admite tanto la copia de seguridad como la restauración de volúmenes, pero es preciso tener en cuenta lo siguiente:

- En lugar de las copias de seguridad de bases de datos completas, se toman instantáneas del volumen de almacenamiento con frecuencia.
- Cuando se desencadena una instantánea a través de/Hana/Data y/Hana/Shared, que incluye/usr/SAP, volúmenes, la tecnología de instantáneas se inicia una instantánea antes de SAP HANA ejecuta la instantánea de almacenamiento. Esta instantánea de SAP HANA es el punto de configuración de las posibles restauraciones del registro después de la recuperación de la instantánea de almacenamiento. Para que una instantánea de HANA se realice correctamente, necesita una instancia HANA activa. En un escenario HSR, una instantánea de almacenamiento no se admite en un nodo secundario actual donde no se puede realizar una instantánea de HANA.
- Después de la instantánea de almacenamiento se ejecuta correctamente, se elimina la instantánea de SAP HANA.
- Las copias de seguridad del registro de transacciones se realizan con frecuencia y se almacenan en el volumen hana/logbackups o en Azure. Puede desencadenar el volumen /hana/logbackups que contiene las copias de seguridad del registro de transacciones para tomar una instantánea por separado. En ese caso, no es necesario ejecutar una instantánea de HANA.
- Si debe restaurar una base de datos a un momento dado para una interrupción de producción, solicitar ese soporte técnico de Microsoft Azure o SAP HANA en Azure de la restauración a una instantánea de almacenamiento determinada. Un ejemplo es una restauración planeada de un sistema de espacio aislado a su estado original.
- La instantánea de SAP HANA que se incluye en la instantánea de almacenamiento es un punto de desplazamiento para aplicar las copias de seguridad de registro de transacciones que se ejecutan y se han almacenado después de que se tomó la instantánea de almacenamiento.
- Estas copias de seguridad del registro de transacciones se realizan para restaurar la base de datos a un momento dado.

Puede realizar instantáneas de almacenamiento que tienen como destino las tres clases de volúmenes:

- Una instantánea combinada en/hana/data y/hana/shared, que incluye/usr/SAP. Esta instantánea requiere la creación de una instantánea de SAP HANA como preparación para la instantánea de almacenamiento. La instantánea de SAP HANA, se garantiza que la base de datos está en un estado coherente desde un punto de vista de almacenamiento. Para el proceso de restauración, que es un punto para configurar en.
- Una instantánea independiente en /hana/logbackups.
- Una partición del sistema operativo.

Para obtener las secuencias de comandos de instantánea y la documentación más reciente, consulte [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Cuando descargue el paquete de secuencia de comandos de instantánea de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0), obtendrá tres archivos. Uno de los archivos se documenta en un archivo PDF para la funcionalidad proporcionada. Después de descargar el conjunto de herramientas, siga las instrucciones que aparecen en "obtener las herramientas de la instantánea".

## <a name="storage-snapshot-considerations"></a>Consideraciones de las instantáneas de almacenamiento

>[!NOTE]
>Las instantáneas de almacenamiento consumen espacio de almacenamiento que se asigna a las unidades de instancia grande de HANA. Tenga en cuenta los siguientes aspectos de la programación de instantáneas de almacenamiento y cuántas de esas instantáneas mantener. 

Mecánica concreta de las instantáneas de almacenamiento de SAP HANA en Azure (instancias grandes):

- En el momento que se toma una instantánea de almacenamiento concreta consume poco espacio de almacenamiento.
- Cuando los cambios de contenido de datos y el contenido de los archivos que cambian en el volumen de almacenamiento de datos de SAP HANA, la instantánea debe almacenar el contenido del bloque original y los cambios de datos.
- El resultado es que la instantánea de almacenamiento aumenta de tamaño. Cuanto más tiempo dure la instantánea, mayor será la instantánea de almacenamiento.
- Cuantos más cambios se realicen en el volumen de la base de datos de SAP HANA a lo largo de la vigencia de una instantánea de almacenamiento, mayor será el consumo de espacio de dicha instantánea.

SAP HANA en Azure (instancias grandes) incluye tamaños de volumen fijos para los volúmenes de datos y de registro de SAP HANA. Realizar instantáneas de esos volúmenes consume el espacio del volumen. Necesita:

- Determinar cuándo se deben programar las instantáneas de almacenamiento.
- Supervisar el consumo de espacio de los volúmenes de almacenamiento. 
- Administrar el número de instantáneas que se almacenan. 

Puede deshabilitar las instantáneas de almacenamiento cuando importa masas de datos o realiza otros cambios importantes en la base de datos de HANA. 


Las secciones siguientes proporcionan información para realizar estas instantáneas e incluyen recomendaciones generales:

- Aunque el hardware puede admitir 255 instantáneas por volumen, que desea no acercarse a este número. La recomendación es 250 o menos.
- Antes de realizar instantáneas de almacenamiento, supervise y realice un seguimiento del espacio disponible.
- Reduzca el número de instantáneas del almacenamiento en función del espacio libre. Puede reducir el número de instantáneas que mantiene, o ampliar los volúmenes. Puede solicitar más almacenamiento en unidades de un terabyte.
- Durante actividades como mover datos a SAP HANA con herramientas de migración de la plataforma SAP (R3load) o restaurar bases de datos de SAP HANA a partir de copias de seguridad, deshabilite las instantáneas de almacenamiento en el volumen /hana/data. 
- Durante reorganizaciones mayores de tablas de SAP HANA, si es posible evitar que las instantáneas de almacenamiento.
- Las instantáneas de almacenamiento son un requisito previo para sacar provecho de las funcionalidades de recuperación ante desastres de SAP HANA en Azure (instancias grandes).

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Requisitos previos para usar instantáneas de almacenamiento de autoservicio

Para asegurarse de que la secuencia de comandos de instantánea se ejecuta correctamente, asegúrese de que Perl está instalado en el sistema operativo Linux en el servidor de instancias grandes de HANA. Perl viene preinstalado en la unidad de instancia grande de HANA. Para comprobar la versión de Perl, utilice la siguiente línea de comandos:

`perl -v`

![La clave pública se copia ejecutando este comando](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Configuración de instantáneas de almacenamiento

Para configurar las instantáneas de almacenamiento con instancias grandes de HANA, siga estos pasos.
1. Asegúrese de que Perl está instalado en el sistema operativo Linux en el servidor de instancias grandes de HANA.
1. Modifique /etc/ssh/ssh\_config para agregar la línea _MACs hmac-sha1_.
1. Crear una cuenta de usuario de copia de seguridad de SAP HANA en el nodo maestro de cada instancia de SAP HANA que se ejecuta, si procede.
1. Instale el cliente HDB de SAP HANA en todos los servidores de instancias grandes de SAP HANA.
1. En el primer servidor de instancias grandes de SAP HANA de cada región, cree una clave pública para acceder a la infraestructura de almacenamiento subyacente que controla la creación de instantáneas.
1. Copie los scripts y el archivo de configuración de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) a la ubicación de **hdbsql** en la instalación de SAP HANA.
1. Modifique el archivo *HANABackupDetails.txt* tanto como sea necesario para ajustarse a las especificaciones apropiadas del cliente.

Obtenga los scripts de instantáneas más recientes y la documentación en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Para conocer los pasos enumerados anteriormente, consulte [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

### <a name="consideration-for-mcod-scenarios"></a>Consideraciones sobre los escenarios MCOD
Si ejecuta un [escenario MCOD](https://launchpad.support.sap.com/#/notes/1681092) con varias instancias de SAP HANA en una unidad de instancia grande de HANA, tiene volúmenes de almacenamiento independientes aprovisionados para cada una de las instancias de SAP HANA. Para obtener más información sobre MDC y otras consideraciones, consulte "Importante a recordar las cosas" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Paso 1: Instalar el cliente de HDB de SAP HANA

El sistema de operativo Linux instalado en SAP HANA en Azure (instancias grandes) incluye las carpetas y los scripts necesarios para ejecutar las instantáneas de almacenamiento de SAP HANA para la copia de seguridad y con fines de recuperación. Busque las versiones más recientes en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). La versión más reciente de las secuencias de comandos es 4.0. Los scripts podrían tener diferentes versiones secundarias dentro de la misma versión principal.

Es responsabilidad suya instalar al cliente HDB de SAP HANA en las unidades de instancia grande de HANA durante la instalación de SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Paso 2: Cambiar /etcetera/ssh/ssh\_config

Este paso se describe en "Habilitar la comunicación con el almacenamiento" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


### <a name="step-3-create-a-public-key"></a>Paso 3: Crear una clave pública

Para habilitar el acceso a las interfaces de instantánea de almacenamiento de su inquilino de instancia grande de HANA, establezca un procedimiento de inicio de sesión a través de una clave pública. 

En el primer SAP HANA en el servidor de Azure (instancias grandes) en el inquilino, cree una clave pública para tener acceso a la infraestructura de almacenamiento. Con una clave pública, no se requiere una contraseña para iniciar sesión en las interfaces de instantánea de almacenamiento. También no es necesario mantener las credenciales de contraseña con una clave pública. 

Para generar una clave pública, consulte "Habilitar la comunicación con el almacenamiento" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


### <a name="step-4-create-an-sap-hana-user-account"></a>Paso 4: Crear una cuenta de usuario de SAP HANA

Para iniciar la creación de instantáneas de SAP HANA, cree una cuenta de usuario en SAP HANA que se pueden usar los scripts de instantánea de almacenamiento. Cree una cuenta de usuario de SAP HANA en SAP HANA Studio para esta finalidad. El usuario debe crearse en SYSTEMDB y *no* en la base de datos de SID para MDC. En el entorno de contenedor único, el usuario se crea en la base de datos de inquilino. Esta cuenta debe tener **Backup Admin** y **catálogo lectura** privilegios. 

Para configurar y usar una cuenta de usuario, vea "Habilitar comunicación con SAP HANA" en [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0).


### <a name="step-5-authorize-the-sap-hana-user-account"></a>Paso 5: Autorizar la cuenta de usuario de SAP HANA

En este paso, autorizar a la cuenta de usuario de SAP HANA que creó para que los scripts no necesitan enviar contraseñas en tiempo de ejecución. El comando de SAP HANA `hdbuserstore` permite la creación de una clave de usuario de SAP HANA. La clave se almacena en uno o más nodos de SAP HANA. La clave de usuario permite que el usuario acceda a SAP HANA sin tener que administrar contraseñas mediante el proceso de scripting. El proceso de scripting se describe más adelante en este mismo artículo.

>[!IMPORTANT]
>Ejecute estos comandos de configuración con el mismo contexto de usuario que se ejecutan los comandos de instantánea en. En caso contrario, los comandos de instantánea no funcionan correctamente.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Paso 6: Obtener los scripts de instantánea, configurar las instantáneas y probar la configuración y la conectividad

Descargue la versión más reciente de los scripts de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Puede cambiar la manera en que se instalan las secuencias de comandos con la versión 4.0 de las secuencias de comandos. Para obtener más información, vea "Habilitar comunicación con SAP HANA" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Para la secuencia exacta de comandos, vea "Instalación fácil de herramientas de instantánea (valor predeterminado)" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Se recomienda el uso de la instalación predeterminada. 

Para actualizar desde la versión 3.x a 4.0, vea "Actualizar una instalación existente" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Para desinstalar el conjunto de 4.0 herramientas, consulte "Desinstalación de las herramientas de la instantánea" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

No se olvide de seguir los pasos descritos en "Completar la instalación de herramientas de la instantánea" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

El propósito de los distintos scripts y archivos porque tiene instaladas se describe en "¿Cuáles son estas herramientas instantánea?" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Antes de configurar las herramientas de la instantánea, asegúrese de que también configurar las ubicaciones de copia de seguridad de HANA y la configuración correctamente. Para obtener más información, consulte "Configuración de SAP HANA" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

La configuración del conjunto de herramientas de instantánea se describe en "Archivo de configuración - HANABackupCustomerDetails.txt" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="test-connectivity-with-sap-hana"></a>Probar la conectividad con SAP HANA

Después de colocar todos los datos de configuración en el archivo *HANABackupCustomerDetails.txt* compruebe si las configuraciones son correctas para los datos de la instancia de HANA. Use el script `testHANAConnection`, que es independiente de una configuración de escalado vertical o escalado horizontal de SAP HANA.

Para obtener más información, vea "Compruebe la conectividad con SAP HANA - testHANAConnection" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="test-storage-connectivity"></a>Probar la conectividad de almacenamiento

El paso siguiente es comprobar la conectividad con el almacenamiento en función de los datos se pone en la *HANABackupCustomerDetails.txt* archivo de configuración. A continuación, ejecute una instantánea de prueba. Antes de ejecutar el `azure_hana_backup` comando, debe ejecutar esta prueba. Para la secuencia de comandos para esta prueba, consulte "Comprobación de conectividad con almacenamiento - testStorageSnapshotConnection" "en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Después de un inicio de sesión correcto en las interfaces de la máquina virtual de almacenamiento, el script pasa a la fase 2 y crea una instantánea de prueba. Aquí se muestra el resultado de una configuración de escalado horizontal de tres nodos de SAP HANA.

Si la instantánea de prueba se ejecuta correctamente con la secuencia de comandos, puede programar las instantáneas de almacenamiento real. Si no es correcta, investigue los problemas antes de continuar. Se debe conservar la instantánea de prueba hasta que se hayan realizado las primeras instantáneas reales.


### <a name="step-7-perform-snapshots"></a>Paso 7: Realizar instantáneas

Cuando haya terminado los pasos de preparación, puede empezar a configurar y programar las instantáneas de almacenamiento real. El script que se va a programar funciona con configuraciones de escalado vertical o escalado horizontal de SAP HANA. Para ejecutar el script de copia de seguridad de forma periódica, prográmelo mediante la utilidad cron. 

La sintaxis de comando exacto y la funcionalidad, vea "Realizar copia de seguridad instantánea - azure_hana_backup" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Cuando la secuencia de comandos `azure_hana_backup` se ejecuta, crea la instantánea de almacenamiento en los siguientes tres fases:

1. Se ejecuta una instantánea de SAP HANA.
1. Se ejecuta una instantánea de almacenamiento.
1. Elimina la instantánea de SAP HANA que se creó antes de que se ejecutó la instantánea de almacenamiento.

Para ejecutar el script, llámelo desde la carpeta ejecutable de HDB en la que se copia. 

El período de retención se administra con el número de instantáneas que se envían como parámetro al ejecutar el script. La cantidad de tiempo que cubren las instantáneas de almacenamiento es una función del período de ejecución y del número de instantáneas que se envían como parámetro cuando se ejecuta la secuencia de comandos. 

Si el número de instantáneas que se mantienen supera el número que se denomine como un parámetro de la llamada de la secuencia de comandos, se elimina la instantánea de almacenamiento más antigua de la misma etiqueta antes de que se ejecuta una nueva instantánea. El número que proporcione como último parámetro de la llamada es el que puede usar para controlar el número de instantáneas que se conservan. Con este número, también puede controlar indirectamente el espacio en disco que se usa para las instantáneas. 


## <a name="snapshot-strategies"></a>Estrategias de instantánea
La frecuencia de las instantáneas de los distintos tipos depende de si usa, o no, la funcionalidad de recuperación ante desastres de HANA (instancias grandes). Esta funcionalidad confía en instantáneas de almacenamiento, lo que podría requerir recomendaciones especiales en cuanto a la frecuencia y los períodos de ejecución de las instantáneas de almacenamiento. 

En las consideraciones y recomendaciones siguientes, se supone que *no* se usa la funcionalidad de recuperación ante desastres que ofrece HANA (instancias grandes). En su lugar, las instantáneas de almacenamiento se usan como medio para tener copias de seguridad y poder proporcionar la recuperación a un momento dado durante los últimos 30 días. Dadas las limitaciones del número de instantáneas y el espacio, tenga en cuenta los siguientes requisitos:

- El tiempo de recuperación para una recuperación a un momento dado.
- El espacio utilizado.
- Los objetivos de punto de recuperación y tiempo de recuperación para una posible recuperación en caso de desastre.
- La ejecución eventual de copias de seguridad completas de la base de datos de HANA en discos. Cada vez que se realiza una copia de seguridad completa de la base de datos en discos o la interfaz de **backint**, se produce un error en la ejecución de las instantáneas de almacenamiento. Si planea ejecutar copias de seguridad de base de datos completa sobre las instantáneas de almacenamiento, asegúrese de la ejecución de las instantáneas de almacenamiento está deshabilitada durante este tiempo.
- El número de instantáneas por volumen, que está limitado a 250.

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

Si no utiliza la funcionalidad de recuperación ante desastres de instancias grandes de HANA, el período de la instantánea es menos frecuente. En tales casos, realizar las instantáneas combinadas en/Hana/Data y/Hana/Shared, que incluye/usr/SAP, en períodos de 12 horas o 24 horas. Mantener las instantáneas de un mes. Lo mismo puede decirse de las instantáneas del volumen de copia de seguridad del registro. Se produce la ejecución de copias de seguridad del registro de transacciones de SAP HANA en el volumen de copia de seguridad del registro en 5 minutos a períodos de 15 minutos.

Las instantáneas de almacenamiento programadas se realizan mejor mediante cron. Usar el mismo script para todas las copias de seguridad y recuperación ante desastres. Modifique las entradas de script para que coincidan con los distintos tiempos de copia de seguridad solicitados. Estas instantáneas todas se programan de manera diferente en cron en función de su tiempo de ejecución. Puede ser cada hora, cada 12 horas, diariamente, semanalmente, etc. 

El ejemplo siguiente muestra una programación cron en/etc/crontab:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
En el ejemplo anterior, una instantánea combinada por hora abarca los volúmenes que contienen los/Hana/Data y /hana/shared/SID, que incluye/usr/SAP, las ubicaciones. Use este tipo de instantánea para una agilizar la recuperación a un momento dado en los dos últimos días. También hay un diarios en esos volúmenes de instantáneas. Por lo tanto, tiene dos días de cobertura por instantáneas por hora más cuatro semanas de cobertura por instantáneas diarias. El volumen de copia de seguridad del registro de transacciones también es una copia de seguridad diaria. Estas copias de seguridad se conservan durante cuatro semanas. 

Como se ve en la tercera línea de crontab, la copia de seguridad del registro de transacciones de HANA está programada para ejecutarse cada 5 minutos. Las horas de inicio de los distintos trabajos de cron que ejecutan las instantáneas de almacenamiento son escalonadas. De este modo, las instantáneas no se ejecutan todas a la vez en un punto determinado en el tiempo. 

En el ejemplo siguiente, realice una instantánea combinada que abarca los volúmenes que contienen las ubicaciones por hora/Hana/Data y /hana/shared/SID, que incluye/usr/SAP. Las instantáneas se conservan durante dos días. Las instantáneas de los volúmenes de copia de seguridad de registro de transacciones se ejecutan cada 5 minutos y se conservan durante cuatro horas. Como antes, la copia de seguridad del archivo de registro de transacciones de HANA está programada para ejecutarse cada 5 minutos. 

La instantánea del volumen de la copia de seguridad del registro de transacciones se realiza con un retraso de 2 minutos una vez que se ha iniciado la copia de seguridad del registro de transacciones. En circunstancias normales, finaliza la copia de seguridad del registro de transacciones de SAP HANA en esos dos minutos. Como antes, una instantánea de almacenamiento crea una copia de seguridad al día del volumen que contiene el LUN de arranque y se conserva durante cuatro semanas.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

El siguiente gráfico ilustra las secuencias del ejemplo anterior. Se excluye el LUN de arranque.

![Relación entre copias de seguridad e instantáneas](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA realiza escrituras normales en el volumen /hana/log para documentar los cambios confirmados en la base de datos. De manera regular, SAP HANA escribe un punto de retorno en el volumen /hana/data. Como se especifica en crontab, una copia de seguridad del registro de transacciones de SAP HANA ejecuta cada 5 minutos. 

También verá que una instantánea de SAP HANA ejecuta cada hora como resultado del desencadenamiento de un instantánea a través de los volúmenes/Hana/Data y /hana/shared/SID del almacenamiento combinado. Después de la instantánea de HANA se realiza correctamente, el almacenamiento combinado de instantáneas se ejecuta. Como se indica en crontab, la instantánea de almacenamiento en el volumen/Hana/logbackup ejecuta cada 5 minutos, alrededor de 2 minutos después de la copia de seguridad del registro de transacciones de HANA.

> 

>[!IMPORTANT]
> El uso de instantáneas de almacenamiento para las copias de seguridad de SAP HANA solo resulta valioso cuando las instantáneas se realizan junto con las copias de seguridad del registro de transacciones de SAP HANA. Es preciso que dichas copias de seguridad del registro de transacciones cubran los períodos entre las instantáneas de almacenamiento. 

Si ha adquirido un compromiso a los usuarios de una recuperación a un momento dado de 30 días, necesita lo siguiente:

- Obtener acceso a una instantánea de almacenamiento combinada en/Hana/Data y /hana/shared/SID que es de 30 días de antigüedad, en casos extremos. 
- Tenga copias de seguridad del registro de transacciones contiguas que cubran el tiempo entre cualquiera de las instantáneas del almacenamiento combinado. Por tanto, la instantánea de mayor antigüedad del volumen de copia de seguridad del registro de transacciones debe tener 30 días. Esto no es el caso si copia las copias de seguridad del registro de transacciones en otro recurso compartido NFS que se encuentra en el almacenamiento de Azure. En ese caso, podría extraer las copias de seguridad del registro de transacciones anteriores de dicho recurso compartido.

Para beneficiarse de las instantáneas de almacenamiento y la posible replicación de almacenamiento de copias de seguridad de registro de transacciones, cambiar la ubicación en la que SAP HANA escribe las copias de seguridad del registro de transacciones. Puede realizar este cambio en HANA Studio. 

Si bien SAP HANA realiza una copia de seguridad de segmentos de registros completos automáticamente, especifique un intervalo de copia de seguridad de registros para que sea determinista. Esto es especialmente cierto cuando se usa la opción de recuperación ante desastres porque normalmente desea ejecutar copias de seguridad de registro con un período determinista. En el caso siguiente, 15 minutos se establece como el intervalo de copia de seguridad de registros.

![Programe los registros de copia de seguridad de SAP HANA en SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

También puede elegir las copias de seguridad más frecuentes que cada 15 minutos. A menudo este valor se usa en conjunción con la funcionalidad de recuperación ante desastres de HANA (instancias grandes). Algunos clientes realizan copias de seguridad del registro de transacciones cada 5 minutos.

Si nunca se realizó una copia de seguridad de la base de datos, el último paso es crear una copia de seguridad en archivo para crear una entrada de copia de seguridad individual que debe existir en el catálogo de copias de seguridad. En caso contrario, SAP HANA no se puede iniciar las copias de seguridad del registro especificado.

![Realizar una copia de seguridad en archivo para crear una entrada de copia de seguridad individual](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Después de ejecutan las primeras instantáneas de almacenamiento realizada correctamente, elimine la instantánea de prueba que se ejecutó en el paso 6. Para obtener más información, vea "Instantáneas de prueba de Remove - removeTestStorageSnapshot" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 


### <a name="monitor-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Supervisar el número y tamaño de las instantáneas de volumen de disco

En un volumen de almacenamiento concreto se puede supervisar tanto el número de instantáneas como el consumo de almacenamiento de dichas instantáneas. El comando `ls` no muestra los archivos o el directorio de instantáneas. El comando del sistema operativo Linux `du` muestra detalles sobre esas instantáneas de almacenamiento porque se almacenan en los mismos volúmenes. Use el comando con las siguientes opciones:

- `du –sh .snapshot`: esta opción proporciona el total de todas las instantáneas del directorio de instantáneas.
- `du –sh --max-depth=1`: esta opción enumera todas las instantáneas que se guardan en la carpeta **.snapshot** y el tamaño de cada una de ellas.
- `du –hc`: esta opción proporciona el tamaño total que usan todas las instantáneas.

Utilice estos comandos para asegurarse de que las instantáneas que tomadas y almacenadas no consumen todo el almacenamiento de los volúmenes.

>[!NOTE]
>Las instantáneas de lo LUN de arranque no son visibles con los comandos anteriores.

### <a name="get-details-of-snapshots"></a>Obtener detalles de las instantáneas
Para obtener más información acerca de las instantáneas, use el script `azure_hana_snapshot_details`. Puede ejecutar este script en cualquier ubicación si hay un servidor activo en la ubicación de recuperación ante desastres. El script proporciona la siguiente salida desglosada por cada volumen que contiene instantáneas: 
   * El tamaño de las instantáneas totales de un volumen
   * Los siguientes detalles de cada instantánea del volumen: 
      - Nombre de la instantánea 
      - Hora de creación 
      - Tamaño de la instantánea
      - Frecuencia de la instantánea
      - Identificador de copia de seguridad de HANA asociado con esa instantánea, si procede

Para conocer la sintaxis del comando y salidas, vea "Instantáneas de lista - azure_hana_snapshot_details para" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 



### <a name="reduce-the-number-of-snapshots-on-a-server"></a>Reducir el número de instantáneas en un servidor

Como se explicó anteriormente, puede reducir el número de etiquetas concretas almacenadas. Los dos últimos parámetros del comando para iniciar una instantánea son la etiqueta y el número de instantáneas que se desean conservar.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

En el ejemplo anterior, la etiqueta de la instantánea es **dailyhana**. Es el número de instantáneas con esta etiqueta que se mantienen **28**. A medida que responda al consumo de espacio en el disco es posible que desee reducir el número de instantáneas que se almacenan. Es una forma sencilla de reducir el número de instantáneas a 15, por ejemplo ejecutar el script con el último parámetro establecido en **15**:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Si ejecuta la secuencia de comandos con esta configuración, el número de instantáneas, que incluye la nueva instantánea de almacenamiento, es 15. Se conservan las 15 instantáneas más recientes y se eliminan las 15 más antiguas.

 >[!NOTE]
 > Este script reduce el número de instantáneas solo si hay más de una hora de antigüedad de instantáneas. El script no elimina las instantáneas que tienen menos de una horas de antigüedad. Estas restricciones están relacionadas con la funcionalidad opcional de recuperación ante desastres que se ofrece.

Si ya no desea mantener un conjunto de instantáneas con el prefijo de copia de seguridad **dailyhana** en los ejemplos de sintaxis, ejecute el script con **0** como el número de retención. A continuación, se quitan todas las instantáneas que coincidan con esa etiqueta. Quitar todas las instantáneas puede afectar a las funcionalidades de funcionalidad de recuperación ante desastres de instancias grandes de HANA.

Otra opción para eliminar instantáneas específicas es usar el script `azure_hana_snapshot_delete`. Este script está diseñado para eliminar una instantánea o un conjunto de instantáneas, ya sea mediante el identificador de la copia de seguridad de HANA como aparece en HANA Studio, o bien a través del nombre mismo de la instantánea. Actualmente, el identificador de la copia de seguridad solo está asociado a las instantáneas creadas para el tipo de instantánea **hana**. Las copias de seguridad del tipo de instantánea **registros** y **arranque** no realiza una instantánea de SAP HANA, así que no hay ningún identificador de copia de seguridad que se encuentran para esas instantáneas. Si se escribe el nombre de la instantánea, busca todas las instantáneas en los distintos volúmenes que coincidan con el nombre de instantánea que se escribió. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

Para obtener más información sobre la secuencia de comandos, vea "Eliminar una instantánea - azure_hana_snapshot_delete" en [herramientas de instantáneas de Microsoft para SAP HANA en Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Ejecute el script como usuario **raíz**.

>[!IMPORTANT]
>Si no hay datos que solo existen en la instantánea va a eliminar, después de elimina la instantánea, que los datos se perderán para siempre.


## <a name="file-level-restore-from-a-storage-snapshot"></a>Restauración de nivel de archivo desde una instantánea de almacenamiento

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
En el caso de los tipos de instantánea **hana** y **logs**, es posible acceder a las instantáneas directamente en los volúmenes del directorio **.snapshot**. Hay un subdirectorio para cada una de las instantáneas. Copiar cada archivo en el estado que tenía en el momento de la instantánea desde ese subdirectorio a la estructura de directorios real. 

En la versión actual de la secuencia de comandos, no hay *ningún* script proporcionado para la restauración de instantáneas como autoservicio de restauración. Restauración de instantáneas puede realizarse como parte de los scripts de recuperación ante desastres de autoservicio en el sitio de recuperación ante desastres durante la conmutación por error. Para restaurar una instantánea deseada de las instantáneas existentes disponibles, debe ponerse en contacto con el equipo de operaciones de Microsoft, abra una solicitud de servicio.

>[!NOTE]
>Solo archivo restauración no funciona para las instantáneas de la LUN independiente del tipo de las unidades de instancia grande de HANA de arranque. El **.snapshot** directorio no se expone en el LUN de arranque. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>Recuperación a la instantánea más reciente de HANA

En un escenario de bajada de producción, se puede iniciar el proceso de recuperación a partir de una instantánea de almacenamiento como un incidente de cliente con el soporte técnico de Microsoft Azure. Es una cuestión de urgencia altos si se han eliminado los datos en un sistema de producción y la única manera de recuperarla consiste en restaurar la base de datos de producción.

En otra situación, una recuperación a un momento dado podría ser poco urgente y se podría planear con días de antelación. Puede planear la recuperación con SAP HANA en Azure en lugar de generar una marca de alta prioridad. Por ejemplo, es posible que va a actualizar el software de SAP mediante la aplicación de un nuevo paquete de mejoras. A continuación, necesita volver a una instantánea que representa el estado antes de la actualización del paquete mejora.

Antes de enviar la solicitud, necesita prepararse. SAP HANA en el equipo de Azure, a continuación, puede controlar la solicitud y proporcionar los volúmenes restaurados. A partir de ese momento, restaurará la base de datos de HANA con las instantáneas.

Para las posibilidades de obtener una instantánea restaurada con el nuevo conjunto de herramientas, vea "Cómo restaurar una instantánea" en [Guía de recuperación Manual de SAP HANA en Azure desde una instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Para prepararse para la solicitud, siga estos pasos.

1. Decida qué instantánea desea restaurar. Salvo que se indique lo contrario, solo se restaura el volumen de datos/HANA. 

1. Cierre la instancia de HANA.

   ![Cerrar la instancia de HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Desmonte los volúmenes de datos de todos los nodos de la base de datos de HANA. Si los volúmenes de datos siguen montados en el sistema operativo, se produce un error en la restauración de la instantánea.

   ![Desmontar los volúmenes de datos de todos los nodos de la base de datos de HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Abra una solicitud de soporte técnico de Azure y se incluyen instrucciones sobre la restauración de una instantánea concreta:

   - Durante la restauración: SAP HANA en Azure Service podría pedirle que asista a una llamada de conferencia para coordinar, comprobar y confirmar que se restaure la instantánea de almacenamiento correcta. 

   - Después de la restauración: SAP HANA en Azure Service le avisa cuando se restaura la instantánea de almacenamiento.

1. Una vez que el proceso de restauración se haya completado, vuelva a montar todos los volúmenes de datos.

   ![Volver a montar todos los volúmenes de datos](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)



Otra posibilidad de obtener, por ejemplo, archivos de datos de SAP HANA se recuperó de una instantánea de almacenamiento, se documenta en el paso 7 [Guía de recuperación Manual de SAP HANA en Azure desde una instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Para restaurar una copia de seguridad de instantánea, vea [Guía de recuperación Manual de SAP HANA en Azure desde una instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf). 

>[!Note]
>Si se ha restaurado la instantánea de las operaciones de Microsoft, no es necesario realizar el paso 7.


### <a name="recover-to-another-point-in-time"></a>Recuperación a otro momento dado
Para restaurar a un momento dado, vea "Recuperar la base de datos en el siguiente punto dado" en [Guía de recuperación Manual de SAP HANA en Azure desde una instantánea de almacenamiento](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf). 


## <a name="next-steps"></a>Pasos siguientes
- Consulte [principios de la recuperación ante desastres y la preparación](hana-concept-preparation.md).
