---
title: archivo de inclusión
description: archivo de inclusión
services: azure-digital-twins
author: alinamstanciu
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/19/2018
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: d08b7a1256a26d1d7d39481ba15a8637339063ea
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322972"
---
1. En [Azure Portal](https://portal.azure.com), abra **Azure Active Directory** desde el panel de navegación izquierdo y, luego, abra el panel **Propiedades**. Copie el **identificador de directorio** en un archivo temporal. Usará este valor para configurar la aplicación de ejemplo en la sección siguiente.

    ![Identificador de directorio de Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. Abra el panel **Registros de aplicaciones** y, luego, haga clic en el botón **Nuevo registro de aplicaciones**.
    
    ![Nuevo registro de aplicaciones de Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-start.png)

1. Asigne un nombre descriptivo a este registro de aplicaciones en el campo **Nombre**. En **Tipo de aplicación**, elija **_Nativa_**, y en **URI de redirección**, seleccione **_https://microsoft.com_**. Haga clic en **Create**(Crear).

    ![Creación de registro de aplicaciones de Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Abra la aplicación registrada y copie el valor del campo **Id. de la aplicación** en un archivo temporal; este valor identifica la aplicación de Azure Active Directory. Usará el identificador de la aplicación para configurar la aplicación de ejemplo en las secciones siguientes.

    ![Identificador de la aplicación de Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Abra el panel de registro de aplicaciones y haga clic en **Configuración** > **Permisos necesarios**:
    - Haga clic en **Agregar** en la parte superior izquierda para abrir el panel **Agregar acceso de API**.
    - Haga clic en **Seleccionar una API** y busque **Azure Digital Twins**. Si la búsqueda no encuentra la API, busque en su lugar **Azure Smart Spaces**.
    - Seleccione la opción **Azure Digital Twins (Azure Smart Spaces Service)** y haga clic en **Seleccionar**.
    - Haga clic en **Seleccionar permisos**. Maque la casilla de permisos delegados **Acceso de lectura y escritura** y haga clic en **Seleccionar**.
    - En la página **Agregar acceso de API**, haga clic en **Listo**.
    - En el panel **Permisos necesarios**, haga clic en el botón **Conceder permisos** y acepte la confirmación que aparece.

       ![Registro de aplicaciones de Azure Active Directory: agregar API](./media/digital-twins-permissions/aad-app-req-permissions.png)
