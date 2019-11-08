---
title: Configuración de alertas métricas en Azure Database for PostgreSQL - Hiperescala (Citus)
description: Este artículo describe cómo configurar y obtener acceso a las alertas métricas de Azure Database for PostgreSQL - Hiperescala (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d5f45df16ac294f50e99cc7e05ab6eba43c0ae85
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511265"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql---hyperscale-citus"></a>Use Azure Portal para configurar alertas métricas en Azure Database for PostgreSQL - Hiperescala (Citus)

En este artículo se explica cómo configurar alertas de Azure Database for PostgreSQL mediante Azure Portal. Puede recibir una alerta basada en las métricas de supervisión para los servicios de Azure.

Configuraremos una alerta que se active cuando el valor de una métrica específica cruce un umbral. La alerta se activa cuando se cumple la condición por primera vez y continúa después.

Puede configurar una alerta para realizar las siguientes acciones cuando se desencadene:
* Envía notificaciones por correo electrónico al administrador del servicio y a los coadministradores.
* Enviar un correo electrónico a direcciones de correo electrónico adicionales que especifique.
* Llamar a un webhook.

Puede obtener información sobre las reglas de alerta y configurarlas mediante:
* [Azure Portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [CLI de Azure](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Creación de una regla de alerta sobre una métrica desde Azure Portal
1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for PostgreSQL que quiera supervisar.

2. En la sección **Supervisión** de la barra lateral, seleccione **Alertas**, tal y como se muestra a continuación:

   ![Selección de Reglas de alerta](./media/howto-hyperscale-alert-on-metric/2-alert-rules.png)

3. Seleccione **Nueva regla de alertas** (icono +).

4. Se abre la página **Crear regla**, tal y como se muestra a continuación. Rellene la información necesaria:

   ![Formulario de adición de alerta de métrica](./media/howto-hyperscale-alert-on-metric/4-add-rule-form.png)

5. En la sección de **Condición**, seleccione **Agregar**.

6. Seleccione una métrica de la lista de señales sobre las que desea recibir alertas. En este ejemplo, seleccione "Porcentaje de almacenamiento".
   
   ![Selección de la métrica](./media/howto-hyperscale-alert-on-metric/6-configure-signal-logic.png)

7. Configure la lógica de alertas:

    * **Operador** (ej. "Mayor que")
    * **Valor umbral** (ej. 85 por ciento)
    * **Granularidad de agregación** cantidad de tiempo en la que se debe satisfacer la regla métrica antes de que la alerta se desencadene (ej. "En los últimos 30 minutos")
    * y **Frecuencia de evaluación** (ej. "1 minuto")
   
   Seleccione **Listo** cuando haya terminado.

   ![Selección de la métrica](./media/howto-hyperscale-alert-on-metric/7-set-threshold-time.png)

8. En la sección **Grupos de acciones**, seleccione **Crear nuevo** para crear un grupo en el que recibir las notificaciones sobre la alerta.

9. Rellene el formulario "Agregar grupo de acciones" con un nombre, un nombre corto, la suscripción y el grupo de recursos.

    ![Grupo de acciones](./media/howto-hyperscale-alert-on-metric/9-add-action-group.png)

10. Configure el tipo de acción **Correo electrónico/SMS/Push/Voz**.
    
    Elija "Rol de Azure Resource Manager de correo electrónico" para enviar notificaciones a los suscriptores, contribuidores y lectores.
   
    Cuando haya terminado, seleccione **Aceptar**.

    ![Grupo de acciones](./media/howto-hyperscale-alert-on-metric/10-action-group-type.png)

11. Especifique el nombre de la regla de alertas, la descripción y la gravedad.

    ![Grupo de acciones](./media/howto-hyperscale-alert-on-metric/11-name-description-severity.png) 

12. Seleccione **Crear regla de alerta** para crear la alerta.

    En cuestión de minutos, se activa la alerta y se desencadena tal como se describió anteriormente.

## <a name="manage-your-alerts"></a>Administración de alertas

Una vez que haya creado una alerta, puede seleccionarla y realizar las siguientes acciones:

* Ver un gráfico que muestre el umbral de las métricas y los valores reales del día anterior en relación con esta alerta.
* **Editar** o **eliminar** la regla de alerta.
* **Deshabilitar** o **habilitar** la alerta, si quiere detener temporalmente o reanudar la recepción de notificaciones.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre cómo [configurar webhooks en las alertas](../azure-monitor/platform/alerts-webhooks.md).
* Obtenga [información general sobre la colección de métricas](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) para garantizar que el servicio está disponible y que responder adecuadamente.
