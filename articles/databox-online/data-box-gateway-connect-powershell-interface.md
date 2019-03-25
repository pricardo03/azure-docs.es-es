---
title: Conectarse y administrar el dispositivo de puerta de enlace de cuadro de datos de Microsoft Azure a través de la interfaz de Windows PowerShell | Microsoft Docs
description: Describe cómo conectarse y, a continuación, administrar puerta de enlace de datos de cuadro a través de la interfaz de Windows PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/21/2019
ms.author: alkohli
ms.openlocfilehash: 67caaa2c6c9bd615d0b88bdd5de3442b46aa32cb
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403500"
---
# <a name="manage-an-azure-data-box-gateway-device-via-windows-powershell"></a>Administración de un dispositivo de puerta de enlace del cuadro de datos de Azure a través de Windows PowerShell

Solución de puerta de enlace de datos cuadro Azure le permite enviar datos a través de la red en Azure. En este artículo se describe algunas de las tareas de configuración y administración para el dispositivo de puerta de enlace de datos cuadro. Puede usar el portal de Azure, IU web local o la interfaz de Windows PowerShell para administrar el dispositivo.

En este artículo se centra en las tareas que realizar mediante la interfaz de PowerShell.

En este artículo incluye los siguientes procedimientos:

- Conectarse a la interfaz de PowerShell
- Iniciar una sesión de soporte técnico
- Crear un paquete de soporte
- Cargar certificado
- Arrancar en un entorno sin DHCP
- Ver información del dispositivo

## <a name="connect-to-the-powershell-interface"></a>Conectarse a la interfaz de PowerShell

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Crear un paquete de soporte

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Cargar certificado

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="boot-up-in-non-dhcp-environment"></a>Arrancar en un entorno sin DHCP

[!INCLUDE [Boot up in non-DHCP environment](../../includes/data-box-edge-gateway-boot-non-dhcp.md)]

## <a name="view-device-information"></a>Ver información del dispositivo

[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="next-steps"></a>Pasos siguientes

- Implemente [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) en Azure Portal.
