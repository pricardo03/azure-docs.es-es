---
title: 'Crear trabajos programados con Azure Scheduler: Azure Portal | Microsoft Docs'
description: Obtenga información sobre cómo crear, programar y ejecutar su primer trabajo automatizado con Azure Scheduler en Azure Portal
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.topic: conceptual
ms.date: 09/17/2018
ms.openlocfilehash: 3b2cfc932c6322df8237ec7cdf820fc4242bfa72
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57887228"
---
# <a name="create-and-schedule-your-first-job-with-azure-scheduler---azure-portal"></a>Crear y programar su primer trabajo con Azure Scheduler : Azure Portal

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza Azure Scheduler, que se va a retirar. Para programar trabajos, [pruebe Azure Logic Apps en su lugar](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Este tutorial muestra la facilidad con la que puede crear y programar un trabajo y después supervisarlo y administrarlo. 

Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

## <a name="create-job"></a>Creación del trabajo

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).  

1. En el menú principal de Azure, seleccione **Crear un recurso**. En el cuadro de búsqueda, escriba "scheduler". En la lista de resultados, seleccione **Scheduler** y después **Crear**.

   ![Crear recurso de Scheduler](./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png)

   Ahora cree un trabajo que envíe una solicitud GET a esta dirección URL: `https://www.microsoft.com/` 

1. En **Trabajo de Scheduler**, especifique esta información:

   | Propiedad | Valor de ejemplo | DESCRIPCIÓN |
   |----------|---------------|-------------| 
   | **Nombre** | getMicrosoft | El nombre para el trabajo | 
   | **Colección de trabajos** | <*job-collection-name*> | Cree una colección de trabajos o seleccione una existente. | 
   | **Suscripción** | <*Azure-subscription-name*> | El nombre de la suscripción a Azure | 
   |||| 

1. Seleccione **Configuración de acción: Configurar**, proporcione esta información y elija **Aceptar** cuando haya terminado:

   | Propiedad | Valor de ejemplo | DESCRIPCIÓN |
   |----------|---------------|-------------| 
   | **Acción** | **Http** | El tipo de acción para ejecutar | 
   | **Método** | **Get** | El método al que llamar | 
   | **URL** | **https://www.microsoft.com** | La dirección URL de destino | 
   |||| 
   
   ![Definir el trabajo](./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png)

1. Seleccione **Programación: Configurar**, defina la programación y seleccione **Aceptar** cuando haya terminado:

   Aunque puede crear un trabajo único, este ejemplo configura una programación periódica.

   | Propiedad | Valor de ejemplo | DESCRIPCIÓN |
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

### <a name="properties"></a>Properties (Propiedades)

Para ver las propiedades de solo lectura que describen los metadatos de administración para el trabajo, seleccione **Propiedades**.

![Ver propiedades del trabajo](./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png)

<a name="action-settings"></a>

### <a name="action-settings"></a>Configuración de la acción

Para cambiar la configuración avanzada del trabajo, seleccione **Configuración de acción**. 

![Revisar la configuración de la acción](./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png)

| Tipo de acción | DESCRIPCIÓN | 
|-------------|-------------| 
| Todos los tipos | Puede cambiar la **Directiva de reintentos** y la configuración de **Acción del error**. | 
| HTTP y HTTPS | Puede cambiar **Método** a cualquier método permitido. También puede agregar, eliminar o cambiar los encabezados y la información de autenticación básica. | 
| Cola de almacenamiento| Puede cambiar la cuenta de almacenamiento, el nombre de la cola, el token de SAS y el cuerpo. | 
| Azure Service Bus | Puede cambiar el espacio de nombres, las rutas de acceso de colas o temas, la configuración de autenticación, el tipo de transporte, las propiedades del mensaje y el cuerpo del mensaje. | 
||| 

<a name="schedule"></a>

### <a name="schedule"></a>Schedule

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
* [Crear programaciones complejas y periodicidad avanzada](scheduler-advanced-complexity.md)
* Obtenga información sobre [alta disponibilidad y confiabilidad de Scheduler](scheduler-high-availability-reliability.md)
* Obtenga información sobre [límites, cuotas, valores predeterminados y códigos de error](scheduler-limits-defaults-errors.md)
