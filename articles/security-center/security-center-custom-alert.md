---
title: Reglas de alerta personalizadas en Azure Security Center  | Microsoft Docs
description: Este documento le ayuda a crear reglas de alerta personalizadas en Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f335d8c4-0234-4304-b386-6f1ecda07833
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2018
ms.author: rkarlin
ms.openlocfilehash: 2573ad64830247d232bbd6d156fac7f50d65f410
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925099"
---
# <a name="custom-alert-rules-in-azure-security-center-preview"></a>Reglas de alerta personalizadas en Azure Security Center (versión preliminar)
Este documento le ayuda a crear reglas de alerta personalizadas en Azure Security Center.

> [!NOTE]
> Alertas personalizadas se retirará pronto, lea más en alternativos servicios siguientes.

## <a name="retirement-of-custom-alert-rules-in-azure-security-center"></a>Reglas de retirada de alerta personalizada en Azure Security Center

La experiencia de alertas personalizadas se retirará el 30 de junio de 2019, debido a la retirada de la infraestructura subyacente que se basa. En el período de tiempo hasta el desuso, los usuarios podrán editar reglas de alerta personalizadas existentes pero no podrá agregar otros nuevos.
Se aconseja a los usuarios para habilitar [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) con la incorporación de un solo clic para migrar sus alertas existentes y crear nuevos o volver a crear también sus alertas con Azure Monitor automáticamente alertas de registro.
                                     
Para mantener las alertas existentes y migrarlas a Azure Sentinel, inicie [iniciar Azure Sentinel](https://portal.azure.com/#create/Microsoft.ASI/preview). Como primer paso, seleccione el área de trabajo donde se almacenan las alertas personalizadas y, a continuación, seleccione el elemento de menú 'Analytics' migrar automáticamente las alertas.

> [!NOTE]
> Migración de alertas personalizadas para Azure Sentinel es una migración única de todas las alertas personalizadas en el área de trabajo seleccionada. Una vez completada la migración, las alertas personalizadas para esa área de trabajo seleccionada no es accesibles a través de Azure Security Center.
>
> Alertas personalizadas con [búsqueda](https://docs.microsoft.com/azure/azure-monitor/log-query/search-queries) o [unión](https://docs-analytics-eus.azurewebsites.net/queryLanguage/query_language_unionoperator.html) las consultas de las instrucciones no se admiten en Azure Sentinel y no se migrará. Modifique estas alertas antes de realizar la migración.

Para volver a crear las alertas mediante alertas de registro de Azure Monitor, consulte: [Crear, ver y administrar alertas de registro mediante Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) para obtener instrucciones sobre cómo crear alertas del registro. Para obtener información general de las alertas del registro en Azure Monitor, haga clic en [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

## <a name="what-are-custom-alert-rules-in-security-center"></a>¿Qué son las reglas de alerta personalizadas en Security Center?

Security Center cuenta con un conjunto de [alertas de seguridad](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) predefinidas, que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. En algunas situaciones, puede que quiera crear una alerta personalizados para satisfacer las necesidades específicas de su entorno.

Las reglas de alerta personalizadas de Security Center le permiten definir nuevas alertas de seguridad basadas en los datos ya recopilados del entorno. Puede crear consultas y el resultado de estas consultas se puede usar como criterio para la regla personalizada, de forma que, cuando se cumplan los criterios, se ejecute la regla. Puede usar eventos de seguridad de equipos, registros de soluciones de seguridad del asociado o datos ingeridos mediante las API para crear consultas personalizadas.

> [!NOTE]
> Las alertas personalizadas no son compatibles con la [característica de investigación](security-center-investigation.md) de Security Center.
>
>

## <a name="how-to-create-a-custom-alert-rule-in-security-center"></a>¿Cómo se crea una regla de alerta personalizada en Security Center?

Abra el panel **Security Center** y siga estos pasos para crear una regla de alerta personalizada:

1.  En el panel izquierdo, en **Detección**, haga clic en **Custom alert rules (Preview)** (Reglas de alerta personalizadas [versión preliminar]).
2.  En la página **Security Center – Custom alert rules (Preview)** (Security Center – Reglas de alerta personalizadas [versión preliminar]), haga clic en **Nueva regla de alertas personalizada**.

    ![Alerta personalizada](./media/security-center-custom-alert/security-center-custom-alert-fig1.png)

3.  Aparece la página Creación de reglas de alertas personalizadas con las siguientes opciones:

    ![Crear](./media/security-center-custom-alert/security-center-custom-alert-fig2.png)

4.  Escriba el nombre de esta regla personalizada en el campo **Nombre**.
5.  Escriba una breve descripción que refleje el propósito de esta regla en el campo **Descripción**.
6.  Seleccione el nivel de gravedad (alta, media, baja) según sus necesidades en el campo **Gravedad**.
7.  Seleccione la suscripción en la que esta regla es aplicable en el campo **Suscripción**.
8.  Seleccione el área de trabajo que quiere supervisar con esta regla en el campo **Área de trabajo** y, en el campo **Consulta de búsqueda**, la consulta que desee usar para obtener los resultados.

    > [!NOTE]
    > Necesita un permiso de escritura en el área de trabajo que seleccione para almacenar la alerta personalizada.
    >
    >

    El resultado de la consulta desencadena la alerta. Tenga en cuenta que cuando se escribe una consulta válida, aparece la marca de verificación verde en la esquina derecha de este campo:

    ![Consultar](./media/security-center-custom-alert/security-center-custom-alert-fig3.png)

10. Seleccione el intervalo de tiempo en el que se ejecutará la consulta anterior en el campo **Período**. Observe que el resultado de la búsqueda de la parte inferior de este campo cambia de acuerdo con el intervalo de tiempo que seleccione.

    ![Período](./media/security-center-custom-alert/security-center-custom-alert-fig4.png)

11. En el campo **Evaluación**, seleccione la frecuencia con la que se debe evaluar y ejecutar esta regla.
12. En el campo **Número de resultados**, seleccione el operador (mayor que o menor que).
13. En el campo **Umbral**, escriba un número que se usará como referencia para el operador seleccionado anteriormente.
14. Habilite la opción **Suprimir alertas** si quiere establecer un tiempo de espera antes de que Security Center envíe otra alerta para esta regla.
15. Haga clic en **Aceptar** para finalizar.

Cuando termine de crear la nueva regla de alerta, aparecerá en la lista de reglas de alerta personalizadas. Una vez que se cumplen las condiciones de esa regla, se desencadena una nueva alerta, que puede ver en el panel **Alertas de seguridad**.

![Alerta](./media/security-center-custom-alert/security-center-custom-alert-fig5.png)

Tenga en cuenta que los parámetros (consulta de búsqueda, umbral, etc.) que se establecieron durante la creación de la regla están disponibles en la alerta para esta regla personalizada.

## <a name="see-also"></a>Otras referencias
En este documento, ha aprendido a crear una regla de alerta personalizada en Azure Security Center. Para obtener más información sobre el Centro de seguridad de Azure, consulte los siguientes recursos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Aprenda a administrar las alertas y responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Guía de solución de problemas de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Obtenga información acerca de cómo solucionar problemas comunes en Security Center.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md). Preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.
