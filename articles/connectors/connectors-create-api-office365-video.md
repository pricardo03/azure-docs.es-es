---
title: Conectarse a Office 365 Video - Azure Logic Apps | Microsoft Docs
description: Administrar vídeos con las API REST de Office 365 Video y Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 44152101f1a2be6268d4e27430fdf60ef0131fae
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295785"
---
# <a name="get-started-with-the-office365-video-connector"></a>Introducción al conector de Office 365 Video
Conéctese a Office 365 Video para conseguir información acerca de un vídeo Office 365 específico, obtener una lista de vídeos y mucho más. Con Office 365 Video puede:

* Compilar el flujo de su negocio en función de los datos que obtiene de Office 365 Video. 
* Usar acciones que comprueban el estado del portal de vídeo, obtener una lista de todos los vídeos en un canal y mucho más. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, puede utilizar el conector de Búsqueda de Bing para buscar vídeos de Office 365 y, después, usar el conector de Office 365 Video para conseguir información sobre ese vídeo. Si el vídeo cumple sus requisitos, puede publicarlo en Facebook. 

Puede empezar creando una aplicación lógica ahora. Para ello, consulte [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office365-video-connector"></a>Creación de una conexión al conector de Office 365 Video
Al agregar este conector a las aplicaciones lógicas, debe iniciar sesión en su cuenta de Office 365 Video y permitir que estas se conecten a su cuenta.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]
> 
> 

Después de crear la conexión, especifique las propiedades del vídeo de Office 365, como el nombre de inquilino o el identificador del canal. 


## <a name="connector-specific-details"></a>Detalles específicos del conector

Vea los desencadenadores y las acciones definidos en Swagger y vea también todos los límites en los [detalles del conector](/connectors/office365videoconnector/).

## <a name="more-connectors"></a>Más conectores
Volver a la [lista de API](apis-list.md).