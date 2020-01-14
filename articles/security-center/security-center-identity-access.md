---
title: Supervisión de identidad y acceso en Azure Security Center | Microsoft Docs
description: Aprenda a usar la funcionalidad de identidad y acceso de Azure Security Center para supervisar los problemas relacionados con la actividad de acceso e identidad de los usuarios.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 6b262baddd10c9d0dff4b196b733972b97d99872
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552991"
---
# <a name="monitor-identity-and-access-preview"></a>Supervisión de la identidad y el acceso (versión preliminar)
Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios para reforzar y proteger sus recursos.

En este artículo se explica la página **Identidad y acceso** de la sección de seguridad de los recursos de Azure Security Center.

Para obtener una lista completa de las recomendaciones que puede ver en esta página, consulte [Recomendaciones de identidad y acceso](recommendations-reference.md#recs-identity).

> [!NOTE]
> La supervisión de la identidad y el acceso se encuentra en versión preliminar y solo está disponible en el nivel estándar de Security Center. Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
>

La identidad debe ser el plano de control de la empresa y protegerla debe ser la prioridad máxima. El perímetro de seguridad ha evolucionado de un perímetro de red a uno de identidad. La seguridad se centra menos en proteger la red y más en proteger los datos, así como en administrar la seguridad de las aplicaciones y los usuarios. Hoy en día, donde cada vez más datos y aplicaciones se mueven a la nube, la identidad se convierte en el nuevo perímetro.

Mediante la supervisión de las actividades de identidad, puede realizar acciones proactivas antes de que se produzca un incidente o acciones reactivas para detener un intento de ataque. El panel Identidad y acceso proporciona recomendaciones como las siguientes:

- Habilitar MFA para las cuentas con privilegios en la suscripción
- Quitar las cuentas externas con permisos de escritura de la suscripción
- Quitar las cuentas externas con privilegios de la suscripción

> [!NOTE]
> Si su suscripción tiene más de 600 cuentas, Security Center no puede ejecutar las recomendaciones de identidades en su suscripción. Las recomendaciones que no se ejecutan se muestran bajo "evaluaciones no disponibles" a continuación.
Security Center no puede ejecutar las recomendaciones de identidad en los agentes de administración del asociado de un proveedor de soluciones en la nube (CSP).
>

## <a name="monitor-identity-and-access"></a>Supervisión de la identidad y el acceso

Para abrir la lista de problemas de identidad y acceso identificados, seleccione **Identidad y acceso** en la barra lateral de Security Center (en **Recursos**) o en la página de información general. 

En **Identidad y acceso**, hay dos pestañas:

- **Información general**: recomendaciones identificadas por Security Center.
- **Suscripciones**: lista de las suscripciones y el estado de seguridad actual de cada una.

[![Identidad y acceso](./media/security-center-identity-access/identity-dashboard.png)](./media/security-center-identity-access/identity-dashboard.png#lightbox)

### <a name="overview-section"></a>Sección Información general
En **Información general**, hay una lista de recomendaciones. La primera columna muestra la recomendación. La segunda columna muestra el número total de suscripciones a las que afecta dicha recomendación. La tercera columna muestra la gravedad del problema.

1. Seleccione una recomendación. La ventana de las recomendaciones se abre y muestra:

   - La descripción de la recomendación
   - Una lista de suscripciones correctas e incorrectas
   - Una lista de los recursos sin examinar debido a una evaluación errónea o a que el recurso está en una suscripción que se ejecuta en el nivel gratuito y no se evalúa

    [![Ventana de recomendaciones](./media/security-center-identity-access/select-subscription.png)](./media/security-center-identity-access/select-subscription.png#lightbox)

1. Seleccione una suscripción en la lista para obtener detalles adicionales.

### <a name="subscriptions-section"></a>Sección Suscripciones
En **Suscripciones**, hay una lista de suscripciones. La primera columna muestra las suscripciones. La segunda columna muestra el número total de recomendaciones para cada suscripción. La tercera columna muestra la gravedad de los problemas.

[![Pestaña Suscripciones](./media/security-center-identity-access/subscriptions.png)](./media/security-center-identity-access/subscriptions.png#lightbox)

1. Seleccione una suscripción. Se abre una vista de resumen con tres pestañas:

   - **Recomendaciones**: en función de las evaluaciones de Security Center que mostraron errores.
   - **Evaluaciones aprobadas**: lista de las evaluaciones realizadas por Security Center que se aprobaron.
   - **Evaluaciones no disponibles**: lista de las evaluaciones que no se pudo ejecutar debido a un error o porque la suscripción tiene más de 600 cuentas.

   En **Recomendaciones** hay una lista de las recomendaciones para la suscripción seleccionada y la gravedad de cada una.

   [![Recomendaciones para la suscripción seleccionada](./media/security-center-identity-access/recommendations.png)](./media/security-center-identity-access/recommendations.png#lightbox)

1. Seleccione una recomendación para obtener su descripción, una lista de suscripciones correctas e incorrectas, y una lista de los recursos sin examinar.

   [![Descripción de la recomendación](./media/security-center-identity-access/designate.png)](./media/security-center-identity-access/designate.png#lightbox)

   En **Evaluaciones aprobadas** hay una lista de las evaluaciones que se consideraron superadas.  La gravedad de estas evaluaciones siempre aparece en verde.

   [![Evaluaciones aprobadas](./media/security-center-identity-access/passed-assessments.png)](./media/security-center-identity-access/passed-assessments.png#lightbox)

1. Seleccione una evaluación aprobada en la lista para obtener su descripción y una lista de suscripciones correctas. Hay una pestaña para las suscripciones incorrectas que enumera todas las que tenían errores.

   [![Evaluaciones aprobadas](./media/security-center-identity-access/remove.png)](./media/security-center-identity-access/remove.png#lightbox)

> [!NOTE]
> Si ha creado una directiva de acceso condicional que necesita MFA pero tiene exclusiones establecidas, la evaluación de la recomendación de MFA de Security Center considera la directiva no compatible, ya que permite que algunos usuarios inicien sesión en Azure sin MFA.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

- [Protección de las aplicaciones y las máquinas en Azure Security Center](security-center-virtual-machine-protection.md)
- [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
- [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)