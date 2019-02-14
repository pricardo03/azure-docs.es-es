---
title: Configuración de Azure Security Center | Microsoft Docs
description: Configure Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: f61f5ebf4c0658ff846421682d50ea346c699e01
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56107936"
---
# <a name="security-center-settings"></a>Configuración de Security Center
Este artículo contiene información general sobre la configuración de Security Center.

Las siguientes opciones están disponibles en Directiva de seguridad:

- **Recopilación de datos**: determina el aprovisionamiento del agente y la configuración de la [colección de datos](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection).
- **Directiva de seguridad**: determina qué controles Security Center supervisa y recomienda. Puede editar la [directiva de seguridad](tutorial-security-policy.md) en Security Center. También puede usar [Azure Policy](tutorial-security-policy.md) para crear definiciones, definir directivas adicionales y asignar directivas en los grupos de administración. 
- **Notificaciones por correo electrónico**: determina los contactos de seguridad y la configuración de [notificaciones por correo electrónico](security-center-provide-security-contact-details.md).
- **Plan de tarifa**: define la [selección de precios](security-center-pricing.md) gratuitos o estándar. El plan elegido determina qué características de Security Center están disponibles para los recursos del ámbito. Puede especificar un plan para las suscripciones, los grupos de recursos y las áreas de trabajo.

> [!NOTE]
> Puede establecerlas todas por suscripción. En Áreas de trabajo, solo puede establecer Recopilación de datos y Plan de tarifa. En Grupos de recursos, solo puede establecer Plan de tarifa.
>


## <a name="who-can-edit-security-policies"></a>¿Quién puede editar directivas de seguridad?
Security Center usa el control de acceso basado en rol (RBAC), que proporciona roles integrados que se pueden asignar a usuarios, grupos y servicios en Azure. Cuando un usuario abre Security Center, solo ve la información relacionada con los recursos a los que tiene acceso. Esto significa que a los usuarios se les asigna el rol de *Propietario*, *Colaborador* o *Lector* para la suscripción o el grupo de recursos a los que pertenece un recurso. Además de estos roles, hay dos roles específicos de Security Center:

- **Lector de seguridad**: el usuario tiene derecho a visualizar el contenido de Security Center (recomendaciones, alertas, directivas y estados) pero no puede realizar cambios.
- **Administrador de seguridad**: tiene los mismos derechos que el *lector de seguridad*, pero también puede actualizar la directiva de seguridad o descartar recomendaciones y alertas.


## <a name="next-steps"></a>Pasos siguientes
En este artículo ha aprendido a configurar directivas de seguridad en Azure Security Center. Para obtener más información sobre Azure Security Center, consulte los siguientes artículos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): Aprenda cómo las recomendaciones de Security Center como ayuda para proteger los recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): Aprenda a administrar y responder a las alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): Aprenda cómo supervisar el estado de mantenimiento de las soluciones de sus asociados.
- [Seguridad de datos de Azure Security Center](security-center-data-security.md): Aprenda cómo Security Center administra y protege los datos.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): Obtenga respuestas para las preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): obtenga las últimas noticias e información sobre la seguridad en Azure.
