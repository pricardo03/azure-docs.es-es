---
title: Habilitación de Peering Service en un emparejamiento directo mediante el portal
titleSuffix: Azure
description: Habilitación de Peering Service en un emparejamiento directo mediante el portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d6b67c42ef8a5ba5ae98894775d1f56cee39ba8b
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773875"
---
# <a name="enable-peering-service-on-a-direct-peering-using-the-portal"></a>Habilitación de Peering Service en un emparejamiento directo mediante el portal

Este artículo describe cómo habilitar [Peering Service](overview-peering-service.md) en un emparejamiento directo mediante el portal.

Si lo prefiere, puede completar esta guía con [PowerShell](howto-peering-service-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) antes de comenzar la configuración.
* Elija un emparejamiento directo en la suscripción en la que desea habilitar Peering Service. Si no tiene una, convierta un emparejamiento directo heredado o cree un nuevo emparejamiento directo.
    * Para convertir un emparejamiento directo heredado, siga las instrucciones de [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md).
    * Para crear un nuevo emparejamiento directo, siga las instrucciones de [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md).

## <a name="enable-peering-service-on-a-direct-peering"></a>Habilitación de Peering Service en un emparejamiento directo

### <a name= get></a>Visualización de un emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name= get></a>Habilitación del emparejamiento directo para Peering Service

Después de abrir el emparejamiento directo en el paso anterior, habilítelo para Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Modificación de una conexión de emparejamiento directo

Si tiene que modificar la configuración de la conexión, consulte la sección **Modificación de un emparejamiento directo** en [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para ver las preguntas más frecuentes, consulte [Preguntas más frecuentes sobre Peering Service](service-faqs.md).