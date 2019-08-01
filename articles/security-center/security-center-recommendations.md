---
title: Recomendaciones de seguridad de Azure Security Center | Microsoft Docs
description: Este documento explica cómo las recomendaciones del Centro de seguridad de Azure ayudan a proteger los recursos de Azure y a cumplir con las directivas de seguridad.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2019
ms.author: v-mohabe
ms.openlocfilehash: fe1d4bf27f3c4bb1f70c1c1fa9767c27f8767998
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064210"
---
# <a name="security-recommendations-in-azure-security-center"></a>Recomendaciones de seguridad en el Centro de seguridad de Azure 
En este tema se explica cómo ver y entender las recomendaciones de Azure Security Center para ayudar a proteger los recursos de Azure.

> [!NOTE]
> En este documento se presenta el servicio mediante una implementación de ejemplo.  Este documento no es una guía paso a paso.
>

## <a name="what-are-security-recommendations"></a>¿Cuáles son las recomendaciones de seguridad?
El Centro de seguridad analiza periódicamente el estado de seguridad de los recursos de Azure. Cuando el Centro de seguridad identifica vulnerabilidades de seguridad potenciales, crea recomendaciones. Las recomendaciones le guían en el proceso de configuración de los controles necesarios.

## <a name="implementing-security-recommendations"></a>Implementación de recomendaciones de seguridad
### <a name="set-recommendations"></a>Obtención de recomendaciones
En [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md), aprenderá lo siguiente:

* Configurar directivas de seguridad.
* Activar la recopilación de datos.
* Elegir las recomendaciones que verá como parte de la directiva de seguridad.

Las recomendaciones de directiva actuales se centran en las actualizaciones del sistema, las reglas de línea de base, los programas antimalware, los [grupos de seguridad de red](../virtual-network/security-overview.md) en subredes y las interfaces de red, la auditoría de bases de datos de SQL, el cifrado de datos transparente de bases de datos de SQL y los firewalls de aplicaciones web.  [Establecimiento de directivas de seguridad](tutorial-security-policy.md) proporciona una descripción de cada opción de recomendación.

### <a name="monitor-recommendations"></a>Supervisión de recomendaciones
Después de establecer una directiva de seguridad, el Centro de seguridad analiza el estado de seguridad de los recursos, con el fin de identificar vulnerabilidades potenciales. El icono **Recomendaciones** de **Información general** permite conocer el número total de recomendaciones que identifica Security Center.

![Introducción a Security Center](./media/security-center-recommendations/asc-overview.png)

1. Seleccione el **icono de Recomendaciones** en **Información general**. Se abre la lista **Recomendaciones**.
    
      ![Ver recomendaciones](./media/security-center-recommendations/view-recommendations.png)

    Puede filtrar las recomendaciones. Para ello, seleccione **Filtro** en la hoja **Recomendaciones**. Se abrirá la hoja **Filtro** , donde podrá seleccionar los valores de gravedad y de estado que quiera ver.

   * **RECOMENDACIONES**: la recomendación.
   * **IMPACTO DE LA PUNTUACIÓN SEGURA**: puntuación que genera Security Center mediante sus recomendaciones de seguridad y la aplicación de algoritmos avanzados para determinar la importancia de cada recomendación. Para más información, consulte [Cálculo de Puntuación segura](security-center-secure-score.md#secure-score-calculation).
   * **RECURSO**: enumera los recursos a los que se aplica la recomendación.
   * **BARRAS DE ESTADO**:  describe la gravedad de una recomendación concreta:
       * **Alta (rojo)** : existe una vulnerabilidad en un recurso importante (como una aplicación, una máquina virtual o un grupo de seguridad de red) y requiere atención.
       * **Media (naranja)** : existe una vulnerabilidad y se requieren pasos adicionales o no críticos para eliminarla o completar un proceso.
       * **Baja (azul)** : existe una vulnerabilidad que se debe abordar, pero no requiere una atención inmediata. (De manera predeterminada no se muestran las recomendaciones bajas, pero si desea verlas, puede filtrar por ellas). 
       * **Correcto (verde)** :
       * **No disponible (gris)** :

1. Para ver los detalles de cada recomendación, haga clic en la recomendación.

    ![Detalles de recomendación](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Consulte los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) de los recursos de Azure.
  
 ### <a name="apply-recommendations"></a>Aplicación de recomendaciones
> Después de revisar todas las recomendaciones, decida cuál se aplica primero. Se recomienda usar el impacto de la puntuación segura para evaluar qué recomendaciones se deben aplicar primero.

1. En la lista, haga clic en la recomendación.
1. Siga las instrucciones de la sección *Pasos para la corrección*.

## <a name="next-steps"></a>Pasos siguientes
En este documento, se han presentando las recomendaciones de seguridad del Centro de seguridad. Para más información sobre Security Center, consulte los temas:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md) : aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md) : obtenga más información sobre cómo supervisar el estado de sus recursos en Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md) : aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/) : encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.
