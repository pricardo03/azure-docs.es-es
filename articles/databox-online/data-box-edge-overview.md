---
title: Introducción a Microsoft Azure Data Box Edge | Microsoft Docs
description: Describe Azure Data Box Edge, una solución de almacenamiento que usa un dispositivo físico para las transferencias basadas en red a Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 07/09/2019
ms.author: alkohli
ms.openlocfilehash: bc6b9662a5d67f6ed315c33444bcb061115fe6d4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67701472"
---
# <a name="what-is-azure-data-box-edge"></a>¿Qué es Azure Data Box Edge? 

Azure Data Box Edge es una solución de almacenamiento que le permite procesar datos y enviarlos a través de la red a Azure. En este artículo se proporciona información general acerca de la solución de Data Box Edge, sus ventajas, funcionalidades clave y los escenarios donde puede implementar este dispositivo. 

Data Box Edge usa un dispositivo físico proporcionado por Microsoft para acelerar la transferencia de datos segura. El dispositivo físico reside en su entorno local y se pueden escribir datos en él mediante los protocolos SMB y NFS. 

Data Box Edge tiene todas las capacidades de puerta de enlace de Data Box Gateway. Además, Data Box está equipado con funcionalidades de procesamiento perimetral habilitadas para inteligencia artificial que ayudan a analizar, procesar o filtrar los datos mientras se desplazan a un blob en bloques de Azure, blob en páginas o a Azure Files.  

## <a name="use-cases"></a>Casos de uso

Azure Data Box Edge es un dispositivo informático perimetral habilitado para inteligencia artificial con funcionalidades de transferencia de datos de red. Estos son los distintos escenarios donde se puede usar Data Box Edge para la transferencia de datos.

- **Preprocesar los datos** : analice datos desde un entorno local o dispositivos de IoT para obtener un resultado rápido mientras se mantiene cerca del origen de los datos. Data Box Edge transfiere conjuntos de datos perimetrales enteros a la nube para llevar a cabo un procesamiento más avanzado o un análisis más profundo.  El preprocesamiento se puede usar para: 

    - Agregar datos.
    - Modificar los datos, por ejemplo, para quitar información de identificación personal (DCP).
    - Crear un subconjunto de los datos necesarios para realizar un análisis más profundo en la nube y transferirlos.
    - Analizar y reaccionar a los eventos de IoT. 

- **Inferencias de Azure Machine Learning**: con Data Box Edge, puede ejecutar modelos de Machine Learning (ML) para obtener resultados rápidos sobre los que se puede actuar antes de que los datos se envían a la nube. El conjunto de datos completo se puede transferir para seguir entrenando y mejorando los modelos de Machine Learning. Para más información sobre cómo utilizar los modelos acelerados por hardware de Azure Machine Learning en el dispositivo Data Box Edge, consulte [Implementación de modelos acelerados por hardware de Azure Machine Learning en Data Box Edge](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Transferir datos a través de red a Azure**: use Data Box Edge para transferir datos a Azure de forma rápida y sencilla para poder realizar más procesos y análisis o para fines de archivado. 

## <a name="benefits"></a>Ventajas

Data Box Edge tiene las siguientes ventajas:

- **Transferencia de datos sencilla**: permite mover datos hacia y desde el almacenamiento de Azure tan fácilmente como si estuviera trabajando con un recurso compartido de red local.  
- **Alto rendimiento**: permite las transferencias de alto rendimiento hacia y desde Azure. 
- **Acceso rápido**: almacena en caché los archivos más recientes para agilizar el acceso a los archivos de entornos locales.  
- **Uso de ancho de banda limitado**: se pueden escribir datos en Azure incluso cuando la red limita el uso durante las horas punta comerciales.  
- **Transformación de datos**: habilita el análisis, procesamiento o filtrado de datos cuando se mueven a Azure.

## <a name="key-capabilities"></a>Principales capacidades

Data Box Edge tiene las siguientes funcionalidades:

|Capacidad |DESCRIPCIÓN  |
|---------|---------|
|Alto rendimiento     | Transferencia de datos y ancho de banda totalmente automatizados y altamente optimizados.|
|Protocolos admitidos     | Compatibilidad con los protocolos estándar SMB y NFS para la ingesta de datos. <br> Para más información sobre las versiones compatibles, vaya a [Requisitos del sistema de Azure Data Box Edge](data-box-edge-system-requirements.md).|
|Procesamiento       |Permite el análisis, procesamiento y filtrado de datos.|
|Acceso a datos     | Ofrece acceso directo a los datos de Azure Storage Blob y Azure Files mediante las API en la nube para un procesamiento de datos adicional en la nube.|
|Acceso rápido     | Memoria caché local en el dispositivo para agilizar el acceso a los archivos usados más recientemente.|
|Carga sin conexión     | El modo sin conexión es compatible con escenarios de carga sin conexión.|
|Actualización de datos     | Capacidad para actualizar los archivos locales con la versión más reciente de la nube.|
|Cifrado    | Compatibilidad con BitLocker para, localmente, cifrar los datos y proteger las transferencias de datos hacia la nube a través de *https*.       |
|Resistencia     | Resistencia de red integrada.        |


## <a name="components"></a>Componentes

La solución Data Box Edge consta de un recurso Data Box Edge, un dispositivo físico Data Box Edge y una interfaz de usuario web local.

* **Dispositivo físico Data Box Edge**: un servidor de montaje en bastidor 1U proporcionado por Microsoft que se puede configurar para enviar datos a Azure. 
    
* **Recurso Data Box Edge**: es un recurso de Azure Portal que le permite administrar un dispositivo Data Box Edge desde una interfaz web a la cual puede acceder desde diferentes ubicaciones geográficas. Use el recurso Data Box Edge para crear y administrar recursos, ver y administrar dispositivos y alertas y administrar recursos compartidos.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Para más información, vaya a [Creación de un pedido para el dispositivo de Data Box Edge](data-box-edge-deploy-prep.md#create-a-new-resource).

* **Interfaz de usuario web local de Data Box**: use la interfaz de usuario web local para ejecutar diagnósticos, apagar y reiniciar el dispositivo Data Box Edge, ver registros de copia y ponerse en contacto con el Soporte técnico de Microsoft para realizar una solicitud de servicio.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Para más información acerca de cómo usar la interfaz de usuario basada en web, consulte [Use the web-based UI to administer your Data Box](data-box-edge-manage-access-power-connectivity-mode.md) (Uso de la interfaz de usuario web para administrar Data Box).


## <a name="region-availability"></a>Disponibilidad en regiones

El dispositivo físico Data Box Edge, el recurso de Azure y la cuenta de almacenamiento de destino a la que transferirá los datos no tienen que estar en la misma región.

- **Disponibilidad de recursos**: para obtener una lista de todas las regiones en donde está disponible el recurso de Data Box Edge, vaya a [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Data Box Edge también se puede implementar en la nube de Azure Government. Para más información, consulte [¿Qué es Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).
    
- **Cuentas de almacenamiento de destino**: las cuentas de almacenamiento que almacenan los datos están disponibles en todas las regiones de Azure. Las regiones en las que las cuentas de almacenamiento almacenan los datos de Data Box Edge deben estar ubicadas cerca de la ubicación del dispositivo para un rendimiento óptimo. Una cuenta de almacenamiento que se encuentra lejos del dispositivo causa latencias largas y un rendimiento más lento. 


## <a name="next-steps"></a>Pasos siguientes

- Revise [los requisitos del sistema Data Box Edge](data-box-edge-system-requirements.md).
- Información acerca de los [límites de Data Box Edge](data-box-edge-limits.md).
- Implemente rápidamente [Azure Data Box Edge](data-box-edge-deploy-prep.md) en Azure Portal.




