---
title: Conexión con Box
description: Creación y administración de archivos con las API REST y Azure Logic Apps de Box
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 38e061e918d445de07961af1789891f44c59090a
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789867"
---
# <a name="create-and-manage-files-in-box-with-azure-logic-apps"></a>Creación y administración de archivos de Box con Azure Logic Apps

En este artículo se muestra cómo puede crear y administrar archivos de Box dentro de una aplicación lógica con el conector de Box. De este modo, puede crear aplicaciones lógicas que automatizan tareas y flujos de trabajo para administrar los archivos y otras acciones, como por ejemplo:

* Compilar el flujo de negocio en función de los datos que obtiene de Box.

* Desencadenar tareas y flujos de trabajo automatizados cuando se crea o actualiza un archivo.

* Ejecutar una acción que copia o elimina un archivo.

  Si estas acciones obtienen una respuesta, dejan la salida a disposición de otras acciones. 
  Por ejemplo, cuando se modifique un archivo en Box, puede enviarlo por correo electrónico mediante Office 365.

## <a name="prerequisites"></a>Requisitos previos

* Una [cuenta de Box](https://www.box.com/home)

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* La aplicación lógica desde donde quiere acceder a la cuenta de Box. Para iniciar la aplicación lógica con un desencadenador de Box, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Si no está familiarizado con las aplicaciones lógicas, vea [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo OpenAPI (antes Swagger) del conector, consulte la [página de referencia del conector](/connectors/box/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)