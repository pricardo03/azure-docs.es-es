---
title: Seguridad de borde del cuadro de datos de Azure | Microsoft Docs
description: Describe las características de seguridad y privacidad que protegen su dispositivo de borde del cuadro de datos de Azure, servicio y datos locales y en la nube.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 5316ddf9d456731f2789241434926366f732993a
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682107"
---
# <a name="azure-data-box-edge-security-and-data-protection"></a>Protección de datos y seguridad de borde del cuadro de datos Azure

Seguridad es una preocupación importante adoptar una nueva tecnología, especialmente si se usa la tecnología con datos confidenciales o propios. Solución de borde del cuadro de datos de Microsoft Azure ayuda a garantizar que solo las entidades autorizadas pueden ver, modificar o eliminar los datos.

En este artículo se describe las características de seguridad de borde del cuadro de datos que ayudan a proteger cada uno de los componentes de la solución y los datos almacenados en ellas.

La solución de borde del cuadro de datos de Azure consta de cuatro componentes principales que interactúan entre sí:

- **Servicio de borde del cuadro de datos hospedado en Azure** : el recurso de administración que usar para crear el orden de los dispositivos, configurar el dispositivo y, a continuación, controlar el orden hasta su finalización.
- **Dispositivo de borde del cuadro de datos** : el dispositivo de transferencia que se envía al importar los datos en el entorno local a Azure.
- **Clientes/hosts conectados al dispositivo** : los clientes de la infraestructura que se conectan al dispositivo de borde del cuadro de datos y contienen datos que deben protegerse.
- **Almacenamiento en la nube** : la ubicación en la nube de Azure donde se almacenan los datos. Esta ubicación es normalmente la cuenta de almacenamiento vinculada al recurso de borde del cuadro de datos que ha creado.

## <a name="data-box-edge-service-protection"></a>Protección de servicio del borde del cuadro de datos

El servicio de borde del cuadro de datos es un servicio de administración hospedado en Microsoft Azure. El servicio se usa para configurar y administrar el dispositivo.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-edge-device-protection"></a>Protección de dispositivos de borde del cuadro de datos

El dispositivo de borde del cuadro de datos es un dispositivo local que ayuda a transformar los datos mediante procesarlo localmente y, a continuación, enviarlos a Azure. El dispositivo:

- Necesita una clave de activación para tener acceso al servicio de borde del cuadro de datos.
- Se protege en todo momento mediante una contraseña de dispositivo.
- Es un dispositivo bloqueado. El dispositivo de BMC y BIOS están protegidas con contraseña con acceso de usuario limitado para que el BIOS.
- Ha habilitado el arranque seguro.
- Se ejecuta Device Guard de Windows Defender. Device Guard le permite ejecutar solo las aplicaciones de confianza que se definen en las directivas de integridad de código.

### <a name="protect-the-device-via-activation-key"></a>Proteger el dispositivo a través de la clave de activación

Solo un dispositivo de borde del cuadro de datos autorizado tiene permiso para unir el servicio de borde del cuadro de datos que creó en su suscripción de Azure. Para autorizar un dispositivo, debe usar una clave de activación para activar el dispositivo con el servicio de borde del cuadro de datos.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Para obtener más información, vaya a [obtener una clave de activación](data-box-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Proteger el dispositivo a través de la contraseña

Las contraseñas de asegurarse de que los datos son accesibles sólo a usuarios autorizados. Arranque de dispositivos de borde del cuadro datos seguridad en un estado bloqueado.

Puede:

- Conéctese a la IU del dispositivo mediante un explorador web local y, a continuación, proporcione una contraseña para iniciar sesión en el dispositivo.
- Conectarse de forma remota a la interfaz de PowerShell del dispositivo a través de HTTP. Administración remota está activada de forma predeterminada. A continuación, puede proporcionar la contraseña del dispositivo para iniciar sesión en el dispositivo. Para obtener más información, vaya a [conexión remota al dispositivo de borde del cuadro de datos](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Use local a la interfaz de usuario de web [cambiar la contraseña](data-box-edge-manage-access-power-connectivity-mode.md#manage-device-access). Si cambia la contraseña, asegúrese de notificar a todos los usuarios de acceso remoto para que no experimenten un error de inicio de sesión.

## <a name="protect-the-data"></a>Proteger los datos

En esta sección se describe las características de seguridad de borde del cuadro de datos que protegen los datos en tránsito y los datos almacenados.

### <a name="protect-data-at-rest"></a>Protección de los datos en reposo

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Proteger datos en tránsito

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Protección de datos a través de cuentas de almacenamiento

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Girar y, a continuación, [sincronizar las claves de cuenta de almacenamiento](data-box-edge-manage-shares.md#sync-storage-keys) con regularidad para ayudar a garantizar que la cuenta de almacenamiento no tiene acceso a los usuarios no autorizados.

## <a name="manage-personal-information"></a>Administrar la información personal

El servicio de borde del cuadro de datos recopila información personal en las instancias de la claves siguientes:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Para ver la lista de usuarios que pueden acceder o eliminar un recurso compartido, siga los pasos descritos en [administrar recursos compartidos en el borde del cuadro de datos](data-box-edge-manage-shares.md).

Para obtener más información, revise la directiva de privacidad de Microsoft en el [Centro de confianza](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Pasos siguientes

[Implementar el dispositivo de borde del cuadro datos](data-box-edge-deploy-prep.md).
