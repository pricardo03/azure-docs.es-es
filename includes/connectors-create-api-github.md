---
title: archivo de inclusión
description: archivo de inclusión
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: 11280e1678f52ede928cb2a85ea83add222e15fa
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66149224"
---
1. En [Azure Portal](https://portal.azure.com), cree una aplicación lógica en blanco. 

2. En el Diseñador de aplicaciones lógicas, escriba "github" como filtro. 

3. Seleccione el conector y el desencadenador de GitHub que desea usar.

   ![Selección del conector y el desencadenador de GitHub](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Todos los flujos de trabajo de aplicaciones lógicas deben comenzar con un desencadenador. Puede seleccionar acciones solo cuando el flujo de trabajo de lógica ya se inicia con un desencadenador. 

4. Si anteriormente no creó una conexión, elija **Iniciar sesión** para proporcionar sus credenciales de GitHub cuando se le soliciten.  

   ![Inicio de sesión con sus credenciales de GitHub](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   La aplicación lógica usa estas credenciales para autorizar la conexión y el acceso a los datos a la cuenta de GitHub. 

5. Indique su nombre de usuario y contraseña de GitHub y luego confirme su autorización.

   ![Provisión de credenciales y confirmación de la autorización](./media/connectors-create-api-github/github-connector-authorize.png)   

   Ya se ha creado la conexión en Azure Portal y está lista para su uso.

6. Continúe definiendo el flujo de trabajo de aplicación lógica.

   ![Adición de más acciones al flujo de trabajo de la aplicación lógica](./media/connectors-create-api-github/github-connector-logic-app.png)

