---
title: 'Creación de trabajos programados: Azure Portal'
description: Cree, programe y ejecute su primer trabajo automatizado en Azure Portal mediante Azure Scheduler.
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: estfan
ms.reviewer: klam, estfan, logicappspm
ms.topic: conceptual
ms.date: 02/29/2020
ms.openlocfilehash: a9f7169f4b54dfc08612b1d53bfde48154ee2d1d
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2020
ms.locfileid: "77524808"
---
# <a name="create-and-schedule-your-first-job-by-using-azure-scheduler---azure-portal"></a>Creación y programación de su primer trabajo con Azure Scheduler: Azure Portal

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes.

Este tutorial muestra la facilidad con la que puede crear y programar un trabajo y después supervisarlo y administrarlo.

Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

## <a name="create-job"></a>Creación del trabajo

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. En el cuadro de búsqueda de Azure, escriba `scheduler` como filtro. En la lista de resultados, seleccione **Colecciones de trabajos del Programador** y seleccione **Crear**.

   ![Crear recurso de Scheduler](./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png)

   Ahora cree un trabajo que envíe una solicitud GET a esta dirección URL: `https://www.microsoft.com/` 

1. En **Trabajo de Scheduler**, especifique esta información:

   | Propiedad | Valor de ejemplo | Descripción |
   |----------|---------------|-------------| 
   | **Nombre** | getMicrosoft | El nombre para el trabajo | 
   | **Colección de trabajos** | <*job-collection-name*> | Cree una colección de trabajos o seleccione una existente. | 
   | **Suscripción** | <*Azure-subscription-name*> | El nombre de la suscripción a Azure | 
   |||| 

1. Seleccione **Configuración de acción: Configurar**, proporcione esta información y elija **Aceptar** cuando haya terminado:

   | Propiedad | Valor de ejemplo | Descripción |
   |----------|---------------|-------------| 
   | **Acción** | **Http** | El tipo de acción para ejecutar | 
   | **Método** | **Get** | El método al que llamar | 
   | **URL** | **https://www.microsoft.com** | La dirección URL de destino | 
   |||| 
   
   ![Definir el trabajo](./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png)

1. Seleccione **Programación: Configurar**, defina la programación y seleccione **Aceptar** cuando haya terminado:

   Aunque puede crear un trabajo único, este ejemplo configura una programación periódica.

   | Propiedad | Valor de ejemplo | Descripción |
   |----------|---------------|-------------| 
   | **Periodicidad** | **Periódica** | Un trabajo puntual o periódico | 
   | **Iniciar el** | <*fecha de hoy*> | La fecha de inicio del trabajo | 
   | **Repetir cada** | **1 hora** | El intervalo de periodicidad y la frecuencia | 
   | **Finalizar** | **Finalizar el** la fecha dos días después de hoy | La fecha de finalización del trabajo | 
   | **Diferencia horaria con UTC** | **UTC +08:00** | La diferencia de tiempo entre la Hora universal coordinada (UTC) y la hora observada de la ubicación | 
   |||| 

   ![Definir programación](./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png)

1. Cuando esté listo, seleccione **Crear**.

   Después de crear el trabajo, Azure lo implementa y aparece en el panel de Azure. 

1. Cuando Azure muestre una notificación de que la implementación se realizó correctamente, elija **Anclar al panel**. En caso contrario, elija el icono **Notificaciones** (campana) en la barra de herramientas de Azure y después **Anclar al panel**.

## <a name="monitor-and-manage-jobs"></a>Supervisar y administrar trabajos

Para revisar, supervisar y administrar su trabajo elíjalo en el panel de Azure. En **Configuración**, éstas son las áreas en las que puede revisar y administrar su trabajo:

![Configuración del trabajo](./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png)

Para obtener más información acerca de estas áreas, seleccione un área:

* [**Propiedades**](#properties)
* [**Configuración de la acción**](#action-settings)
* [**Programación**](#schedule)
* [**Historial**](#history)
* [**Usuarios**](#users)

<a name="properties"></a>

### <a name="properties"></a>Propiedades

Para ver las propiedades de solo lectura que describen los metadatos de administración para el trabajo, seleccione **Propiedades**.

![Ver propiedades del trabajo](./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png)

<a name="action-settings"></a>

### <a name="action-settings"></a>Configuración de la acción

Para cambiar la configuración avanzada del trabajo, seleccione **Configuración de acción**. 

![Revisar la configuración de la acción](./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png)

| Tipo de acción | Descripción | 
|-------------|-------------| 
| Todos los tipos | Puede cambiar la **Directiva de reintentos** y la configuración de **Acción del error**. | 
| HTTP y HTTPS | Puede cambiar **Método** a cualquier método permitido. También puede agregar, eliminar o cambiar los encabezados y la información de autenticación básica. | 
| Cola de almacenamiento| Puede cambiar la cuenta de almacenamiento, el nombre de la cola, el token de SAS y el cuerpo. | 
| Azure Service Bus | Puede cambiar el espacio de nombres, las rutas de acceso de colas o temas, la configuración de autenticación, el tipo de transporte, las propiedades del mensaje y el cuerpo del mensaje. | 
||| 

<a name="schedule"></a>

### <a name="schedule"></a>Programación

Si configura una programación a través del Asistente de trabajo puede cambiarla, modificando la fecha de inicio y tiempo, la programación de periodicidad, la fecha de finalización y el tiempo para los trabajos periódicos.
También puede crear más [programaciones complejas y periodicidad avanzada](scheduler-advanced-complexity.md).

Para cambiar la vista o cambiar la programación del trabajo, seleccione **Programación**:

![Ver programación de trabajo](./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png)

<a name="history"></a>

### <a name="history"></a>Historial

Para ver las métricas sobre cada ejecución de un trabajo seleccionado, seleccione **Historial**. Estas métricas proporcionan valores en tiempo real sobre el estado del trabajo, como el estado, el número de reintentos, el número de repeticiones, la hora de inicio y la hora de finalización.

![Ver las métricas y el historial de trabajos](./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png)

Para ver los detalles del historial de cada ejecución, como la respuesta completa para cada ejecución, seleccione cada ejecución en **Historial**. 

![Ver detalles del historial de trabajos](./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png)

<a name="users"></a>

### <a name="users"></a>Usuarios

Puede administrar el acceso a Azure Scheduler para cada usuario en un nivel específico mediante Control de acceso basado en rol (RBAC). Para obtener información sobre cómo configurar el acceso basándose en roles, consulte [Administración del acceso mediante RBAC](../role-based-access-control/role-assignments-portal.md)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [conceptos, terminología y jerarquía de entidades](scheduler-concepts-terms.md)
* [Creación de programaciones complejas y periodicidad avanzada](scheduler-advanced-complexity.md)
* Obtenga información sobre [alta disponibilidad y confiabilidad de Scheduler](scheduler-high-availability-reliability.md)
* Obtenga información sobre [límites, cuotas, valores predeterminados y códigos de error](scheduler-limits-defaults-errors.md)
