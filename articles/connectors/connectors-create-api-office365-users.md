---
title: Conectar con los usuarios de Office 365 - Azure Logic Apps | Microsoft Docs
description: Administrar los perfiles de los usuarios de Office 365 con las API REST y Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 08/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 3865fbc4fbc39da0860218565b0a8956b2dad8ee
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105878"
---
# <a name="get-started-with-the-office-365-users-connector"></a>Introducción al conector de Usuarios de Office 365
Conéctese a Usuarios de Office 365 para obtener perfiles, buscar usuarios, etc. Con Usuarios de Office 365, puede:

* Compilar el flujo de su negocio en función de los datos que obtiene de Usuarios de Office 365. 
* Usar acciones que obtienen informes directos, obtienen el perfil de usuario de un administrador, etc. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, obtenga informes directos de la persona y, luego, tome esta información y actualice una base de datos de SQL Azure. 

Puede empezar creando una aplicación lógica ahora. Para ello, consulte [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office-365-users"></a>Creación de una conexión a Usuarios de Office 365
Al agregar este conector a las aplicaciones lógicas, debe iniciar sesión en su cuenta de Usuarios de Office 365 y permitir que estas se conecten a su cuenta.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Después de crear la conexión, especifique las propiedades de Usuarios de Office 365, como el identificador de usuario. En la **referencia de la API REST** de este artículo se describen estas propiedades.

## <a name="connector-specific-details"></a>Detalles específicos del conector

Vea los desencadenadores y las acciones definidos en Swagger y vea también todos los límites en los [detalles del conector](/connectors/officeusers/).

## <a name="more-connectors"></a>Más conectores
Volver a la [lista de API](apis-list.md).