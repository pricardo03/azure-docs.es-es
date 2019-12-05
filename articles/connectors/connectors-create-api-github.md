---
title: Conexión a GitHub
description: Supervisar los eventos de GitHub con las API REST de GitHub y Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: 7dc865f520b6f4667ace720e656a210e0252d1a1
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789757"
---
# <a name="connect-to-github-from-azure-logic-apps"></a>Conexión a GitHub desde Azure Logic Apps

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

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica acerca de los desencadenadores, las acciones y los límites, que se detallan en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia de los conectores](/connectors/github/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)