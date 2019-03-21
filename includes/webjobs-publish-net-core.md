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
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56751369"
---
1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.

1. En el **publicar** cuadro de diálogo, seleccione **Microsoft Azure App Service**, elija **crear nuevo**y, a continuación, seleccione **publicar**.

   ![Seleccionar destino de la publicación](./media/webjobs-publish-netcore/pick-publish-target.png)

1. En el **crear App Service** cuadro de diálogo, use la configuración de hospedaje que se especifica en la tabla debajo de la imagen:

    ![Cuadro de diálogo Crear servicio de aplicaciones](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre de aplicación** | Nombre único globalmente | Nombre que identifica de forma única la nueva aplicación de función. |
    | **Suscripción** | Elija una suscripción | La suscripción de Azure que se va a usar. |
    | **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nombre del grupo de recursos en el que se va a crear la aplicación de función. Elija **Nuevo** para crear un grupo de recursos nuevo.|
    | **[Plan de hospedaje](../articles/app-service/overview-hosting-plans.md)** | Plan de App Service | Un [plan de App Service](../articles/app-service/overview-hosting-plans.md) especifica la ubicación, el tamaño y las características de la granja de servidores web que hospeda la aplicación. Para ahorrar dinero cuando hospede varias aplicaciones, configure las aplicaciones web para que compartan un único plan de App Service. Los planes de App Service definen la región, tamaño de la instancia, recuento de escala y SKU (gratis, compartido, básico, estándar o Premium). Elija **New** para crear un nuevo plan de App Service. |

1. Haga clic en **crear** para crear un trabajo Web y los recursos relacionados en Azure con esta configuración e implementar el código del proyecto.