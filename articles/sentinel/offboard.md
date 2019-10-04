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
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: f636fc9bab5ea77d11ac7944e7aa18cb0e402b8c
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71241204"
---
# <a name="remove-azure-sentinel-from-your-tenant"></a>Quitar Azure Sentinel del inquilino

Si ya no desea usar Azure Sentinel, en este artículo se explica cómo quitarlo del inquilino.

## <a name="how-to-delete-azure-sentinel"></a>Eliminación de Azure Sentinel

Cuando instala Azure Sentinel, se instala **SecurityInsights** en segundo plano en el área de trabajo seleccionada. Por tanto, lo primero que tiene que hacer es quitar la solución **SecurityInsights**.

1.  Vaya a **Azure Sentinel**, luego a **Configuración**, después a **Configuración del área de trabajo** y, finalmente, a **Soluciones**.

2.  Seleccione `SecurityInsights` y haga clic en ella.

    ![Buscar la solución SecurityInsights](media/offboard/find-solution.png)

3.  En la parte superior de la página, seleccione **Eliminar**.

    > [!IMPORTANT]
    > Si elimina el área de trabajo, se quitará esta y Azure Sentinel del inquilino de Azure Monitor.

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

