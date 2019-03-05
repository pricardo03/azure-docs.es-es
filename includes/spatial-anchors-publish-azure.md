---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 3ee9aca1ee86b04998e357875b98b14782b3e56d
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56963388"
---
### <a name="launch-the-publish-wizard"></a>Inicio del Asistente para publicación

En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **SharingService** y seleccione **Publicar**.

El Asistente para publicación se inicia automáticamente. Seleccione **App Service** > **Publicar** para abrir el cuadro de diálogo **Crear servicio de aplicaciones**.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

En el cuadro de diálogo **Crear App Service**, haga clic en **Agregar una cuenta** e inicie sesión en su suscripción de Azure. Si ya ha iniciado sesión, seleccione la cuenta que desee en la lista desplegable.

> [!NOTE]
> Si ya ha iniciado sesión, no seleccione **Crear** todavía.
>

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [resource group intro text](resource-group.md)]

Junto a **Grupo de recursos**, seleccione **Nuevo**.

Asigne el nombre **myResourceGroup** al grupo de recursos y seleccione **Aceptar**.

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [app-service-plan](app-service-plan.md)]

Junto a **Plan de hospedaje**, seleccione **Nuevo**.

En el cuadro de diálogo **Configurar un plan de hospedaje**, use la configuración de la tabla.

| Configuración | Valor sugerido | DESCRIPCIÓN |
|-|-|-|
|Plan de servicio de aplicación| MySharingServicePlan | Nombre del plan de App Service. |
| Ubicación | Oeste de EE. UU. | El centro de datos donde se hospeda la aplicación web. |
| Tamaño | Gratuito | [Plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) determina las características de hospedaje. |

Seleccione **Aceptar**.

### <a name="create-and-publish-the-web-app"></a>Creación y publicación de la aplicación web

En **Nombre de la aplicación**, escriba un nombre único de aplicación (los caracteres válidos son `a-z`, `0-9` y `-`) o acepte el nombre único generado automáticamente. La dirección URL de la aplicación web es `https://<app_name>.azurewebsites.net`, donde `<app_name>` es el nombre de la aplicación.

Seleccione **Crear** para comenzar a crear los recursos de Azure.

Una vez completado el asistente, publica la aplicación web ASP.NET Core en Azure e inicia la aplicación en el explorador predeterminado.

![Aplicación web de ASP.NET publicada en Azure](./media/spatial-anchors-azure/web-app-running-live.png)

El nombre de aplicación especificado en el [paso de creación y publicación](#create-and-publish-the-web-app) se usa como el prefijo de dirección URL en el formato `https://<app_name>.azurewebsites.net`. Anote esta dirección URL, ya que se usará más adelante.
