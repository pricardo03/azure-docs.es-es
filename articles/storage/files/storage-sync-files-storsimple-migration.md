---
title: Migración de StorSimple a Azure File Sync
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: fauhse
ms.subservice: files
description: StorSimple es un producto que ha llegado al final de su ciclo de vida y Azure File Sync es la solución a la que se va a migrar. Obtenga información sobre el concepto de migración y utilice AzureFiles@microsoft.com para obtener ayuda sobre la migración personalizada.
ms.openlocfilehash: 1cc88080522a62085d9a515223512ef25c20a9e4
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895076"
---
# <a name="storsimple-migration-to-azure-file-sync"></a>Migración de StorSimple a Azure File Sync

StorSimple es un producto de Microsoft que ha dejado de utilizarse. El soporte extendido para este producto y su servicio en la nube durará hasta finales de 2022.
Es importante empezar a planear de inmediato la migración de StorSimple.

El servicio de Azure a largo plazo, estratégico y predeterminado al que se pueden migrar los dispositivos StorSimple es Azure File Sync. Es un servicio de Azure disponible con carácter general con un superconjunto de características a través de StorSimple.

## <a name="full-cloud-side-migration-with-limited-downtime"></a>Migración completa en la nube con poco tiempo de inactividad
En este artículo se describe cómo comenzará una migración.
Es imperativo indicar que no es preciso que los clientes que necesitan migrar de StorSimple a Azure File Sync lo hagan solos.

> [!IMPORTANT]
> Microsoft se compromete a ayudar a los clientes en su migración. Envíe un correo electrónico a AzureFiles@microsoft.com si desea obtener un plan de migración personalizado, así como ayuda durante la migración.

## <a name="migration-approach"></a>Enfoque de migración
La migración a Azure File Sync comenzará en la nube y tendrá un impacto mínimo en el entorno local; el tiempo de inactividad será escaso.
El siguiente concepto está dirigido a los dispositivos de la serie StorSimple 8000.
Si necesita migrar desde la serie StorSimple 7000, el primer paso es realizar una actualización gratis a un dispositivo de préstamo de la serie 8000 equivalente de Microsoft.
Envíe un mensaje de correo a AzureFiles@microsoft.com y le ayudaremos a organizar un número adecuado de dispositivos de préstamo.

### <a name="general-approach"></a>Enfoque general
![Alt](media/storage-sync-files-storsimple-migration/storsimple-docs-overview-concept.png "Ilustración de la migración en la nube mediante una aplicación virtual temporal y Windows Server a un nuevo servidor de Windows Server local que reemplace el dispositivo StorSimple local.")

1. Clone el volumen de su dispositivo StorSimple local y móntelo en una aplicación virtual StorSimple temporal.
2. Conecte la aplicación virtual a través de iSCSI a una máquina virtual de Azure temporal.
3. Instale Azure File Sync en la máquina virtual Windows Server temporal (también hay que establecer una clave del registro específica para esta migración antes de configurar la sincronización en el servidor).
    * Implemente el mismo número de recursos compartidos de archivos de Azure que recursos compartidos haya en el volumen de StorSimple (se recomienda implementar un recurso compartido de archivos de Azure por cada cuenta de almacenamiento).
    * Configure la sincronización entre el recurso compartido individual de la máquina virtual Windows Server en la nube y un recurso compartido de archivos de Azure (asignación 1:1)
    * Opcionalmente, agregue un servidor local como caché de rendimiento local, con la opción de nube por niveles habilitada. Este paso es necesario si desea reemplazar su dispositivo StorSimple local por una memoria caché local con más características, con la tecnología de Windows Server y la opción de nube por niveles de Azure File Sync. El equipo Windows Server local puede ser una máquina física o un clúster, o bien una máquina virtual. No es preciso implementar tanto almacenamiento como el tamaño del conjunto de datos. Solo se necesita el almacenamiento suficiente para almacenar localmente en caché los archivos a los que se accede con más frecuencia.

## <a name="minimizing-downtime"></a>Minimización del tiempo de inactividad
Después del paso 3 anterior, tanto los usuarios como las aplicaciones siguen usando activamente el dispositivo StorSimple local. Por consiguiente, el conjunto de archivos del clon del volumen inicial que se sincronizó está ligeramente obsoleto en el momento en que se completa la sincronización.
El enfoque para minimizar el tiempo de inactividad es repetir la sincronización desde el proceso de clonación del volumen para que la sincronización finalice más rápidamente con cada iteración que, a su vez, se habilita por los cambios entre los clones del volumen, que cada vez son menos.
Puede repetir este proceso hasta que la sincronización del clon de un volumen pueda finalizar en el período que le parezca aceptable.
A continuación, bloquee los usuarios y las aplicaciones para que no puedan realizar cambios en el dispositivo StorSimple. Se inicia el tiempo de inactividad.
Cree otro clon del volumen y deje que se sincronice con los servidores conectados.
Dé acceso a sus usuarios y aplicaciones al nuevo equipo Windows Server, que usa Azure File Sync.
Considere la posibilidad de implementar o ajustar un espacio de nombres DFS para que la migración del dispositivo StorSimple anterior al nuevo servidor de Windows Server sea transparente para las aplicaciones y los usuarios.
La migración se ha completado.

## <a name="migration-goal"></a>Objetivo de la migración
Una vez que se haya completado la migración, se pueden desaprovisionar tanto la aplicación virtual StorSimple temporal como la máquina virtual de Azure.

Además, el dispositivo StorSimple local se puede desaprovisionar, ya que los usuarios y las aplicaciones ya tienen acceso al servidor de Windows Server.
La imagen siguiente muestra lo que queda. Una implementación de Azure File Sync estándar incluye varios recursos compartidos de Azure y servidores de Windows Server conectados a ellos a través de Azure File Sync. Recuerde que un servidor individual puede conectar distintas carpetas locales a diferentes recursos compartidos de archivos a la vez.
Además, un recurso compartido de archivos de Azure se puede sincronizar con muchos servidores diferentes, por si se necesita que los datos se almacenen en la caché de las sucursales. Compruebe también si puede optimizar las directivas de nube por niveles para hacer un uso más eficaz del espacio de almacenamiento local.

![Alt](media/storage-sync-files-storsimple-migration/storsimple-docs-goal.PNG "Ilustración que muestra el objetivo una vez completada la migración. Muestra varios recursos compartidos de archivos que se sincronizan con un servidor de Windows Server local y los usuarios y aplicaciones que acceden a los archivos que están en la nube o en el servidor de Windows Server.")

## <a name="next-steps"></a>Pasos siguientes
Familiarícese con Azure Files y Azure File Sync. Para que una migración se realice correctamente, es importante conocer la terminología y el patrón de implementación de Azure File Sync. En este artículo hay información más detallada acerca de cada paso. Póngase en contacto con Microsoft para obtener ayuda personalizada durante el planeamiento y la ejecución de la migración.

> [!IMPORTANT]
> Microsoft se compromete a ayudar a los clientes en su migración. Envíe un correo electrónico a AzureFiles@microsoft.com si desea obtener un plan de migración personalizado, así como ayuda durante la migración.

## <a name="additional-resources"></a>Recursos adicionales
Azure File Sync, como servicio de destino, tiene dos documentos fundamentales cuya lectura es muy recomendable si nunca se ha usado Azure File Sync.
* [Introducción a Azure File Sync](storage-sync-files-planning.md)
* [Guía de implementación de Azure File Sync](storage-sync-files-deployment-guide.md)

Azure Files es un servicio de almacenamiento de Azure que ofrece recursos compartidos de archivos como servicio. No es necesario pagar ni mantener una máquina virtual ni el almacenamiento de máquina virtual asociado.
* [Introducción a Azure Files](storage-files-introduction.md)
* [Implementación de un recurso compartido de archivos de Azure en Azure Files](storage-how-to-create-file-share.md)