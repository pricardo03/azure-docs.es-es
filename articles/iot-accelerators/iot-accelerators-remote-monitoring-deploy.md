---
title: 'Implementación de la solución Supervisión remota: Azure | Microsoft Docs'
description: En este tutorial, se explica cómo aprovisionar el acelerador de la solución Supervisión remota desde azureiotsuite.com.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: 42f6afcd3cb6880ba6c9cdd2a51e2a3e9ff2c2d4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626860"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator"></a>Implementación del acelerador de la solución Supervisión remota

En este tutorial, se muestra cómo aprovisionar el acelerador de la solución Supervisión remota. Puede implementar la solución desde azureiotsuite.com. También puede implementar la solución con la CLI. Para información sobre esta opción, consulte el artículo sobre la [implementación del acelerador de una solución desde la línea de comandos](iot-accelerators-remote-monitoring-deploy-cli.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configuración del acelerador de la solución
> * Implementación del acelerador de solución
> * Inicio de sesión en el acelerador de la solución

## <a name="prerequisites"></a>requisitos previos

Para completar este tutorial, deberá tener una suscripción activa de Azure.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-solution-accelerator"></a>Implementación del acelerador de solución

Antes de implementar el acelerador de la solución en la suscripción de Azure, debe elegir algunas opciones de configuración:

1. Inicie sesión en [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) con las credenciales de la cuenta de Azure.

1. Haga clic en **Intentar ahora** en el icono **Supervisión remota**.

    ![Elegir Supervisión remota](./media/iot-accelerators-remote-monitoring-deploy/remotemonitoring.png)

1. En la página **Create Remote Monitoring solution** (Crear la solución Supervisión remota), escriba un **nombre de solución** para el acelerador de la solución Supervisión remota.

1. Seleccione una implementación **Básica** o **Estándar**. Si implementa la solución para saber cómo funciona o para ejecutar una demostración, elija la opción **Básica** para minimizar los costos.

1. Elija **Java** o **.NET** como lenguaje. Todos los microservicios están disponibles como implementaciones Java o .NET.

1. Revise el panel de **detalles de la solución** para más información sobre sus opciones de configuración.

1. Seleccione la **Suscripción** y **Región** que desea usar para aprovisionar la solución.

1. Haga clic en **Crear solución** para comenzar el proceso de aprovisionamiento. Este proceso normalmente tarda varios minutos en ejecutarse:

    ![Detalles de la solución de supervisión remota](./media/iot-accelerators-remote-monitoring-deploy/createform.png)

Para información sobre la solución de problemas, consulte la sección sobre [lo que se debe hacer cuando se produce un error en una implementación](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) del repositorio GitHub.

## <a name="sign-in-to-the-solution-accelerator"></a>Inicio de sesión en el acelerador de la solución

Cuando el proceso de aprovisionamiento se completa, puede iniciar sesión en el acelerador de la solución Supervisión remota.

1. En la página **Soluciones aprovisionadas**, elija la solución Supervisión remota nueva:

    ![Elegir la nueva solución](./media/iot-accelerators-remote-monitoring-deploy/choosenew.png)

1. Puede consultar información sobre la solución Supervisión remota en el panel que aparece. Elija el **panel de la solución** para conectarse a la solución Supervisión remota.

    > [!NOTE]
    > Puede eliminar la solución Supervisión remota de este panel cuando termine de usarla.

    ![Panel de soluciones](./media/iot-accelerators-remote-monitoring-deploy/solutionpanel.png)

1. El panel de la solución Supervisión remota se muestra en el explorador.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Configuración del acelerador de la solución
> * Implementación del acelerador de solución
> * Inicio de sesión en el acelerador de la solución

Ahora que ha implementado la solución Supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](iot-accelerators-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->