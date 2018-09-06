---
title: Introducción a Microsoft Azure Data Box Disk | Microsoft Docs en datos
description: Describe Azure Data Box Disk, una solución en la nube que permite transferir grandes cantidades de datos en Azure
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/04/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand what Data Box Disk is and how it works so I can use it to import on-premises data into Azure.
ms.openlocfilehash: b1beb0e9a5a0435bdf298eddbefc230b2f95ed0a
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698509"
---
# <a name="what-is-azure-data-box-disk-preview"></a>¿Qué es Azure Data Box Disk? (versión preliminar)

La solución Microsoft Azure Data Box Disk le permite enviar terabytes de datos locales a Azure de forma rápida, económica y confiable. La transferencia de datos segura se acelera enviándole de 1 a 5 discos de estado sólido (SSD). Estos discos de 8 TB cifrados se envían al centro de datos a través de un operador regional. 

Puede configurar, conectar y desbloquear rápidamente los discos a través del servicio Data Box en Azure Portal. Copie los datos en discos y vuelva a enviar los discos a Azure. En el centro de datos de Azure, los datos se cargan automáticamente de las unidades a la nube mediante un vínculo de carga de red rápido y privado.


> [!IMPORTANT]
> - Data Box Disk se encuentra en versión preliminar. Antes de implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
> - Debe suscribirse a este servicio. Para suscribirse, vaya al [portal de vista previa](http://aka.ms/azuredataboxfromdiskdocs).
> - Durante la versión preliminar, Data Box Disk puede enviarse tanto a los clientes de Estados Unidos como a los de la Unión Europea. Para más información, vaya a [Disponibilidad por región](#region-availability).

## <a name="use-cases"></a>Casos de uso

Utilice Data Box Disk para transferir TB de datos en escenarios sin conectividad de red limitada. El movimiento de datos puede ser único, periódico o una transferencia de datos masiva inicial seguida de transferencias periódicas. 

- **Migración única**: cuando se mueve gran cantidad de datos locales a Azure. Por ejemplo, mover datos de cintas sin conexión a datos de archivo de un almacenamiento esporádico de Azure.
- **Transferencia incremental**: cuando se realiza una transferencia masiva inicial con Data Box Disk (inicialización) seguido de transferencias incrementales a través de la red. Por ejemplo, Commvault y Data Box Disk se usan para mover las copias de seguridad a Azure. A esta migración la sigue la copia de datos incrementales a través de la red al almacenamiento de Azure. 
- **Cargas periódicas**: cuando se genera periódicamente una gran cantidad de datos y es necesario moverlos a Azure. Por ejemplo, en la exploración de energía, donde el contenido de vídeo se genera en plataformas petrolíferas y parques eólicos.

## <a name="the-workflow"></a>El flujo de trabajo

Un flujo típico incluye los siguientes pasos:

1. **Solicitar**: cree un pedido en Azure Portal, proporcione la información de envío y la cuenta de almacenamiento de Azure de destino para los datos. Si hay discos disponibles, Azure cifra, prepara y envía los discos con un identificador de seguimiento del envío.

2. **Recibir**: una vez que se entregan los discos, desempaquete y conecte los discos al equipo que tiene los datos que se van a copiar. Desbloquee los discos.
    
3. **Copiar datos**: arrastre y colóquelos para copiar los datos en los discos.

4. **Devolver**: prepare y envíe los discos de nuevo al centro de datos de Azure.

5. **Cargar**: los datos se copian automáticamente de los discos a Azure. Los discos se borran de forma segura según las directrices del Instituto Nacional de Normas y Tecnología (NIST).

Durante este proceso, se le notificará por correo electrónico de todos los cambios de estado. Para más información sobre el flujo detallado, consulte [cómo implementar instancias de Data Box Disk en Azure Portal](data-box-disk-quickstart-portal.md).


## <a name="benefits"></a>Ventajas

Data Box Disk está pensado para mover grandes cantidades de datos a Azure sin que afecte a la red. La solución tiene las siguientes ventajas:

- **Velocidad**: Data Box Disk usa una conexión USB 3.0 para mover hasta 35 TB de datos en Azure en menos de una semana.   

- **Fácil de usar**: Data Box es una solución fácil de usar.

    - Los discos utilizan conexión USB prácticamente sin tiempo de instalación.
    - Los discos tienen un factor de forma pequeño que hace que sean fáciles de manipular.
    - Los discos no tienen ningún requisito de alimentación externa.
    - Los discos pueden usarse con un servidor de centro de datos, equipo de escritorio o portátil.
    - La solución proporciona un seguimiento total a través de Azure Portal.    

- **Proteger**: Data Box Disk tiene protecciones de seguridad integradas para los discos, los datos y el servicio. 
    - Los discos son resistente a alteraciones y admiten la funcionalidad de actualización segura. 
    - Los datos en los discos se protegen con un cifrado AES de 128 bits en todo momento. 
    - Los discos solo se pueden desbloquear con una clave proporcionada en Azure Portal. 
    - El servicio está protegido por las características de seguridad de Azure. 
    - Cuando los datos se cargan en Azure, los discos se limpian, según las normas NIST 800-88r1.  
    
Para más información, vaya a [Azure Data Box Disk security and data protection](data-box-disk-security.md) (Protección de datos y seguridad de Azure Data Box Disk).


## <a name="features-and-specifications"></a>Características y especificaciones


| Especificaciones                                          | DESCRIPCIÓN              |
|---------------------------------------------------------|--------------------------|
| Peso                                                  | < 2 libras por caja. Hasta 5 discos en la caja                |
| Dimensiones                                              | Disco: SSD de 2,5" |            
| Cables                                                  | 1 cable USB 3.1 por disco|
| Capacidad de almacenamiento por pedido                              | 40 TB (utilizable ~ 35 TB)|
| Capacidad de almacenamiento de disco                                   | 8 TB (utilizable ~ 7 TB)|
| Interfaz de datos                                          | USB   |
| Seguridad                                                | Previamente cifrada mediante BitLocker y la actualización segura <br> Discos protegidos por clave de paso <br> Datos cifrados en todo momento  |
| Velocidad de transferencia de datos                                      | Hasta 430 MBps en función del tamaño de archivo      |
|Administración                                               | Azure Portal |


## <a name="region-availability"></a>Disponibilidad en regiones

Durante la versión preliminar, Data Box Disk puede transferir datos a las siguientes regiones de Azure:


|Región de Azure  |Región de Azure  |
|---------|---------|
|Centro occidental de EE.UU.     |Centro de Canadá       |        
|Oeste de EE. UU. 2     |Este de Canadá         |     
|Oeste de EE. UU.     | Europa occidental        |      
|Centro-Sur de EE. UU   |Europa del Norte     |         
|Centro de EE. UU.     |Este de Australia|
|Centro-Norte de EE. UU  |Sudeste de Australia   |
|Este de EE. UU      |Centro de Australia |
|Este de EE. UU. 2     |Centro de Australia 2|


## <a name="pricing"></a>Precios

Durante la versión preliminar, Data Box Disk está disponible sin costo alguno. Esta situación cambiará cuando Data Box Disk esté disponible con carácter general.

## <a name="next-steps"></a>Pasos siguientes

- Revise los [requisitos de Data Box Disk](data-box-disk-system-requirements.md).
- Información acerca de los [límites de Data Box Disk](data-box-disk-limits.md).
- Implemente rápidamente [Azure Data Box Disk](data-box-disk-quickstart-portal.md) en Azure Portal.
