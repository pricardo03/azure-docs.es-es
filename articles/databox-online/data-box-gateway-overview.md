---
title: Introducción a Microsoft Azure Data Box Gateway | Microsoft Docs
description: Describe Azure Data Box Gateway, una solución de almacenamiento de aplicaciones virtuales que permite transferir datos en Azure
services: databox
author: alkohli
ms.service: databox
ms.topic: overview
ms.date: 01/18/2019
ms.author: alkohli
ms.openlocfilehash: 9670d67fa1eb79e9e5e8c81726c10cc78767fb74
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435471"
---
# <a name="what-is-azure-data-box-gateway-preview"></a>¿Qué es Azure Data Box Gateway (versión preliminar)? 

Azure Data Box Gateway es una solución de almacenamiento que permite enviar fácilmente datos a Azure. En este artículo se proporciona información general acerca de la solución de Azure Data Box Gateway, sus ventajas, funcionalidades clave y los escenarios donde puede implementar este dispositivo. 

Data Box Gateway es un dispositivo virtual basado en una máquina virtual aprovisionada en un entorno virtualizado o hipervisor. El dispositivo virtual reside en su entorno local y se pueden escribir datos en él mediante los protocolos SMB y NFS. El dispositivo, a continuación, transfiere los datos a blob en bloques de Azure, blob en páginas o Azure Files. 

> [!IMPORTANT]
> Data Box Gateway está en versión preliminar. Revise los [términos de uso para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solución.

## <a name="use-cases"></a>Casos de uso

Data Box Gateway puede usarse para transferir datos a la nube, como archivos en la nube, para la recuperación ante desastres o si es necesario procesar los datos en la nube. Estos son los distintos escenarios donde se puede usar Data Box Gateway para la transferencia de datos.

- **Archivos en la nube**: copie cientos de TB de datos en Azure Storage mediante Data Box Gateway de manera segura y eficaz. Los datos pueden ingerirse de una vez o de forma continuada en los escenarios de archivo.

- **Agregación de datos**: agregue datos de varios orígenes en una única ubicación en Azure Storage para el análisis y procesamiento de datos.  

- **Integración con las cargas de trabajo locales**: integración con las cargas de trabajo locales, como la copia de seguridad y la restauración, que usan el almacenamiento en la nube y necesitan acceso local para los archivos usados habitualmente. 

## <a name="benefits"></a>Ventajas

Data Box Gateway tiene las siguientes ventajas:

- **Transferencia de datos sencilla**: facilita el traslado de datos hacia y desde Azure Storage tan fácilmente como si estuviera trabajando con un recurso compartido de red local.  
- **Alto rendimiento**: evita los inconvenientes de tener que transportar los datos de red con transferencias de alto rendimiento a y desde Azure. 
- **Acceso rápido y altas tasas de ingesta de datos durante el horario laboral**: Data Box Gateway tiene una caché local que se define como el tamaño de la capacidad local cuando se aprovisiona el dispositivo virtual. El tamaño del disco de datos debe especificarse según los [requisitos mínimos del dispositivo virtual](data-box-gateway-system-requirements.md#specifications-for-the-virtual-device). La caché local proporciona las siguientes ventajas:
    - La memoria caché local permite la ingesta de datos a alta velocidad. Cuando se ingiere una gran cantidad de datos durante las horas punta, la caché puede retener los datos y cargarlos en la nube.
    - La caché local permite un acceso rápido a la lectura hasta un determinado umbral. Hasta que el dispositivo esté lleno al 50-60 %, se accede a todas las lecturas del dispositivo desde la caché, lo que las hace más rápidas. Una vez que el espacio utilizado en el dispositivo supera este umbral, el dispositivo comienza a quitar los archivos locales. 
 
- **Uso de ancho de banda limitado**: se pueden escribir datos en Azure incluso cuando la red limita el uso durante las horas punta comerciales.  

## <a name="key-capabilities"></a>Principales capacidades

Data Box Gateway tiene las siguientes funcionalidades:

|Capacidad |DESCRIPCIÓN  |
|---------|---------|
|Velocidad     | Transferencia de datos y ancho de banda totalmente automatizados y altamente optimizados.|
|Protocolos admitidos     | Compatibilidad con los protocolos estándar SMB y NFS para la ingesta de datos. <br> Para obtener más información sobre las versiones compatibles, vaya a [Requisitos del sistema de Data Box Gateway](data-box-gateway-system-requirements.md).|
|Acceso a datos     | Ofrece acceso directo a los datos de Azure Storage Blob y Azure Files mediante las API en la nube para un procesamiento de datos adicional en la nube.|
|Acceso rápido     | Memoria caché local en el dispositivo para agilizar el acceso a los archivos usados más recientemente.|
|Carga sin conexión     | El modo sin conexión es compatible con escenarios de carga sin conexión.|
|Actualización de datos     | Capacidad para actualizar los archivos locales con la versión más reciente de la nube.|
|Cifrado    | Compatibilidad con BitLocker para, localmente, cifrar los datos y proteger las transferencias de datos hacia la nube a través de *https*.       |
|Resistencia     | Resistencia de red integrada        |


## <a name="specifications"></a>Especificaciones

El dispositivo virtual Data Box Gateway tiene las siguientes especificaciones:

| Especificaciones                                          | DESCRIPCIÓN              |
|---------------------------------------------------------|--------------------------|
| Procesadores virtuales (núcleos)   | Cuatro como mínimo |            
| Memoria  | 8 GB como mínimo|
| Disponibilidad|Nodo único|
| Discos| Disco del sistema operativo: 250 GB <br> Disco de datos: 2 TB como mínimo, con aprovisionamiento fino y debe estar basado en SSD|
| Interfaces de red|Una o varias interfaces de red virtual|
| Protocolos de uso compartido de archivos nativos|SMB y NFS  |
| Seguridad| Autenticación para desbloquear el acceso a datos y dispositivos <br> Cifrado de datos en proceso mediante el cifrado AES de 256 bits|
| Administración| Interfaz de usuario web local: programa de instalación inicial, diagnósticos y administración de energía de dispositivos <br> Azure Portal: administración diaria de dispositivos Data Box Gateway       |


## <a name="components"></a>Componentes

La solución Data Box Gateway consta de un recurso Data Box Gateway, un dispositivo físico Data Box Gateway y una interfaz de usuario web local.

* **Dispositivo virtual Data Box Gateway**: es un dispositivo virtual basado en una máquina virtual aprovisionada en un entorno virtualizado o hipervisor que le permite enviar datos a Azure. 
    
* **Recurso Data Box Gateway**: es un recurso de Azure Portal que le permite administrar un dispositivo Data Box Gateway desde una interfaz web a la cual puede obtener acceso desde diferentes ubicaciones geográficas. Use el recurso Data Box Gateway para crear y administrar recursos, ver y administrar dispositivos y alertas y administrar recursos compartidos.  

    <!--![The Data Box Gateway service in Azure portal](media/data-box-overview/data-box-Gateway-service1.png)-->

    <!--For more information, go to [Use the Data Box Gateway service to administer your Data Box Gateway device](data-box-gateway-portal-ui-admin.md).-->

* **Interfaz de usuario web local de Data Box**: use la interfaz de usuario web local para ejecutar diagnósticos, apagar y reiniciar el dispositivo Data Box Gateway, ver registros de copia y ponerse en contacto con el Soporte técnico de Microsoft para realizar una solicitud de servicio.

    <!--![The Data Box Gateway local web UI](media/data-box-gateway-overview/data-box-gateway-local-web-ui.png)-->

    <!-- For information about using the web-based UI, go to [Use the web-based UI to administer your Data Box](data-box-gateway-portal-ui-admin.md).-->


## <a name="region-availability"></a>Disponibilidad en regiones

El dispositivo físico Data Box Edge, el recurso de Azure y la cuenta de almacenamiento de destino a la que transferirá los datos no tienen que estar en la misma región.

- **Disponibilidad del recurso**: para esta versión, el recurso Data Box Edge está disponible en las siguientes regiones:
    - **Estados Unidos**: Oeste de EE. UU. 2 y Este de EE. UU.
    - **Unión Europea**: Europa Occidental
    - **Asia Pacífico**: Sudeste de Asia

- **Cuentas de almacenamiento de destino**: las cuentas de almacenamiento que almacenan los datos están disponibles en todas las regiones de Azure. 

    Las regiones en las que las cuentas de almacenamiento almacenan los datos de Data Box deben estar ubicadas cerca de la ubicación del dispositivo para un rendimiento óptimo. Una cuenta de almacenamiento que se encuentra lejos del dispositivo causa latencias largas y un rendimiento más lento. 


## <a name="next-steps"></a>Pasos siguientes

- Revise [los requisitos del sistema Data Box Gateway](data-box-gateway-system-requirements.md).
- Información acerca de los [límites de Data Box Gateway](data-box-gateway-limits.md).
- Implemente [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) en Azure Portal.




