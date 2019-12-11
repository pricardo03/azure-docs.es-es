---
title: Retirada de Azure Sentinel| Microsoft Docs
description: Eliminación de la instancia de Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2019
ms.author: rkarlin
ms.openlocfilehash: 4c0c415235fd290bc47ac402a6b81a1afa7af903
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74777441"
---
# <a name="remove-azure-sentinel-from-your-workspace"></a>Eliminación de Azure Sentinel del área de trabajo

Si ya no desea usar Azure Sentinel, en este artículo se explica cómo eliminarlo del área de trabajo.

## <a name="how-to-delete-azure-sentinel"></a>Eliminación de Azure Sentinel

Cuando instala Azure Sentinel, se instala **SecurityInsights** en segundo plano en el área de trabajo seleccionada. Por tanto, lo primero que tiene que hacer es quitar la solución **SecurityInsights**.

1.  Vaya a **Azure Sentinel**, luego a **Configuración**, después a **Configuración del área de trabajo** y, finalmente, a **Soluciones**.

2.  Seleccione `SecurityInsights` y haga clic en ella.

    ![Buscar la solución SecurityInsights](media/offboard/find-solution.png)

3.  En la parte superior de la página, seleccione **Eliminar**.

    > [!IMPORTANT]
    > Si quita el área de trabajo, puede que otras soluciones y orígenes de datos que la usan, como Azure Monitor, se vean afectadas. Para comprobar qué soluciones usan este área de trabajo, consulte [Lista de soluciones de supervisión instaladas](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions). Para comprobar qué datos de las soluciones se ingieren en el área de trabajo, consulte [Descripción del volumen de datos ingeridos](../azure-monitor/platform/manage-cost-storage.md#understanding-ingested-data-volume).

    ![Eliminar la solución SecurityInsights](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>¿Qué sucede en segundo plano?

Al eliminar la solución, Azure Sentinel tarda hasta 48 horas en completar la primera fase del proceso de eliminación.

Una vez identificada la desconexión, comienza el proceso de retirada.

**Se elimina la configuración de estos conectores:**
-   Office 365

-   AWS

-   Alertas de seguridad de los servicios de Microsoft (Azure ATP, Microsoft Cloud App Security incluido Cloud Discovery Shadow IT Reporting, Azure AD Identity Protection, Microsoft Defender ATP, Azure Security Center)

-   Información sobre amenazas

-   Registros de seguridad habituales (incluidos los registros basados en CEF, Barracuda y Syslog) (si tiene Azure Security Center, se seguirán recopilando estos registros).

-   Eventos de seguridad de Windows (si tiene Azure Security Center, se seguirán recopilando estos registros).

Durante las primeras 48 horas, los datos y las reglas de alertas (incluida la configuración de la automatización en tiempo real) dejarán de ser accesibles o consultables en Azure Sentinel.

**Después de 30 días, se eliminarán estos recursos:**

-   Incidentes (incluidos los metadatos de investigación)

-   Reglas de alertas

-   Marcadores

Los cuadernos de estrategias, libros guardados, consultas de búsqueda guardadas y cuadernos no se eliminan. **Algunos pueden generar errores debido a los datos que se han quitado. Deberá eliminarlos manualmente.**

Después de quitar el servicio, hay un período de gracia de 30 días durante el cual puede volver a habilitar la solución y se restaurarán los datos y las reglas de alertas, pero los conectores configurados que se desconectaron deberán volver a conectarse.

> [!NOTE]
> Si quita la solución, la suscripción se seguirá registrando con el proveedor de recursos de Azure Sentinel. **Puede eliminarlos manualmente.**




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a eliminar el servicio Azure Sentinel. Si cambia de opinión y desea volver a instalarlo:
- Introducción a la [incorporación de Azure Sentinel](quickstart-onboard.md).

