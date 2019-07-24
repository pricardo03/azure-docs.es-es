---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 97387e24d5b55c1438a69da1a1fd0a9bc1720e47
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186474"
---
1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.

1. En el cuadro de diálogo **Publicar**, seleccione **Microsoft Azure App Service**, elija **Crear nuevo** y haga clic en **Publicar**.

   ![Selección del destino de publicación](./media/webjobs-publish-netcore/pick-publish-target.png)

1. En el cuadro de diálogo **Crear servicio de aplicaciones**, use la configuración de hospedaje que se especifica en la tabla que hay debajo de la imagen:

    ![Cuadro de diálogo Crear servicio de aplicaciones](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre de aplicación** | Nombre único globalmente | Nombre que identifica de forma única la nueva aplicación de función. |
    | **Suscripción** | Elija una suscripción | La suscripción de Azure que se va a usar. |
    | **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nombre del grupo de recursos en el que se va a crear la aplicación de función. Elija **Nuevo** para crear un grupo de recursos nuevo.|
    | **[Plan de hospedaje](../articles/app-service/overview-hosting-plans.md)** | Plan de App Service | Un [plan de App Service](../articles/app-service/overview-hosting-plans.md) especifica la ubicación, el tamaño y las características de la granja de servidores web que hospeda la aplicación. Para ahorrar dinero cuando hospede varias aplicaciones, configure las aplicaciones web para que compartan un único plan de App Service. Los planes de App Service definen la región, el tamaño de la instancia, el recuento de escala y la SKU (gratis, compartida, básica, estándar o premium). Seleccione **Nuevo** para crear un plan de App Service. |

1. Haga clic en **Crear** para crear una instancia de WebJobs y los recursos relacionados en Azure con esta configuración e implemente el código del proyecto.