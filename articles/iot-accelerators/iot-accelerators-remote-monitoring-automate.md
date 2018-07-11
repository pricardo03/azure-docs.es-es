---
title: Detección de dispositivos en una solución de supervisión remota basada en Azure | Microsoft Docs
description: En este tutorial, se muestra cómo usar reglas y acciones para detectar automáticamente problemas de los dispositivos basados en el umbral en la solución de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 1e3eaeec1d2eae3c36f285a3e4c536657504cbb8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098488"
---
# <a name="tutorial-detect-issues-with-devices-connected-to-your-monitoring-solution"></a>Tutorial: Detección de problemas con los dispositivos conectados a una solución de supervisión

En este tutorial, configurará el acelerador de soluciones de supervisión remota para detectar problemas de los dispositivos de IoT conectados. Para detectar problemas con los dispositivos, agregue reglas que generen alertas en el panel de la solución.

Para introducir reglas y alertas, el tutorial usa un dispositivo refrigerador simulado. El refrigerador es administrado por una organización denominada Contoso y está conectado al acelerador de soluciones de supervisión remota. Contoso ya dispone de una regla que genera una alerta crítica cuando la presión en un refrigerador supera los 298 PSI. Como operador de Contoso, desea identificar los dispositivos refrigeradores con sensores problemáticos buscando los picos de presión iniciales. Para identificar estos dispositivos, agregue una regla que genere una alerta de advertencia cuando la presión en el refrigerador esté por encima de 150 PSI.

También se le ha pedido que cree una alerta crítica para un refrigerador cuando, en los últimos cinco minutos, la humedad media del dispositivo sea mayor del 80 % y la temperatura del dispositivo sea superior a 75 grados Fahrenheit.

En este tutorial, va a:

>[!div class="checklist"]
> * Ver las reglas de la solución
> * Crear una regla
> * Crear una regla con varias condiciones
> * Editar una regla existente
> * Activar y desactivar reglas

## <a name="prerequisites"></a>Requisitos previos

Para seguir este tutorial, necesita una instancia implementada de la solución de supervisión remota en la suscripción de Azure.

Si aún no ha implementado el acelerador de soluciones de supervisión remota, debe completar la guía de inicio rápido [Implementación de una solución de supervisión remota basada en la nube](quickstart-remote-monitoring-deploy.md).

## <a name="view-the-existing-rules"></a>Visualización de las reglas existentes

La página **Rules** (Reglas) del acelerador de soluciones muestra una lista de todas las reglas actuales:

[![Página Rules](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-expanded.png#lightbox)

Para ver solo las reglas que se aplican a los dispositivos refrigerador, aplique un filtro:

[![Filtrado de la lista de reglas](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2-expanded.png#lightbox)

Puede ver más información sobre una regla y editarla si la selecciona en la lista:

[![Visualización de los detalles de la regla](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-expanded.png#lightbox)

Para deshabilitar o habilitar una o varias reglas, selecciónelas en la lista:

[![Selección de varias reglas](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2-expanded.png#lightbox)

## <a name="create-a-rule"></a>Crear una regla

Para crear una regla que genere una advertencia cuando la presión del dispositivo refrigerador se sitúe por encima de 150 PSI, elija **Nueva regla**. Utilice los valores siguientes para crear la regla:

| Configuración          | Valor                                 |
| ---------------- | ------------------------------------- |
| Nombre de la regla        | Advertencia del refrigerador                       |
| Descripción      | La presión del refrigerador ha superado 150 PSI |
| Grupo de dispositivos     | Grupo de dispositivos **Refrigeradores**             |
| Cálculo      | Instantánea                               |
| Campo de condición 1| presión                              |
| Operador de condición 1 | Mayor que                      |
| Valor de condición 1    | 150                               |
| Nivel de gravedad  | Advertencia                               |

[![Creación de regla de advertencia](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-expanded.png#lightbox)

Para guardar la nueva regla, haga clic en **Aplicar**.

Puede ver cuándo se desencadena la regla en la página **Rules** (Reglas) o en la página **Panel**:

[![Regla de advertencia desencadenada](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-expanded.png#lightbox)

## <a name="create-a-rule-with-multiple-conditions"></a>Creación de una regla con varias condiciones

Para crear una regla con varias condiciones que genere una alerta crítica cuando la humedad media del dispositivo refrigerador durante los últimos cinco minutos es superior al 80 % y la temperatura es superior a 75 grados Fahrenheit, seleccione **Nueva regla**. Utilice los valores siguientes para crear la regla:

| Configuración          | Valor                                 |
| ---------------- | ------------------------------------- |
| Nombre de la regla        | Humedad y temperatura críticas del refrigerador    |
| Descripción      | La temperatura y la humedad son críticas |
| Grupo de dispositivos     | Grupo de dispositivos **Refrigeradores**             |
| Cálculo      | Media                               |
| Período de tiempo      | 5                                     |
| Campo de condición 1| humedad                              |
| Operador de condición 1 | Mayor que                      |
| Valor de condición 1    | 80                                |
| Nivel de gravedad  | Crítico                              |

[![Creación de la parte uno de una regla de condición múltiple](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-expanded.png#lightbox)

Para agregar la segunda condición, haga clic en "+ Agregar condición". Utilice los siguientes valores para la nueva condición:

| Configuración          | Valor                                 |
| ---------------- | ------------------------------------- |
| Campo de condición 2| temperatura                           |
| Operador de condición 2 | Mayor que                      |
| Valor de condición 2    | 75                                |

[![Creación de la parte dos de una regla de condición múltiple](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-expanded.png#lightbox)

Para guardar la nueva regla, haga clic en **Aplicar**.

Puede ver cuándo se desencadena la regla en la página **Rules** (Reglas) o en la página **Panel**:

[![Regla de condición múltiple desencadenada](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-expanded.png#lightbox)

## <a name="edit-an-existing-rule"></a>Editar una regla existente

Para realizar un cambio en una regla existente, selecciónela en la lista de reglas y haga clic en **Editar**:

[![Editar regla](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-expanded.png#lightbox)

## <a name="disable-a-rule"></a>Deshabilitar una regla

Para desactivar temporalmente una regla, puede deshabilitarla en la lista de reglas. Elija la regla para deshabilitar y, a continuación, elija **Deshabilitar**. El **estado** de la regla en la lista cambia para indicar que está ahora deshabilitada. Puede volver a habilitar una regla que anteriormente ha deshabilitado siguiendo el mismo procedimiento.

[![Deshabilitar regla](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-expanded.png#lightbox)

Puede habilitar y deshabilitar varias reglas al mismo tiempo si selecciona varias en la lista.

<!-- ## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="clean-up-resources"></a>Limpieza de recursos

Si va a pasar al tutorial siguiente, deje el acelerador de soluciones de supervisión remota implementado. Para reducir los costos de la ejecución del acelerador de soluciones mientras no lo esté usando, puede detener los dispositivos simulados en el panel de configuración:

[![Pausa de la telemetría](./media/iot-accelerators-remote-monitoring-automate/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-automate/togglesimulation-expanded.png#lightbox)

Puede reiniciar los dispositivos simulados cuando esté preparado para comenzar el tutorial siguiente.

Si ya no necesita el acelerador de soluciones, elimínelo de la página [Soluciones aprovisionadas](https://www.azureiotsolutions.com/Accelerators#dashboard):

![Eliminación de la solución](media/iot-accelerators-remote-monitoring-automate/deletesolution.png)

## <a name="next-steps"></a>Pasos siguientes

Este tutorial ha mostrado cómo usar la página **Reglas** en el acelerador de soluciones de supervisión remota para crear y administrar las reglas que desencadenan alertas en la solución. Para obtener información sobre cómo usar el acelerador de soluciones para administrar y configurar los dispositivos conectados, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Configuración y administración de los dispositivos conectados a la solución de supervisión](iot-accelerators-remote-monitoring-manage.md)