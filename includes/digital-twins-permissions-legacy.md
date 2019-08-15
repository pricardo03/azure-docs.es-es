---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 9f4bf6fb92b590e274e8880b5f900e5469f85727
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012278"
---
1. En [Azure Portal](https://portal.azure.com), abra **Azure Active Directory** desde el panel izquierdo y, luego, abra el panel **Propiedades**. Copie el **identificador de directorio** en un archivo temporal. Usará este valor para configurar una aplicación de ejemplo en la sección siguiente.

    ![Identificador de directorio de Azure Active Directory](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)

1. En [Azure Portal](https://portal.azure.com), abra **Azure Active Directory** desde el panel izquierdo y, luego, abra el panel **Registros de aplicaciones (heredado)** . Seleccione el botón **Nuevo registro de aplicaciones**.

1. Asigne un nombre descriptivo a este registro de aplicaciones en el cuadro **Nombre**. Para **Tipo de aplicación**, elija **Nativa** y para **URI de redirección**, elija `https://microsoft.com`. Seleccione **Crear**.

    ![Crear panel](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)

1.  Abra la aplicación registrada y copie el valor del campo **Id. de aplicación** en un archivo temporal. Este valor identifica la aplicación de Azure Active Directory. Usará el identificador de la aplicación para configurar la aplicación de ejemplo en las secciones siguientes.

    ![Identificador de la aplicación de Azure Active Directory](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)

1. Abra el panel de registro de aplicaciones. Seleccione **Configuración** > **Permisos necesarios** y, luego:

   a. Haga clic en **Agregar** en la parte superior izquierda para abrir el panel **Agregar acceso de API**.

   b. Haga clic en **Seleccionar una API** y busque **Azure Digital Twins**. Si la búsqueda no encuentra la API, busque en su lugar **Azure Smart Spaces**.

   c. Seleccione la opción **Azure Digital Twins (Azure Smart Spaces Service)** y haga clic en **Seleccionar**.

   d. Elija **Seleccionar permisos**. Active la casilla de permisos delegados **Acceso de lectura y escritura** y elija **Seleccionar**.

   e. En la página **Agregar acceso de API**, haga clic en **Hecho**.

   f. En el panel **Permisos necesarios**, haga clic en el botón **Conceder permisos** y acepte la confirmación que aparece. Si no se concede el permiso para esta API, póngase en contacto con el administrador.

      ![Panel Permisos necesarios](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)

 