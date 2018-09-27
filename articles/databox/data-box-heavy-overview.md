---
title: Introducción a Microsoft Azure Data Box Heavy | Microsoft Docs en datos
description: Describe Azure Data Box, una solución híbrida que permite transferir grandes cantidades de datos en Azure
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 8ecef86841bcf13a469f9c0dc81f114bd54acdba
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946528"
---
# <a name="what-is-azure-data-box-heavy-preview"></a>¿Qué es Azure Data Box Heavy? (versión preliminar)

La solución híbrida Microsoft Azure Data Box le permite enviar cientos de terabytes de datos a Azure de forma rápida, económica y confiable. La transferencia de datos segura se acelera mediante el envío de un dispositivo de almacenamiento de propietario con una capacidad de 1 PB. El dispositivo tiene un sólido uso de mayúsculas y minúsculas para proteger los datos durante el tránsito.

Data Box Heavy está actualmente en versión preliminar, pero puede registrarse para solicitar un dispositivo mediante Azure Portal. Una vez reciba el dispositivo en el centro de datos, puede configurarlo rápidamente mediante la interfaz de usuario web local. Copie los datos de los servidores en el dispositivo y envíelo de nuevo a Azure. En el centro de datos de Azure, los datos se cargan automáticamente del dispositivo a la nube. El servicio de Data Box se encarga de realizar el seguimiento de todo el proceso en Azure Portal.


> [!IMPORTANT]
> - Data Box Heavy se encuentra en versión preliminar. Antes de implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
> - Para solicitar un dispositivo, regístrese en el [portal de versión preliminar](http://aka.ms/).
> - Durante la versión preliminar, Data Box Heavy puede enviarse tanto a los clientes de Estados Unidos como a los de la Unión Europea. Para más información, vaya a [Disponibilidad por región](#region-availability).

## <a name="use-cases"></a>Casos de uso

Data Box Heavy es ideal para más de 500 TB de datos en escenarios sin conectividad de red limitada. El movimiento de datos puede ser único, periódico o una transferencia de datos masiva inicial seguida de transferencias periódicas. Estos son los distintos escenarios donde se puede usar Data Box Heavy para la transferencia de datos.

 - **Migración única**: cuando se mueve gran cantidad de datos locales a Azure. 
     - Traslade una biblioteca multimedia de cintas sin conexión a Azure para crear una biblioteca multimedia en línea.
     - Migre la granja de máquinas virtuales, SQL Server y las aplicaciones a Azure.
     - Traslade los datos históricos a Azure para un análisis exhaustivo y generar informes con HDInsight.

 - **Transferencia masiva inicial**: cuando se realiza una transferencia masiva inicial con Data Box Heavy (inicialización) seguida de transferencias incrementales a través de la red. 
     - Por ejemplo, los asociados de soluciones de copia de seguridad, como Commvault y Data Box Heavy, se usan para mover la copia de seguridad histórica de gran tamaño inicial a Azure. Una vez completado el proceso, los datos incrementales se transfieren a través de la red a Azure Storage.

 - **Cargas periódicas**: cuando se genera periódicamente una gran cantidad de datos y es necesario moverlos a Azure. Por ejemplo, en la exploración de energía, donde el contenido de vídeo se genera en plataformas petrolíferas y parques eólicos.      

## <a name="benefits"></a>Ventajas

Data Box Heavy está pensado para mover grandes cantidades de datos a Azure sin que afecte casi a la red. La solución tiene las siguientes ventajas:

- **Velocidad**: Data Box Heavy utiliza interfaces de red de alto rendimiento de 40 Gbps.

- **Seguridad**: Data Box Heavy tiene protecciones de seguridad integradas para el dispositivo, los datos y el servicio.
    - El dispositivo tiene un sólido uso de mayúsculas y minúsculas protegido por tornillos resistentes a alteraciones y adhesivos de alteración evidente. 
    - Los datos en el dispositivo se protegen con un cifrado AES de 256 bits en todo momento.
    - El dispositivo solo se puede desbloquear con una contraseña proporcionada en Azure Portal.
    - El servicio está protegido por las características de seguridad de Azure.
    - Cuando los datos se cargan en Azure, los discos se limpian en el dispositivo, según las normas NIST 800-88r1.


<!--## Features and specifications

The Data Box Heavy device has the following features in this release.

| Specifications                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Weight                                                  | < 50 lbs.                |
| Dimensions                                              | Device - Width: 309.0 mm Height: 430.4 mm Depth: 502.0 mm |            
| Rack space                                              | 7 U when placed in the rack on its side (cannot be rack-mounted)|
| Cables required                                         | 1 X power cable (included) <br> 2 RJ45 cables <br> 2 X SFP+ Twinax copper cables|
| Storage capacity                                        | 100 TB <br> 80 TB usable capacity after RAID 5 protection|
| Network interfaces                                      | 2 X 1 GbE interface - MGMT, DATA 3. <br> MGMT - for management, not user configurable, used for initial setup <br> DATA3 - for data, user configurable, and is dynamic by default <br> MGMT and DATA 3 can also work as 10 GbE <br> 2 X 10 GbE interface - DATA 1, DATA 2 <br> Both are for data, can be configured as dynamic (default) or static |
| Data transfer media                                     | RJ45, SFP+ copper 10 GbE Ethernet  |
| Security                                                | Rugged device casing with tamper-proof custom screws <br> Tamper-evident stickers placed at the bottom of the device|
| Data transfer rate                                      | Up to 80 TB in a day over 10 GbE network interface        |
| Management                                              | Local web UI - one-time initial setup and configuration <br> Azure portal - day-to-day device management        |-->

## <a name="components"></a>Componentes

Data Box Heavy incluye los siguientes componentes:

* **Dispositivo de Data Box Heavy**: un dispositivo físico con una sólida estructura exterior que almacena los datos de forma segura. Este dispositivo tiene una capacidad de almacenamiento utilizable de 800 TB. 

    
* **Servicio Data Box**: es una extensión de Azure Portal que le permite administrar un dispositivo Data Box Heavy desde una interfaz web a la cual puede acceder desde diferentes ubicaciones geográficas. Use el servicio Data Box para realizar la administración diaria del dispositivo Data Box Heavy. Las tareas del servicio incluyen cómo crear y administrar pedidos, ver y administrar alertas y administrar recursos compartidos.  

* **Interfaz de usuario web local**: es una interfaz de usuario basada en web que se usa para configurar el dispositivo; de esta manera, se podrá conectar a la red local y registrar el dispositivo con el servicio Data Box. Use también la interfaz de usuario web local para apagar y reiniciar el dispositivo, ver registros de copia y ponerse en contacto con el Soporte técnico de Microsoft para realizar una solicitud de servicio.


## <a name="the-workflow"></a>El flujo de trabajo

Un flujo típico incluye los siguientes pasos:

1. **Solicitar**: cree un pedido en Azure Portal, proporcione la información de envío y la cuenta de almacenamiento de Azure de destino para los datos. Si el dispositivo está disponible, Azure lo prepara y lo envía con un identificador de seguimiento del envío.

2. **Recibir**: una vez entregado el dispositivo, conéctelo a la red y enciéndalo con los cables especificados. Active el dispositivo y conéctese a él. Configure la red del dispositivo y monte los recursos compartidos en el equipo host desde donde desea copiar los datos.

3. **Copiar los datos**: copie los datos en los recursos compartidos de Data Box Heavy.

4. **Devolver**: prepare, desactive y devuelva el dispositivo a los centros de datos de Azure.

5. **Cargar**: los datos se copian automáticamente del dispositivo a Azure. Los discos del dispositivo se borran de forma segura según las directrices del Instituto Nacional de Normas y Tecnología (NIST).

Durante este proceso, se le notificará por correo electrónico de todos los cambios de estado. 

## <a name="region-availability"></a>Disponibilidad en regiones

Data Box Heavy puede transferir datos en función de la región en la que se implementa el servicio, del país al que se envía el dispositivo y de la cuenta de Azure Storage en la que transfiere los datos. 

- **Disponibilidad del servicio**: para esta versión, Data Box Heavy está disponible en las siguientes regiones:
    - Todas las regiones de nube pública de Estados Unidos: Centro-oeste de EE. UU., Oeste de EE. UU. 2, Oeste de EE. UU., Centro-Sur de EE. UU, Centro de EE. UU., Centro-Norte de EE. UU., Este de EE. UU. y Este de EE. UU. 2.
    - Unión Europea: Europa Occidental y Europa del Norte.
    - Reino Unido: Sur de Reino Unido y Oeste de Reino Unido.
    - Francia: Centro de Francia y Sur de Francia.

- **Cuentas de almacenamiento de destino**: las cuentas de almacenamiento que almacenan los datos están disponibles en todas las regiones de Azure donde está disponible el servicio. 

## <a name="sign-up"></a>Suscripción

Data Box Heavy está en versión preliminar y es preciso registrarse. Siga estos pasos para registrarse en Data Box Heavy:

1. Inicie sesión en Azure Portal en: https://aka.ms/azuredatabox.
2. Haga clic en **+** para crear un nuevo recurso. Busque **Azure Data Box**. Seleccione el servicio **Azure Data Box**.

    <!--![The Data Box Heavy sign up 1]()-->

3. Haga clic en **Create**(Crear).

    <!--![The Data Box Heavy sign up 2]()-->

4. Seleccione la suscripción que quiere usar para la versión preliminar de Data Box Heavy. Seleccione la región donde quiere implementar el recurso Data Box Heavy. En la opción **Data Box Heavy**, haga clic en **Registrarse**.

   <!--![The Data Box Heavy sign up 3]()-->

5. Responda a las preguntas relativas al país de residencia de los datos, período de tiempo, servicio de Azure de destino para la transferencia de datos, ancho de banda y frecuencia de transferencia de datos. Revise los Términos y privacidad y seleccione la casilla "Microsoft can use your email address to contact you" (Microsoft puede usar su dirección de correo electrónico para ponerse en contacto con usted).

    <!--![The Data Box Heavy sign up 4]()-->

Una vez que se haya registrado y esté habilitado para la versión preliminar, puede solicitar Data Box Heavy.




