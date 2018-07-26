---
title: 'Azure Active Directory Domain Services: dominios suspendidos | Microsoft Docs'
description: Suspensión y eliminación de dominios administrados
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: ergreenl
ms.openlocfilehash: 99896b0f618151cd368a21c4a212b7d8e28b7a6f
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126100"
---
# <a name="suspended-domains"></a>Dominios suspendidos
Cuando Azure AD Domain Services no puede atender un dominio administrado durante un largo período de tiempo, el dominio administrado se coloca en un estado suspendido. En este artículo se explica por qué se suspenden los dominios administrados y cómo corregir un dominio suspendido.


## <a name="states-your-managed-domain-can-be-in"></a>Estados que pueden tener los dominios administrados

![Escala de tiempo de un dominio suspendido](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

En el gráfico anterior se describen los posibles estados que puede tener un dominio administrado de Azure AD Domain Services.

### <a name="running-state"></a>Estado "En ejecución"
Un dominio administrado que está configurado correctamente y que funciona de manera habitual tiene el estado **En ejecución**.

**Lo que se puede esperar:**
* Microsoft puede supervisar periódicamente el estado del dominio administrado.
* Los controladores de dominio del dominio administrado se revisan y actualizan de manera periódica.
* Los cambios que se realizan desde Azure Active Directory se sincronizan periódicamente con el dominio administrado.
* Se realizan copias de seguridad periódicas para el dominio administrado.


### <a name="needs-attention-state"></a>Estado "Requiere atención"
Un dominio administrado tiene el estado **Requiere atención** si uno o más problemas requieren que un administrador tome medidas. La página de estado del dominio administrado mostrará una o varias alertas con este estado. Por ejemplo, si configuró un NSG restrictivo para la red virtual, es posible que Microsoft no pueda actualizar ni supervisar el dominio administrado. Esta configuración no válida se traduce en que se genera una alerta y el dominio administrado se coloca en el estado "Requiere atención".

Cada alerta tiene un conjunto de pasos de resolución. Algunas alertas son transitorias y el servicio las resolverá de manera automática. Para resolver algunas otras alertas, siga las instrucciones que aparecen en los pasos de resolución correspondientes para esa alerta. Si quiere resolver algunas alertas críticas, debe ponerse en contacto con Soporte técnico de Microsoft.

Para más información, consulte [cómo solucionar problemas de alertas en un dominio administrado](active-directory-ds-troubleshoot-alerts.md).

**Lo que se puede esperar:**

En algunos casos (por ejemplo, si tiene una configuración de red no válida), es posible que no se pueda acceder a los controladores de dominio del dominio administrado. Microsoft no puede garantizar la supervisión, revisión, actualización ni copia de seguridad de un dominio administrado que se encuentre en este estado.

* El dominio administrado tiene un estado incorrecto y el seguimiento de estado actual se podría detener hasta que se resuelva la alerta.
* No se pueden revisar ni actualizar los controladores de dominio del dominio administrado.
* Los cambios que se realizan desde Azure Active Directory no se pueden sincronizar con el dominio administrado.
* Se pueden crear copias de seguridad del dominio administrado, si es posible.
* Si resuelve las alertas que afectan el dominio administrado, es posible restaurar el dominio administrado al estado "En ejecución".
* Las alertas críticas se desencadenan en caso de errores de configuración cuando Microsoft no puede acceder a los controladores de dominio. Si las alertas en cuestión no se resuelven dentro de 15 días, el dominio administrado se colocará en el estado "Suspendido".


### <a name="suspended-state"></a>Estado "Suspendido"
Un dominio administrado se coloca en el estado **Suspendido** por los motivos siguientes:
* Una o varias alertas críticas no se han resuelto en 15 días. Las alertas críticas pueden deberse a un error de configuración que bloquea el acceso de Azure AD Domain Services a los recursos necesarios.
    * Por ejemplo, si el dominio administrado tiene la alerta [AADDS104: Error de red](active-directory-ds-troubleshoot-nsg.md) no resuelta durante más de 15 días.
* Hay un problema con la facturación de la suscripción de Azure o la suscripción de Azure expiró.

Los dominios administrados se suspenden cuando no Microsoft no puede administrar, supervisar, aplicar revisiones ni crear copia de seguridad del dominio de manera habitual.

**Lo que se puede esperar:**
* Los controladores de dominio del dominio administrado se desaprovisionan y no son accesibles dentro de la red virtual.
* El acceso de LDAP seguro al dominio administrado a través de Internet (si se habilita) deja de funcionar.
* Se observan errores en la autenticación en el dominio administrado, el inicio de sesión en máquinas virtuales unidas al dominio y en la conexión a través de LDAP/LDAPS.
* Se dejan de crear copias de seguridad del dominio administrado.
* La sincronización con Azure AD se detiene.
* Resuelva la alerta para poner el dominio administrado en el estado "Suspendido" y póngase en contacto con el soporte técnico.
* El soporte técnico puede restaurar el dominio administrado solo si existe una copia de seguridad con menos de 30 días de antigüedad.

El dominio administrado solo permanecerá en un estado de suspensión durante 15 días. Para recuperar el dominio administrado, Microsoft recomienda resolver de inmediato las alertas críticas.


### <a name="deleted-state"></a>Estado "Eliminado"
Un dominio administrado que permanece en el estado "Suspendido" durante 15 días pasa al estado **Eliminado**.

**Lo que se puede esperar:**
* Se eliminan todos los recursos y las copias de seguridad del dominio administrado.
* No puede restaurar el dominio administrado y necesita crear un dominio administrado nuevo para usar Azure AD Domain Services.
* Una vez que lo elimine, no se le facturará el dominio administrado.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>¿Cómo es posible saber si el dominio administrado está suspendido?
Verá una [alerta](active-directory-ds-troubleshoot-alerts.md) en la página Azure AD Domain Services Health de Azure Portal que declara el dominio suspendido. El estado del dominio también muestra "Suspendido".


## <a name="restore-a-suspended-domain"></a>Restauración de un dominio suspendido
Para restaurar un dominio con estado "Suspendido", complete estos pasos:

1. Vaya a la [página de Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) en Azure Portal.
2. Haga clic en el dominio administrado.
3. En el panel de navegación de la izquierda, haga clic en **Estado**.
4. Haga clic en la alerta. El identificador de la alerta será AADDS503 o AADDS504, dependiendo de la causa de la suspensión.
5. Haga clic en el vínculo de resolución que se proporciona en la alerta y siga los pasos para resolverla.

El dominio administrado solo se puede restaurar a la fecha de la última copia de seguridad. La fecha de la última copia de seguridad se muestra en la página Estado del dominio administrado. No se restaurarán los cambios que se produjeron después de la última copia de seguridad. Las copias de seguridad de un dominio administrado se almacenan por hasta 30 días. Las copias de seguridad con una antigüedad mayor a los 30 días se eliminan.


## <a name="next-steps"></a>Pasos siguientes
- [Resuelva alertas en el dominio administrado](active-directory-ds-troubleshoot-alerts.md)
- [Más información sobre Azure AD Domain Services](active-directory-ds-overview.md)
- [Póngase en contacto con el equipo de productos](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Ponerse en contacto con nosotros
Póngase en contacto con el equipo de productos de Active Directory Domain Services para [compartir comentarios u obtener asistencia](active-directory-ds-contact-us.md).
