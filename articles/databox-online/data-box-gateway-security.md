---
title: Seguridad de la puerta de enlace de datos cuadro Azure | Microsoft Docs
description: Describe las características de seguridad y privacidad que protegen su dispositivo virtual de puerta de enlace del cuadro de datos de Azure, servicio y datos, de forma local y en la nube.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 230d1a28ba15a8736e46c02cb08217a28fc18599
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754359"
---
# <a name="azure-data-box-gateway-security-and-data-protection"></a>Protección de datos y seguridad de puerta de enlace de datos cuadro Azure

Seguridad es una preocupación importante está adoptando una tecnología nueva, especialmente si se usa la tecnología con datos confidenciales o propios. Azure ayuda puerta de enlace de datos cuadro se asegura de que solo en las entidades autorizadas puede ver, modificar o eliminar los datos.

En este artículo se describe las características de seguridad de la puerta de enlace del cuadro de datos de Azure que ayudan a proteger cada uno de los componentes de la solución y los datos almacenados en ellas.

La solución de puerta de enlace de datos cuadro consta de cuatro componentes principales que interactúan entre sí:

- **Servicio de puerta de enlace de cuadro de datos, hospedado en Azure**. El recurso de administración que usar para crear el orden de los dispositivos, configurar el dispositivo y, a continuación, controlar el orden hasta su finalización.
- **El dispositivo del cuadro puerta de enlace de datos**. El dispositivo virtual que se aprovisiona en el hipervisor del sistema que proporcione. Este dispositivo virtual se usa para importar los datos de forma local en Azure.
- **Clientes/hosts conectados al dispositivo**. Los clientes de la infraestructura que se conectan al dispositivo de puerta de enlace de datos cuadro y contienen datos que deben protegerse.
- **Almacenamiento en nube**. La ubicación en la plataforma de nube de Azure donde se almacenan los datos. Esta ubicación es normalmente la cuenta de almacenamiento vinculada al recurso de puerta de enlace de datos cuadro que cree.


## <a name="data-box-gateway-service-protection"></a>Protección de servicio del cuadro puerta de enlace de datos

El servicio de puerta de enlace de datos cuadro es un servicio de administración que se hospeda en Azure. El servicio se usa para configurar y administrar el dispositivo.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-gateway-device-protection"></a>Protección de dispositivos del cuadro puerta de enlace de datos

El dispositivo de puerta de enlace de datos cuadro es un dispositivo virtual que se aprovisiona en el hipervisor de un sistema local que proporcione. El dispositivo le ayuda a enviar datos a Azure. El dispositivo:

- Necesita una clave de activación para tener acceso al servicio de puerta de enlace de datos cuadro Edge/datos de cuadro.
- Se protege en todo momento mediante una contraseña de dispositivo.
<!---  secure boot enabled.
- Runs Windows Defender Device Guard. Device Guard allows you to run only trusted applications that you define in your code integrity policies.-->

### <a name="protect-the-device-via-activation-key"></a>Proteger el dispositivo a través de la clave de activación

Se permite solo un dispositivo de puerta de enlace de datos cuadro autorizado a unirse al servicio de puerta de enlace de datos cuadro que se crea en su suscripción de Azure. Para autorizar un dispositivo, deberá usar una clave de activación para activar el dispositivo con el servicio de puerta de enlace de datos cuadro.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Para obtener más información, consulte [obtener una clave de activación](data-box-gateway-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Proteger el dispositivo a través de la contraseña

Las contraseñas de asegurarse de que solo los usuarios autorizados pueden acceder a los datos. Arranque de dispositivos de puerta de enlace de cuadro datos seguridad en un estado bloqueado.

Puede:

- Conéctese a la IU del dispositivo mediante un explorador web local y, a continuación, proporcione una contraseña para iniciar sesión en el dispositivo.
- Conectarse de forma remota a la interfaz de PowerShell del dispositivo a través de HTTP. Administración remota está activada de forma predeterminada. A continuación, puede proporcionar la contraseña del dispositivo para iniciar sesión en el dispositivo. Para obtener más información, consulte [conexión remota al dispositivo de puerta de enlace de datos cuadro](data-box-gateway-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Use local a la interfaz de usuario de web [cambiar la contraseña](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Si cambia la contraseña, asegúrese de notificar a todos los usuarios de acceso remoto para que no tengan problemas al iniciar sesión.


## <a name="protect-your-data"></a>Proteja sus datos

Esta sección describen las características de seguridad de la puerta de enlace de datos cuadro que protejan los datos en tránsito y se almacenan.

### <a name="protect-data-at-rest"></a>Protección de los datos en reposo

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Proteger datos en tránsito

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Protección de datos a través de cuentas de almacenamiento

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Girar y, a continuación, [sincronizar las claves de cuenta de almacenamiento](data-box-gateway-manage-shares.md#sync-storage-keys) periódicamente para ayudar a proteger su cuenta de almacenamiento de los usuarios no autorizados.

## <a name="manage-personal-information"></a>Administrar la información personal

El servicio de puerta de enlace de datos cuadro recopila información personal en los escenarios siguientes:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Para ver la lista de usuarios que pueden acceder o eliminar un recurso compartido, siga los pasos descritos en [administrar recursos compartidos en la puerta de enlace de datos de cuadro](data-box-gateway-manage-shares.md).

Para obtener más información, revise la directiva de privacidad de Microsoft en la [centro de confianza](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Pasos siguientes

[Implementar el dispositivo de puerta de enlace de datos cuadro](data-box-gateway-deploy-prep.md)
