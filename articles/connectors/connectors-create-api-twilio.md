---
title: Incorporación del conector de Twilio a Azure Logic Apps | Microsoft Docs
description: Información general del conector de Twilio con parámetros de la API de REST
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 8bcf69a7c8e04cb45d795fd0d6f20d477c15865d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38652012"
---
# <a name="get-started-with-the-twilio-connector"></a>Introducción al conector de Twilio
Conectarse a Twilio para enviar y recibir mensajes SMS, MMS y IP globales. Con Twilio, puede:

* Compilar el flujo de negocio en función de los datos que obtiene de Twilio. 
* Usar acciones que obtienen un mensaje, enumeran mensajes y mucho más. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, cuando reciba un nuevo mensaje de Twilio, puede usarlo en un flujo de trabajo de Service Bus. 

Para empezar, cree una aplicación lógica; consulte [Creación de su primer flujo de trabajo de aplicación lógica para automatizar los procesos entre aplicaciones de nube y servicios en la nube](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-twilio"></a>Creación de una conexión a Twilio
Cuando agregue este conector a las aplicaciones lógicas, escriba los siguientes valores de Twilio:

| Propiedad | Obligatorio | DESCRIPCIÓN |
| --- | --- | --- |
| Id. de cuenta |Sí |Escriba el identificador de cuenta de Twilio |
| Token de acceso |Sí |Escriba el token de acceso de Twilio |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Si no tiene un token de acceso de Twilio, consulte [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity) (Tokens de identidad de usuario y de acceso).

## <a name="connector-specific-details"></a>Detalles específicos del conector

Vea los desencadenadores y las acciones definidos en Swagger y vea también todos los límites en los [detalles del conector](/connectors/twilio/).

## <a name="more-connectors"></a>Más conectores
Volver a la [lista de API](apis-list.md).