---
title: Preguntas frecuentes de Microsoft Azure Data Box Disk | Microsoft Docs
description: Contiene preguntas frecuentes acerca de Azure Data Box Disk, una solución en la nube que permite transferir grandes cantidades de datos en Azure
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2018
ms.author: alkohli
ms.openlocfilehash: 611dcb2cb904b5d3ee6ce0f571c2d04cfd7e7c35
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451757"
---
# <a name="what-is-azure-data-box-disk-preview"></a>¿Qué es Azure Data Box Disk? (versión preliminar)

La solución en la nube Microsoft Azure Data Box Disk permite enviar terabytes de datos a Azure de forma rápida, económica y confiable. Este documento contiene preguntas que pueden surgirle al usar Data Box Disks en Azure Portal y respuestas a dichas preguntas. 

Las preguntas y respuestas se organizan en las categorías siguientes:

- Acerca del servicio
- Configuración y conexión 
- Seguimiento del estado
- Migración de los datos 
- Comprobación y carga de datos 

> [!IMPORTANT]
> Data Box Disk se encuentra en versión preliminar. Antes de implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="about-the-service"></a>Acerca del servicio

### <a name="q-what-is-azure-data-box-service"></a>P: ¿Qué es el servicio Azure Data Box? 
A.  El servicio Azure Data Box está diseñado para la ingesta de datos sin conexión. Este servicio administra una matriz de productos adaptados para el transporte de datos para diferentes capacidades de almacenamiento. 

### <a name="q-what-are-azure-data-box-disks"></a>P: ¿Qué son los discos de Azure Data Box?
A. Los discos de Azure Data Box permiten transferir terabytes de datos de forma rápida, barata y segura a y desde Azure. Microsoft envía entre 1 y 5, con una capacidad de almacenamiento máxima de 35 TB. Dichos discos se pueden configurar, conectar y desbloquear fácilmente con el servicio Data Box en Azure Portal.  

Los discos cuentan con cifrado de unidad BitLocker de Microsoft, mientras que las claves de cifrado se administran desde Azure Portal. Luego se copian los datos de los servidores del cliente. En el centro de datos, Microsoft migra los datos de la unidad a la nube con un vínculo de carga en la red rápido y privado, y los carga en Azure.

### <a name="q-when-should-i-use-data-box-disks"></a>P: ¿Cuándo deben usarse los discos de Data Box?
A. Si desea transferir 40 TB de datos (o menos) a Azure, es aconsejable usar de los discos de Data Box.

### <a name="q-what-is-the-price-of-data-box-disks"></a>P: ¿Cuánto cuestan los discos de Data Box?
A. Durante la versión preliminar, los discos de Data Box Disk está disponibles sin costo alguno. El envío es también gratuito; sin embargo, se aplicarán los gastos de almacenamiento de Azure.

### <a name="q-how-do-i-get-data-box-disks"></a>P: ¿Cómo se obtienen los discos de Data Box? 
A.  Para obtener los discos de Azure Data Box, primero es preciso registrarse para obtener la [versión preliminar de Data Box Disk](http://aka.ms/AzureDataBox). Luego, es preciso iniciar sesión en Azure Portal y cree un pedido de discos de Data Box. Proporcione su información de contacto y los datos de notificación. Una vez que realiza el pedido, y en función de la disponibilidad, los discos se envían en un plazo máximo de 10 días.   

### <a name="q-what-is-the-maximum-amount-of-data-i-can-transfer-with-data-box-disks-in-one-instance"></a>P: ¿Cuál es la cantidad máxima de datos que se puede transferir con los discos de Data Box en una instancia?
A. Para cinco discos de 8 TB cada uno (7 TB de capacidad usable), la capacidad máxima usable es 35 TB. Por tanto, puede transferir 35 TB de datos en una instancia.  Para transferir más datos, es preciso solicitar más discos.

### <a name="q-how-can-i-check-if-data-box-disks-are-available-in-my-region"></a>P: ¿Cómo se puede comprobar si los discos de Data Box están disponibles en una región concreta? 
A.  Los discos de Data Box están disponibles en Estados Unidos, Canadá, Australia y en todos los países en la Unión Europea durante la fase de versión preliminar.  

### <a name="q-which-regions-can-i-store-data-in-with-data-box-disks"></a>P: ¿En qué regiones puedo almacenar datos con los discos de Data Box?
A. Data Box Disk se admite en todas las regiones de Estados Unidos, Canadá, Australia, Europa Occidental y Europa del Norte en la versión preliminar. Solo se admiten las regiones con nube pública de Azure. Azure Government u otras nubes soberanas no se admiten.

### <a name="q-whom-should-i-contact-if-i-encounter-any-issues--with-data-box-disks"></a>P: ¿Con quién debo ponerme en contacto si surge algún problemas con los discos de Data Box?
A. Si surge algún problema con los discos de Data Box, póngase en contacto con el [soporte técnico de Data Box Disk](mailto:expresspodsupport@microsoft.com).

## <a name="configure-and-connect"></a>Configuración y conexión
 
### <a name="q-can-i-specify-the-number-of-data-box-disks-in-the-order"></a>P: ¿Se puede especificar el número de discos de Data Box en el pedido?
A.  No. Recibe discos de 8 TB (un máximo de cinco) en función del tamaño de los datos y de la disponibilidad de los discos.  

### <a name="q-how-do-i-unlock-the-data-box-disks"></a>P: ¿Cómo se desbloquean los discos de Data Box? 
A.  En Azure Portal, vaya a su pedido de discos de Data Box y, después, a **Detalles del dispositivo**. Copie la clave de paso. Descargue y extraiga la herramienta de desbloqueo Data Box Disk para su sistema operativo desde Azure Portal. Ejecute la herramienta en el equipo que contiene los datos que desea copiar en los discos. Escriba la clave de paso para desbloquear los discos. La misma clave desbloquea todos los discos. 

Para obtener instrucciones detalladas, consulte los temas donde se explica [cómo desbloquear discos en un cliente de Windows](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) o [cómo desbloquear discos en un cliente de Linux](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client).

### <a name="q-can-i-use-a-linux-host-computer-to-connect-and-copy-the-data-on-to-the-data-box-disks"></a>P: ¿Se puede usar un equipo host Linux para conectarse y copiar los datos de los discos de Data Box?
A.  Sí. Tanto los clientes de Linux como los clientes de Windows pueden utilizarse para conectar y copiar datos en los discos de Data Box Disk. Para más información, vaya a la lista de [sistemas operativos compatibles](data-box-disk-system-requirements.md) del equipo host.

### <a name="q-my-disks-are-dispatched-but-now-i-want-to-cancel-this-order-why-is-the-cancel-button-not-available"></a>P: Quiero cancelar un pedido una vez que los discos se han enviado. ¿Por qué no está disponible el botón de cancelación?
A.  El pedido solo se puede cancelar durante el periodo que va entre la realización del pedido y el momento en que se realiza el envío. Una vez que los discos se han enviado, no de puede cancelar el pedido. Durante el periodo de la versión preliminar, los discos se pueden devolver sin costo alguno, aunque es probable que esto cambie cuando la solución está disponible con carácter general. 

### <a name="q-can-i-connect-multiple-data-box-disks-at-the-same-to-the-host-computer-to-transfer-data"></a>P: ¿Se pueden conectar varios discos de Data Box al mismo al equipo host para transferir datos?
A. Sí. Se pueden conectar varios discos de Data Box al mismo equipo host para transferir datos y se pueden ejecutar varios trabajos de copia en paralelo.

## <a name="track-status"></a>Seguimiento del estado

### <a name="q-how-do-i-track-the-disks-from-when-i-placed-the-order-to-shipping-the-disks-back"></a>P: ¿Cómo se realiza el seguimiento de los discos desde el momento en que se realizó el pedido a la devolución de los discos? 
A.  El seguimiento del estado del pedido de Data Box Disk se puede realizar desde Azure Portal. Al crear el pedido, también se le solicita que indique una dirección de correo electrónico para cualquier notificación. Si ya la ha especificado, los cambios de estado del pedido se le notificarán por correo electrónico. Para más información acerca de cómo [configurar correos electrónicos de notificación](data-box-portal-ui-admin.md#edit-notification-details).

### <a name="q-how-do-i-return-the-disks"></a>P: ¿Cómo se devuelven los discos? 
A.  Microsoft incluye una etiqueta de envío con los discos de Data Box en el paquete en que estos se envían. Pegue dicha etiqueta en la caja de envío y lleve el paquete precintado a las instalaciones de la empresa que va a realizar el envío. Si la etiqueta se daña o se pierde, vaya a **Información general > Descargar la etiqueta de envío** y descargue una nueva etiqueta de envío de devolución.

## <a name="migrate-data"></a>Migración de los datos

### <a name="q-what-is-the-maximum-data-size-that-can-be-used-with-data-box-disks"></a>P: ¿Cuál es el tamaño máximo de los datos que se puede usar con los discos de Data Box?  
A.  La solución Data Box Disks puede tener hasta cinco discos, con una capacidad máxima utilizable de 35 TB. Los discos en sí tienen 8 TB (7 TB utilizables). 

### <a name="q-what-are-the-maximum-block-blob-and-page-blob-sizes-supported-by-data-box-disks"></a>P: ¿Qué tamaño máximo de blobs en bloques y blobs en páginas admiten los discos de Data Box? 
A.  El tamaño máximo lo determinan los límites de Azure Storage. El blob en bloques máximo es, aproximadamente, de 4,768 TiB, mientras que el tamaño de blob en páginas máximo es de 8 TiB. Para más información, consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../storage/common/storage-scalability-targets.md). 

### <a name="q-what-is-the-data-transfer-speed-for-data-box-disks"></a>P: ¿Cuál es la velocidad de transferencia de datos de los discos de Data Box?
A. Cuando se prueba con discos conectados mediante USB 3.0, el rendimiento del disco era de hasta 430 MB/s. Los números reales varían según el tamaño de archivo usado. Con archivos más pequeños, es posible que observe un rendimiento menor.

### <a name="q-how-do-i-know-that-my-data-is-secure-during-transit"></a>P: ¿Cómo se sabe si los datos están protegidos durante el tránsito? 
A.  Con los discos de Data Box se emplea el cifrado BitLocker AES de 128 bits y la clave de paso solo está disponible en Azure Portal. Para obtenerla, debe iniciar sesión en Azure Portal con las credenciales de su cuenta. Escriba dicha clave al ejecutar la herramienta de desbloqueo de Data Box Disk.

### <a name="q-how-do-i-copy-the-data-to-the-data-box-disks"></a>P: ¿Cómo se copian los datos a los discos de Data Box? 
A.  Mediante una herramienta de copia de SMB, como Robocopy, Diskboss, o incluso el Explorador de archivos de Windows, para arrastrar y colocar los datos en los discos, y así copiarlos. 

### <a name="q-are-there-any-tips-to-speed-up-the-data-copy"></a>P: ¿Algunas sugerencia para agilizar la copia de datos?
A.  Para acelerar el proceso de copia:

- Use varias secuencias de copia de datos. Por ejemplo, si usa Robocopy, emplee la opción de varios procesos. Para más información acerca del comando exacto que se debe usar, vaya a [Tutorial: Copia de datos a Azure Data Box Disk y comprobación de los mismos](data-box-disk-deploy-copy-data.md#copy-data-to-disks).
- Utilice varias sesiones.
- En lugar de realizar la copia a través de un recurso compartido de red (en el que puede tener la limitación de la velocidad de la red), asegúrese de que los datos se encuentran localmente en el equipo al que se conectan los discos.
- Asegúrese de que usa USB 3.0, o posterior, en todo el proceso de copia. Descargue la [herramienta USBView](https://docs.microsoft.com/windows-hardware/drivers/debugger/usbview) y úsela para identificar los controladores USB y dispositivos USB conectados al equipo.
- Evalúe el rendimiento del equipo usado para copiar los datos. Descargue la [herramienta Bluestop FIO](https://bluestop.org/fio/) y úsela para evaluar el rendimiento del hardware del servidor.

### <a name="q-how-to-speed-up-the-data-if-the-source-data-has-small-files-kbs-or-few-mbs"></a>P: ¿Cómo acelerar los datos si los datos de origen tienen archivos pequeños (de KB o pocos MB)?
A.  Para acelerar el proceso de copia:

- Cree un VHDx local en el almacenamiento rápido, o bien cree disco duro virtual vacío en la HDD o SSD (más lenta).
- Móntelo en una máquina virtual.
- Copie los archivos al disco de la máquina virtual.


### <a name="q-can-i-use-multiple-storage-accounts-with-data-box-disks"></a>P: ¿Se pueden usar varias cuentas de almacenamiento con los discos de Data Box?
A.  No. Actualmente, los discos de Data Box solo admiten una cuenta de almacenamiento, general o clásica. Se admiten blobs frecuentes y esporádicos. Durante la fase de versión preliminar, solo se admiten las cuentas de almacenamiento de Estados Unidos, Europa Occidental y Europa del Norte en la nube pública de Azure.

## <a name="verify-and-upload"></a>Comprobación y carga

### <a name="q-how-soon-can-i-access-my-data-in-azure-once-ive-shipped-the-disks-back"></a>P: ¿Cuándo se puede acceder a los datos en Azure una vez que se han devuelto los discos? 
A.  Una vez que el estado del pedido de Copia de datos indica que se ha completado, debería poder acceder a los datos de inmediato.

### <a name="q-where-is-my-data-located-in-azure-after-the-upload"></a>P: ¿Dónde se encuentran los datos en Azure después de la carga?
A.  Cuando se copian los datos de las carpetas *BlockBlob* y *PageBlob* del disco, se crea un contenedor en la cuenta de Azure Storage para cada una de las subcarpetas de las carpetas *BlockBlob* y *PageBlob*. Si ha copiado los archivos de las carpetas *BlockBlob* y *PageBlob* directamente, estos se encontrarán en el contenedor predeterminado *$root* de la cuenta de Azure Storage. 

### <a name="q-i-just-noticed-that-i-did-not-follow-the-azure-naming-requirements-for-my-containers-will-my-data-fail-to-upload-to-azure"></a>P: Acabo de darme cuenta de que no he seguido los requisitos de nomenclatura de Azure en los contenedores. ¿Pueden aparecer errores al cargar los datos en Azure?
A. Si los nombres de los contenedores tienen mayúsculas, se convierten automáticamente en minúsculas. Si los nombres no son compatibles por algún otro motivo (caracteres especiales, otros idiomas, etc.), se producirá un error en la carga. Para más información, vaya a [Convenciones de nomenclatura de Azure](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions).

### <a name="q-how-do-i-verify-the-data-i-copied-onto-multiple-data-box-disks"></a>P: ¿Cómo se comprueban los datos que se han copiado en varios discos de Data Box?
A.  Una vez completada la copia de datos, puede ejecutar `DataBoxDiskValidation.cmd`, que se encuentra en la carpeta *DataBoxDiskImport*, para generar sumas de comprobación de validación. Si tiene varios discos, es preciso que abra una ventana Comandos por disco y que ejecute este comando. Tenga en cuenta que esta operación puede tardar mucho tiempo (horas) en función del tamaño de los datos.

### <a name="q-what-happens-to-my-data-after-i-have-returned-the-disks"></a>P: ¿Qué ocurre con los datos después de que se devuelven los discos?
A.  Una vez que se completa la copia de datos en Azure, los datos de los discos se borran de forma segura según las directrices de la revisión 1 de NIST SP 800-88.  

### <a name="q-how-is-my-data-protected-during-transit"></a>P: ¿Cómo se protegen los datos durante el tránsito? 
A.  Los discos de Data Box se cifran con cifrado AES-128 BitLocker de Microsoft. Para desbloquear todos los discos y acceder a los datos, se requiere una clave de paso.

### <a name="q-do-i-need-to-rerun-checksum-validation-if-i-add-more-data-to-the-data-box-disks"></a>P: ¿Es preciso volver a ejecutar la validación de suma de comprobación si se agregan más datos a los discos de Data Box?
A. Sí. Si decide validar los datos (es aconsejable que lo haga), tendrá que volver a ejecutar la validación si agrega más datos a los discos.

### <a name="q-i-used-all-my-disks-to-transfer-data-and-need-to-order-more-disks-is-there-a-way-to-quickly-place-the-order"></a>P: He usado todos los discos para transferir datos y necesito pedir más discos. ¿Hay alguna manera rápida de realizar el pedido?
A. Puede clonar el pedido anterior. La clonación crea el mismo pedido que antes y permite editar los detalles del pedido solo sin necesidad de escribir los datos de dirección, contacto y notificación. 



## <a name="next-steps"></a>Pasos siguientes

- Revise [los requisitos del sistema Data Box](data-box-disk-system-requirements.md).
- Información acerca de los [límites de Data Box](data-box-disk-limits.md).
- Implemente rápidamente [Azure Data Box Disk](data-box-disk-quickstart-portal.md) en Azure Portal.
