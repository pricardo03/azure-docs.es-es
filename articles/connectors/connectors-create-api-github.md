---
title: Conectarse a GitHub - Azure Logic Apps | Microsoft Docs
description: Supervisar los eventos de GitHub con las API REST de GitHub y Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: ce567dc631c3a147b795eb2355a4961faa8881d6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295819"
---
# <a name="connect-to-github"></a>Conexión a GitHub

GitHub es un servicio de almacenaje de repositorios Git basado en web. Ofrece toda la funcionalidad de control de revisión distribuido y de administración de código fuente (SCM) en Git, además de otras características.

Para empezar a trabajar con el conector de GitHub, [cree primero una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Creación de una conexión a GitHub

Para usar el conector de GitHub en una aplicación de lógica, debe crear primero una *conexión* y luego especificar los detalles de estas propiedades: 

| Propiedad | Obligatorio | DESCRIPCIÓN | 
| -------- | -------- | ----------- | 
| Se necesita el cifrado de tokens | Sí | Proporcione las credenciales de GitHub. |

Después de crear la conexión, puede ejecutar las acciones y escuchar los desencadenadores descritos en este artículo.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Detalles específicos del conector

Para los desencadenadores y las acciones definidos en Swagger y los posibles límites, revise los [detalles del conector](/connectors/github/).

## <a name="find-more-connectors"></a>Búsqueda de más conectores

* Revise la [lista de conectores](apis-list.md).