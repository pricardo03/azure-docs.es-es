---
title: 'Tutorial: Supervisión de recursos de Azure Spring Cloud mediante alertas y grupos de acciones | Microsoft Docs'
description: Aprenda a usar las alertas de Spring Cloud.
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/18/2019
ms.openlocfilehash: 2be21b20c394ae8505ad18f2c411db7aab06215f
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689570"
---
# <a name="tutorial-monitor-spring-cloud-resources-using-alerts-and-action-groups"></a>Tutorial: Supervisión de recursos de Spring Cloud mediante alertas y grupos de acciones

Las alertas de Azure Spring Cloud permiten supervisar los recursos en función de condiciones tales como el almacenamiento disponible, la tasa de solicitudes o el uso de datos. Una alerta envía una notificación cuando las condiciones cumplen las especificaciones definidas.

Hay dos pasos para configurar una canalización de alertas: 
1. Configure un grupo de acciones con las acciones que se llevarán a cabo cuando se desencadene una alerta; por ejemplo, correo electrónico, SMS, runbook o webhook. Los grupos de acciones se pueden volver a usar con distintas alertas.
2. Configure las reglas de alerta. Las reglas asocian los patrones de las métricas con los grupos de acciones según el recurso de destino, la métrica, la condición, la agregación de tiempo, etc.

## <a name="prerequisites"></a>Requisitos previos
Además de los requisitos de Azure Spring, este tutorial depende de los siguientes recursos.

* Una instancia de Azure Spring Cloud implementada.  Siga nuestro [inicio rápido](spring-cloud-quickstart-launch-app-cli.md) para comenzar.

* El recurso de Azure que se va a supervisar; por ejemplo, la base de datos que se implementa en este artículo: [Uso de Spring Data Apache Cassandra API con Azure Cosmos DB](https://docs.microsoft.com/azure/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
 
Los procedimientos siguientes inicializan **Grupo de acciones** y **Alertas** a partir de la opción **Alertas** situada en el panel de navegación izquierdo de una instancia de Spring Cloud. (El procedimiento también se puede iniciar desde la página **Información general de Monitor**, en Azure Portal). 

Vaya desde un grupo de recursos a la instancia de Spring Cloud. Seleccione **Alertas** en el panel izquierdo y, a continuación, seleccione **Administrar acciones**:

![Captura de pantalla del grupo de recursos del portal](media/alerts-action-groups/action-1-a.png)

## <a name="set-up-action-group"></a>Configuración de un grupo de acciones

Para comenzar el procedimiento para inicializar un nuevo **Grupo de acciones**, seleccione **+ Agregar grupo de acciones**.

![Captura de pantalla del portal con la opción Agregar grupo de acciones](media/alerts-action-groups/action-1.png)

En la página **Agregar grupo de acciones**:

 1. Especifique los valores de **Nombre del grupo de acciones** y **Nombre corto**.

 1. Especifique los valores de **Suscripción** y **Grupo de recursos**.

 1. Especifique **Nombre de acción**.

 1. Seleccione **Tipo de acción**.  Se abrirá otro panel a la derecha para definir la acción que se realizará en la activación.

 1. Defina la acción con las opciones del panel derecho.  En este caso se usa la notificación por correo electrónico.

 1. Haga clic en **Aceptar** en el panel de acciones de la derecha.

 1. Haga clic en **Aceptar** en el cuadro de diálogo **Agregar grupo de acciones**. 

  ![Captura de pantalla del portal con la definición de la acción](media/alerts-action-groups/action-2.png)

## <a name="set-up-alert"></a>Configuración de una alerta 

En los pasos anteriores se creó un **Grupo de acciones** que usa notificaciones por correo electrónico. También puede usar notificaciones por teléfono, webhooks, Azure Functions, etc.  

Para configurar una **Alerta**, vuelva a la página **Alertas** y haga clic en **Administrar reglas de alerta**.

  ![Captura de pantalla del portal con la definición de la alerta](media/alerts-action-groups/alerts-2.png)

1. Seleccione el valor de **Recurso** para la alerta.

1. Haga clic en **Nueva regla de alertas**.

  ![Captura de pantalla del portal con la nueva regla de alerta](media/alerts-action-groups/alerts-3.png)

1. En la página **Crear regla**, especifique los valores de **RECURSO**, **CONDICIÓN** y **ACCIONES**.  En el panel **Acciones**, seleccione el **Grupo de acciones** definido previamente.

1. En **DETALLES DE LA ALERTA**, asigne un nombre a la regla de alerta.

1. Haga clic en **Crear regla de alertas**.

  ![Captura de pantalla del portal con la nueva regla de alerta](media/alerts-action-groups/alerts-4.png)

Compruebe que la nueva regla de alerta esté habilitada.

  ![Captura de pantalla del portal con la nueva regla de alerta](media/alerts-action-groups/alerts-5.png)

## <a name="next-steps"></a>Pasos siguientes
* [Creación y administración de grupos de acciones en Azure Portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* [Comportamiento de las alertas por SMS en los grupos de acciones](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-sms-behavior)
* [Tutorial: Uso del seguimiento distribuido con Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
