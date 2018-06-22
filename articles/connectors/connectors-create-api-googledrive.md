---
title: Conectarse a Google Drive - Azure Logic Apps | Microsoft Docs
description: Crear y administrar archivos con las API REST de Google Drive y Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 4994687afbdd23f0265138cd1d4eff53bb47c163
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295890"
---
# <a name="get-started-with-the-google-drive-connector"></a>Introducción al conector de Google Drive
Conéctese a Google Drive para crear archivos, obtener filas, etc. Con Google Drive, puede: 

* Compilar el flujo de negocio en función de los datos obtenidos de la búsqueda. 
* Usar acciones para buscar imágenes, noticias, etc. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, puede buscar un vídeo y luego usar Twitter para publicar ese vídeo en una fuente de Twitter.

Puede empezar creando una aplicación lógica ahora. Para ello, consulte [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-the-connection-to-google-drive"></a>Creación de la conexión a Google Drive
Al agregar este conector a las aplicaciones lógicas, debe autorizar a estas para que se conecten a su Google Drive.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

Después de crear la conexión, especifique las propiedades de Google Drive, como la ruta de acceso a la carpeta o el nombre de archivo. 

## <a name="connector-specific-details"></a>Detalles específicos del conector

Vea los desencadenadores y las acciones definidos en Swagger y vea también todos los límites en los [detalles del conector](/connectors/googledrive/).

## <a name="more-connectors"></a>Más conectores
Volver a la [lista de API](apis-list.md).
