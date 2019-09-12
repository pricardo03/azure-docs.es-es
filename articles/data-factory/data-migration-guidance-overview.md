---
title: Uso de Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure | Microsoft Docs
description: Use Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 7/30/2019
ms.openlocfilehash: 937a076b3e0e3c5170779d3449776f0aa1cf5b49
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258997"
---
# <a name="use-azure-data-factory-to-migrate-data-from-your-data-lake-or-data-warehouse-to-azure"></a>Uso de Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure 

Azure Data Factory puede ser la herramienta de migración de datos cuando desee migrar el lago de datos o el almacenamiento de datos empresariales (EDW) a Azure. La migración del lago de datos y la migración del almacenamiento de datos están relacionadas con los siguientes escenarios: 

- Migración de cargas de trabajo de macrodatos desde AWS S3 y el sistema de archivos Hadoop local a Azure. 
- Migración de EDW desde Oracle Exadata, Netezza, Teradata y AWS Redshift a Azure. 

Azure Data Factory puede mover petabytes de datos para la migración del lago de datos y decenas de terabytes de datos para la migración del almacenamiento de datos. 

## <a name="why-azure-data-factory-can-be-used-for-data-migration"></a>Por qué usar Azure Data Factory para la migración de datos 

- Azure Data Factory puede escalar fácilmente la cantidad de potencia necesaria para mover datos en modo sin servidor con alto rendimiento, resistencia y escalabilidad y pagar solo por lo que usa.  
  - Azure Data Factory no tiene ninguna limitación en cuanto al volumen de datos y el número de archivos.
  - Azure Data Factory puede utilizar el 100 % de la red y del ancho de banda de almacenamiento para lograr el mayor rendimiento de movimiento de datos en su entorno.   
  - Azure Data Factory sigue la estrategia de pago por uso, por lo que solo tiene que pagar por el tiempo que usa Azure Data Factory para realizar la migración de datos a Azure.  
- Azure Data Factory tiene la capacidad de realizar una carga histórica única, así como una carga incremental programada. 
- Azure Data Factory usa Azure IR para mover datos entre los puntos de conexión de acceso público del lago de datos o el almacenamiento de datos o bien puede usar IR autohospedado para mover los datos de los puntos de conexión del lago de datos o el almacenamiento de datos dentro de la red virtual o detrás de un firewall. 
- Azure Data Factory tiene seguridad de nivel empresarial: use MSI o la identidad de servicio para una integración de servicio a servicio protegida o aproveche las ventajas de Azure Key Vault para la administración de credenciales. 
- Azure Data Factory proporciona una experiencia de creación sin código y un completo panel de supervisión integrado.  

## <a name="online-vs-offline-data-migration"></a>Migración de datos en línea frente a sin conexión

Azure Data Factory es una herramienta de migración de datos en línea típica para transferir datos a través de la red (Internet, ER o VPN), en la que la migración de datos sin conexión permite a los usuarios enviar físicamente los dispositivos de transferencia de datos desde su organización al centro de datos de Azure.  

Hay tres consideraciones importantes a la hora de seleccionar el método de migración en línea frente al método sin conexión:  

- Tamaño de los datos que se van a migrar. 
- Ancho de banda de la red. 
- Ventana de migración.   

Si desea completar la migración de datos en un plazo de dos semanas (ventana de migración), puede ver una línea de corte en la imagen siguiente para mostrar cuándo es adecuado usar la herramienta de migración en línea (Azure Data Factory) con diferentes tamaños de datos y anchos de banda de red.   

![en línea frente a sin conexión](media/data-migration-guidance-overview/online-offline.png)

> [!NOTE]
> La ventaja del enfoque de migración en línea es que puede lograr la carga de datos históricos y las fuentes incrementales de un extremo a otro con una única herramienta.  Al hacerlo así, los datos se pueden mantener sincronizados entre el almacén nuevo y el existente durante toda la ventana de migración para que pueda volver a generar la lógica de ETL en el nuevo almacén con datos actualizados. 


## <a name="next-steps"></a>Pasos siguientes

- [Migración de datos de AWS S3 a Azure](data-migration-guidance-s3-azure-storage.md)
- [Migración de datos desde un clúster de Hadoop local a Azure](data-migration-guidance-hdfs-azure-storage.md)
- [Migración de datos desde un servidor de Netezza local a Azure](data-migration-guidance-netezza-azure-sqldw.md)
