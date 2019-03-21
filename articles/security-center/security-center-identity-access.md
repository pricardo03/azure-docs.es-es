---
title: Supervisión de identidad y acceso en Azure Security Center | Microsoft Docs
description: Aprenda a usar la funcionalidad de identidad y acceso de Azure Security Center para supervisar los problemas relacionados con la actividad de acceso e identidad de los usuarios.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 6f8fadc42d1c725002f2552a3fc1fc98e8564437
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58098787"
---
# <a name="monitor-identity-and-access-in-azure-security-center-preview"></a>Supervisión de la identidad y el acceso en Azure Security Center (Versión preliminar)
Este artículo le ayuda a usar Azure Security Center para supervisar la identidad y la actividad de acceso del usuario.

> [!NOTE]
> La supervisión de la identidad y el acceso se encuentra en versión preliminar y solo está disponible en el nivel estándar de Security Center. Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
>

La identidad debe ser el plano de control de la empresa y protegerla debe ser la prioridad máxima. El perímetro de seguridad ha evolucionado de un perímetro de red a uno de identidad. La seguridad se centra menos en proteger la red y más en proteger los datos, así como en administrar la seguridad de las aplicaciones y los usuarios. Hoy en día, donde cada vez más datos y aplicaciones se mueven a la nube, la identidad se convierte en el nuevo perímetro.

Mediante la supervisión de las actividades de identidad, puede realizar acciones proactivas antes de que se produzca un incidente o acciones reactivas para detener un intento de ataque. El panel Identidad y acceso proporciona recomendaciones como las siguientes:

- Habilitar MFA para las cuentas con privilegios en la suscripción
- Quitar las cuentas externas con permisos de escritura de la suscripción
- Quitar las cuentas externas con privilegios de la suscripción

> [!NOTE]
> Si su suscripción tiene más de 600 cuentas, Security Center no puede ejecutar las recomendaciones de identidades en su suscripción. Las recomendaciones que no se ejecutan se muestran bajo "evaluaciones no disponibles", lo que se describe a continuación.
Security Center no puede ejecutar las recomendaciones de identidad en los agentes de administración del asociado de un proveedor de soluciones en la nube (CSP).
>
>

Consulte [Recomendaciones](security-center-identity-access.md#recommendations) para obtener una lista de las recomendaciones de acceso e identidad proporcionadas por Security Center.

## <a name="monitoring-security-health"></a>Supervisión del estado de seguridad
Puede supervisar el estado de seguridad de los recursos en el panel **Security Center – Información general**. La sección **Recursos** es un indicador de mantenimiento que muestra la gravedad para cada tipo de recurso.

Para ver una lista de todos los problemas, puede seleccionar **Recomendaciones**. En **Recursos**, puede ver una lista de los problemas específicos del proceso y las aplicaciones, la seguridad de los datos, las redes o la identidad y el acceso. Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).

Para una lista completa de las recomendaciones de identidades y acceso, consulte [Recomendaciones](security-center-identity-access.md#recommendations).

Para continuar, seleccione **Identidad y acceso** en **Recursos** o en el menú principal de Security Center.

![Panel de Security Center][1]

## <a name="monitor-identity-and-access"></a>Supervisión de la identidad y el acceso
En **Identidad y acceso**, hay dos pestañas:

- **Información general**: recomendaciones identificadas por Security Center.
- **Suscripciones**: lista de las suscripciones y el estado de seguridad actual de cada una.

![Identidad y acceso][2]

### <a name="overview-section"></a>Sección Información general
En **Información general**, hay una lista de recomendaciones. La primera columna muestra la recomendación. La segunda columna muestra el número total de suscripciones a las que afecta dicha recomendación. La tercera columna muestra la gravedad del problema.

1. Seleccione una recomendación. La ventana de la recomendación se abre y muestra:

   - La descripción de la recomendación
   - Una lista de suscripciones correctas e incorrectas
   - Una lista de los recursos sin examinar debido a una evaluación errónea o a que el recurso está en una suscripción que se ejecuta en el nivel gratuito y no se evalúa

   ![Ventana de recomendaciones][3]

1. Seleccione una suscripción en la lista para obtener detalles adicionales.

### <a name="subscriptions-section"></a>Sección Suscripciones
En **Suscripciones**, hay una lista de suscripciones. La primera columna muestra las suscripciones. La segunda columna muestra el número total de recomendaciones para cada suscripción. La tercera columna muestra la gravedad de los problemas.

![Pestaña de la suscripción][4]

1. Seleccione una suscripción. Se abre una vista de resumen con tres pestañas:

   - **Recomendaciones**: en función de las evaluaciones de Security Center que mostraron errores.
   - **Evaluaciones aprobadas**: lista de las evaluaciones realizadas por Security Center que se aprobaron.
   - **Evaluaciones no disponibles**: lista de las evaluaciones que no se pudo ejecutar debido a un error o porque la suscripción tiene más de 600 cuentas.

   En **Recomendaciones** hay una lista de las recomendaciones para la suscripción seleccionada y la gravedad de cada una.

   ![Recomendaciones para la suscripción seleccionada][5]

1. Seleccione una recomendación para obtener su descripción, una lista de suscripciones correctas e incorrectas, y una lista de los recursos sin examinar.

   ![La descripción de la recomendación][6]

   En **Evaluaciones aprobadas** hay una lista de las evaluaciones que se consideraron superadas.  La gravedad de estas evaluaciones siempre aparece en verde.

   ![Evaluaciones aprobadas][7]

1. Seleccione una evaluación aprobada en la lista para obtener su descripción y una lista de suscripciones correctas. Hay una pestaña para las suscripciones incorrectas que enumera todas las que tenían errores.

   ![Evaluaciones aprobadas][8]

## <a name="recommendations"></a>Recomendaciones
Use la tabla siguiente como referencia para ayudarle a conocer las recomendaciones de identidad y acceso disponibles, y lo que hará cada una si la aplica.

|Tipo de recurso|Puntuación segura|Recomendación|DESCRIPCIÓN|
|----|----|----|----|
|Subscription|50|Habilitar MFA para las cuentas de la aplicación de administración de Azure con permisos de propietario en la suscripción|Habilite Multi-Factor Authentication (MFA) para todas las cuentas de suscripción con privilegios de administrador para evitar una vulneración de las cuentas o recursos.|
|Subscription|50|Habilitar Security Center en las suscripciones |Habilite Security Center en todas las suscripciones para la detección de amenazas avanzada, JIT, listas de aplicaciones permitidas y recomendaciones avanzadas. |
|Subscription|50|Habilitar el nivel estándar de Security Center en las suscripciones |Habilite el centro de seguridad de todas las suscripciones para la detección de amenazas avanzada, JIT, listas de aplicaciones permitidas y recomendaciones avanzadas.|
|Subscription|40|Habilitar MFA para las cuentas de la aplicación de administración de Azure con permisos de escritura en la suscripción|Habilite Multi-Factor Authentication (MFA) para todas las cuentas de suscripción con privilegios de escritura para evitar una vulneración de las cuentas o recursos.|
|Subscription|30|Quitar las cuentas externas con permisos de propietario de la suscripción|Elimine las cuentas externas con permisos de propietario de la suscripción a fin de evitar el acceso no supervisado. |
|Subscription|30|Habilitar MFA para las cuentas de la aplicación de administración de Azure con permisos de lectura en la suscripción|Habilite Multi-Factor Authentication (MFA) para todas las cuentas de suscripción con privilegios de lectura para evitar una vulneración de las cuentas o recursos.|
|Subscription|25|Quitar las cuentas externas con permisos de escritura de la suscripción|Elimine las cuentas externas con permisos de escritura de la suscripción a fin de evitar el acceso no supervisado. |
|Subscription|20|Quitar de la suscripción las cuentas en desuso con permisos de propietario|Quite de la suscripción las cuentas en desuso con permisos de propietario.|
|Subscription|5|Quitar las cuentas en desuso de la suscripción|Quite las cuentas en desuso de sus suscripciones para habilitar el acceso a solo los usuarios actuales. |
|Subscription|5|Designar a más de un propietario en la suscripción|Designe a más de un propietario de la suscripción para tener redundancia de acceso de administrador.|
|Subscription|5|Designar a un máximo de 3 propietarios en la suscripción|Designe a un máximo de 3 propietarios de suscripción para reducir el riesgo de vulneración por parte de un propietario en peligro.|
|Almacén de claves|5|Habilitar los registros de diagnóstico en Key Vault|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Subscription|15|Quitar las cuentas externas con permisos de lectura de la suscripción|Elimine las cuentas externas con privilegios de lectura de la suscripción a fin de evitar el acceso no supervisado.|
|Subscription|1|Proporcionar datos de los contactos de seguridad|Proporcione información de los contactos de seguridad para cada una de sus suscripciones. La información de los contactos es una dirección de correo electrónico y un número de teléfono. La información se usará para establecer contacto con usted si nuestro equipo de seguridad descubre que sus recursos están en peligro.|

> [NOTA] Si ha creado una directiva de acceso condicional que requiere MFA pero tiene exclusiones establecidas, la evaluación de la recomendación de MFA de Security Center considera la directiva no compatible, porque permite que algunos usuarios inicien sesión en Azure sin MFA.
>

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

- [Protección de las aplicaciones y las máquinas en Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
- [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Aprenda a administrar las alertas y a responder a incidentes de seguridad en Security Center.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md). Obtenga respuestas a las preguntas más frecuentes acerca de Security Center.


<!--Image references-->
[1]: ./media/security-center-identity-access/overview.png
[2]: ./media/security-center-identity-access/identity-dashboard.png
[3]: ./media/security-center-identity-access/select-subscription.png
[4]: ./media/security-center-identity-access/subscriptions.png
[5]: ./media/security-center-identity-access/recommendations.png
[6]: ./media/security-center-identity-access/designate.png
[7]: ./media/security-center-identity-access/passed-assessments.png
[8]: ./media/security-center-identity-access/remove.png
