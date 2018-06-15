---
title: Conector de Outlook.com en Azure Logic Apps | Microsoft Docs
description: Cree aplicaciones lógicas con el Servicio de aplicaciones de Azure. El conector de Outlook.com permite administrar su correo, calendarios y contactos. Puede realizar diversas acciones como enviar correo, programar reuniones, agregar contactos, etc.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 1338b7ce8f72a57cb65f9389e630fd7ebd0417b1
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295591"
---
# <a name="get-started-with-the-outlookcom-connector"></a>Get started with the Outlook.com connector (Introducción al conector de Outlook.com)
El conector de Outlook.com permite administrar su correo, calendarios y contactos. Puede realizar diversas acciones como enviar correo, programar reuniones, agregar contactos, etc.

Puede empezar creando una aplicación lógica ahora. Para ello, consulte [Creación de una nueva aplicación lógica mediante la conexión de servicios de SaaS](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-outlookcom"></a>Creación de una conexión a Outlook.com
Para crear aplicaciones lógicas con Outlook.com, primero debe crear una **conexión** y, después, especificar los detalles de las siguientes propiedades:

| Propiedad | Obligatorio | DESCRIPCIÓN |
| --- | --- | --- |
| Se necesita el cifrado de tokens |Sí |Proporcionar las credenciales de Outlook.com |

Después de crear la conexión, puede usarla para ejecutar las acciones y escuchar los desencadenadores descritos en este artículo.

> [!INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)]
>

## <a name="connector-specific-details"></a>Detalles específicos del conector

Vea los desencadenadores y las acciones definidos en Swagger y vea también todos los límites en los [detalles del conector](/connectors/outlook/).

## <a name="more-connectors"></a>Más conectores
Volver a la [lista de API](apis-list.md).