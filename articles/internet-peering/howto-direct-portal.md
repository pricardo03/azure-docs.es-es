---
title: Creación o modificación de un emparejamiento directo mediante el portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento directo mediante el portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 67a4944082d3ebc14fd564eedee0310afe6e3ff5
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773983"
---
# <a name="create-or-modify-a-direct-peering-using-the-portal"></a>Creación o modificación de un emparejamiento directo mediante el portal

En este artículo se describe cómo crear un emparejamiento directo de Microsoft mediante el portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía mediante [PowerShell](howto-direct-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) y el [tutorial del emparejamiento directo](walkthrough-direct-all.md) antes de empezar la configuración.
* Si ya dispone de un emparejamiento directo con Microsoft, que no se convierte en un recurso de Azure, vea el artículo [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Creación y aprovisionamiento de un emparejamiento directo

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Creación de un emparejamiento directo

Puede crear una nueva solicitud de emparejamiento mediante el recurso **Emparejamiento**.

#### <a name="launch-resource-and-configure-basic-settings"></a>Creación de un recurso y configuración de las opciones básicas
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío de solicitudes
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name=get></a>Comprobación del emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify"></a>Modificación de un emparejamiento directo
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="delete"></a>Desaprovisionamiento de un emparejamiento directo
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md).
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md).

## <a name="additional-resources"></a>Recursos adicionales

Para más información, vea las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).
