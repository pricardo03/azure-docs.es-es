---
title: Conectarse y administrar el dispositivo de puerta de enlace de cuadro de datos de Microsoft Azure a través de la interfaz de Windows PowerShell | Microsoft Docs
description: Describe cómo conectarse y, a continuación, administrar puerta de enlace de datos de cuadro a través de la interfaz de Windows PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 0ca570235ac2a87b62c5d0fcebbd24dc5186e37d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556517"
---
# <a name="manage-an-azure-data-box-gateway-device-via-windows-powershell"></a>Administración de un dispositivo de puerta de enlace del cuadro de datos de Azure a través de Windows PowerShell

Solución de puerta de enlace de datos cuadro Azure le permite enviar datos a través de la red en Azure. En este artículo se describe algunas de las tareas de configuración y administración para el dispositivo de puerta de enlace de datos cuadro. Puede usar el portal de Azure, IU web local o la interfaz de Windows PowerShell para administrar el dispositivo.

En este artículo se centra en las tareas que realizar mediante la interfaz de PowerShell.

En este artículo incluye los siguientes procedimientos:

- Conectarse a la interfaz de PowerShell
- Iniciar una sesión de soporte técnico
- Crear un paquete de soporte
- Carga del certificado
- Arrancar en un entorno sin DHCP
- Ver información del dispositivo

> [!IMPORTANT]
> Puerta de enlace de cuadro de datos de Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="connect-to-the-powershell-interface"></a>Conectarse a la interfaz de PowerShell

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="start-a-support-session"></a>Iniciar una sesión de soporte técnico

[!INCLUDE [Connect to support runspace](../../includes/data-box-edge-gateway-connect-support.md)]

## <a name="create-a-support-package"></a>Crear un paquete de soporte

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Carga del certificado

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="boot-up-in-non-dhcp-environment"></a>Arrancar en un entorno sin DHCP

[!INCLUDE [Boot up in non-DHCP environment](../../includes/data-box-edge-gateway-boot-non-dhcp.md)]

## <a name="view-device-information"></a>Ver información del dispositivo

[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="next-steps"></a>Pasos siguientes

- Implemente [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) en Azure Portal.
