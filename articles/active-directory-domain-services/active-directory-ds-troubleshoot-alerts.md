---
title: 'Azure Active Directory Domain Services: Solución de problemas de alertas | Microsoft Docs'
description: Solución de problemas de alertas para Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: ergreenl
ms.openlocfilehash: 7d99f5a5d027c825fa1145328bb9576229ce39b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60416524"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services: solución de problemas de alertas
En este artículo se brinda guías para la solución de problemas de cualquier alerta que pueda recibir en su dominio administrado.


Elija los pasos de solución de problemas que correspondan al identificador o mensaje de la alerta.

| **Id. de alerta** | **Mensaje de alerta** | **Resolución** |
| --- | --- | :--- |
| AADDS001 | *LDAP seguro a través de Internet está habilitado para el dominio administrado. Sin embargo, el acceso al puerto 636 no está bloqueado mediante un grupo de seguridad de red. Esto puede exponer las cuentas de usuario del dominio administrado a los ataques de fuerza bruta de contraseñas.* | [Configuración incorrecta de LDAP seguro](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Es posible que se haya eliminado el directorio de Azure AD asociado con su dominio administrado. El dominio administrado ya no está en una configuración admitida. Microsoft no puede supervisar, administrar, revisar ni sincronizar el dominio administrado.* | [Falta un directorio](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services no se puede habilitar en un directorio de Azure AD B2C.* | [Azure AD B2C se ejecuta en este directorio](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Una entidad de servicio necesaria para que Azure AD Domain Services funcione correctamente se ha eliminado del directorio de Azure AD. Esta configuración afecta a la capacidad de Microsoft para supervisar, administrar, revisar y sincronizar el dominio administrado.* | [Falta una entidad de servicio](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *El intervalo de direcciones IP de la red virtual en la que habilitó Azure AD Domain Services está en un intervalo IP público. Azure AD Domain Services debe estar habilitado en una red virtual con un intervalo de direcciones IP privado. Esta configuración afecta a la capacidad de Microsoft para supervisar, administrar, revisar y sincronizar el dominio administrado.* | [La dirección está en un intervalo IP público](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft no puede tener acceso a los controladores de dominio de este dominio administrado. Esto puede ocurrir si un grupo de seguridad de red (NSG) configurado en la red virtual bloquea el acceso al dominio administrado. Otro motivo posible es que hay una ruta definida por el usuario que bloquea el tráfico entrante desde Internet.* | [Error de red](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *Se eliminó la entidad de servicio con el identificador de aplicación "d87dcbc6-a371-462e-88e3-28ad15ec4e64" y, a continuación, se volvió a crear. La regeneración excluye los permisos incoherentes en los recursos de Azure AD Domain Services necesarios para atender su dominio administrado. La sincronización de contraseñas en el dominio administrado podría verse afectada.* | [La aplicación de sincronización de contraseñas no está actualizada](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS106 | *Se ha eliminado la suscripción de Azure asociada a su dominio administrado.  Azure AD Domain Services requiere una suscripción activa para seguir funcionando correctamente.* | [No se encuentra la suscripción](#aadds106-your-azure-subscription-is-not-found) |
| AADDS107 | *La suscripción de Azure asociada a su dominio administrado no está activa.  Azure AD Domain Services requiere una suscripción activa para seguir funcionando correctamente.* | [La suscripción de Azure está deshabilitada](#aadds107-your-azure-subscription-is-disabled) |
| AADDS108 | *La suscripción que utiliza Azure AD Domain Services se ha movido a otro directorio. Azure AD Domain Services necesita tener una suscripción activa en el mismo directorio para que funcione correctamente.* | [Suscripción ha movido los directorios](#aadds108-subscription-moved-directories) |
| AADDS109 | *Se ha eliminado un recurso que se usa con el dominio administrado. Este recurso es necesario para que Azure AD Domain Services funcione correctamente.* | [Se ha eliminado un recurso](#aadds109-resources-for-your-managed-domain-cannot-be-found) |
| AADDS110 | *La subred seleccionada para la implementación de Azure AD Domain Services está llena y carece de espacio para el controlador de dominio adicional que hay que crear.* | [La subred está completa](#aadds110-the-subnet-associated-with-your-managed-domain-is-full) |
| AADDS111 | *Una entidad de servicio que Azure AD Domain Services utiliza para atender a su dominio no está autorizado para administrar recursos en la suscripción de Azure. La entidad de servicio debe tener permisos para atender al dominio administrado.* | [Entidad de servicio no autorizada](#aadds111-service-principal-unauthorized) |
| AADDS112 | *Hemos identificado que la subred de la red virtual de este dominio no tiene suficientes direcciones IP. Azure AD Domain Services necesita como mínimo dos direcciones IP disponibles en la subred en que se habilita. Se recomienda tener al menos de tres a cinco direcciones IP de reserva en la subred. Esto puede deberse a que se hayan implementado otras máquinas virtuales dentro de la subred, agotando así el número de direcciones IP disponibles, o a que haya una restricción en el número de direcciones IP disponibles en la subred.* | [No hay suficientes direcciones IP](#aadds112-not-enough-ip-address-in-the-managed-domain) |
| AADDS113 | *Se ha detectado que los recursos que utiliza Azure AD Domain Services tienen un estado inesperado y no se pueden recuperar.* | [Los recursos son irrecuperables](#aadds113-resources-are-unrecoverable) |
| AADDS114 | *La subred seleccionada para la implementación de Azure AD Domain Services no es válida y no se puede utilizar.* | [Subred no válida](#aadds114-subnet-invalid) |
| AADDS115 | *Uno o varios de los recursos de red que se utilizan en el dominio administrado no pueden ejecutarse porque el ámbito de destino se ha bloqueado.* | [Los recursos están bloqueados](#aadds115-resources-are-locked) |
| AADDS116 | *Uno o varios de los recursos de red que se utilizan en el dominio administrado no pueden ejecutarse debido a restricciones de directiva.* | [Los recursos no son utilizables](#aadds116-resources-are-unusable) |
| AADDS500 | *El dominio administrado se sincronizó por última vez con Azure AD el [fecha]. Los usuarios no pueden iniciar sesión en el dominio o puede que las pertenencias a grupos no estén sincronizadas con Azure AD.* | [Hace un tiempo que no se realiza una sincronización](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Se hizo copia de seguridad del dominio administrado por última vez el [fecha].* | [Hace un tiempo que no se realiza una copia de seguridad](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *El certificado LDAP seguro del dominio administrado expirará en [fecha].* | [Expiración del certificado LDAP seguro](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *El dominio administrado se suspende debido a que la suscripción de Azure asociada al dominio no está activa.* | [Suspensión debida a una suscripción deshabilitada](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *El dominio administrado se suspende debido a una configuración no válida. El servicio lleva mucho tiempo sin poder administrar, actualizar o aplicar revisiones a los controladores de dominio en el dominio administrado.* | [Suspensión debida a una configuración no válida](#aadds504-suspension-due-to-an-invalid-configuration) |



## <a name="aadds100-missing-directory"></a>AADDS100: Falta un directorio
**Mensaje de alerta:**

*Es posible que se haya eliminado el directorio de Azure AD asociado con su dominio administrado. El dominio administrado ya no está en una configuración admitida. Microsoft no puede supervisar, administrar, revisar ni sincronizar el dominio administrado.*

**Resolución:**

Este error habitualmente se produce por la migración incorrecta de la suscripción de Azure a un directorio de Azure AD nuevo y la eliminación del directorio de Azure AD anterior que sigue asociado con Azure AD Domain Services.

Este error es irrecuperable. Para resolverlo, debe [eliminar el dominio administrado existente](active-directory-ds-disable-aadds.md) y volver a crearlo en el directorio nuevo. Si tiene problemas para eliminarlo, póngase en contacto con el equipo de productos de Azure Active Directory Domain Services [para obtener asistencia](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C se ejecuta en este directorio
**Mensaje de alerta:**

*Azure AD Domain Services no se puede habilitar en un directorio de Azure AD B2C.*

**Resolución:**

>[!NOTE]
>Para seguir usando Azure AD Domain Services, debe volver a crear la instancia de Azure AD Domain Services en un directorio distinto de Azure AD B2C.

Para restaurar el servicio, siga estos pasos:

1. [Elimine el dominio administrado](active-directory-ds-disable-aadds.md) del directorio de Azure AD existente.
2. Cree un directorio que no sea un directorio de Azure AD B2C.
3. Siga la guía de [introducción](active-directory-ds-getting-started.md) para volver a crear un dominio administrado.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: La dirección está en un intervalo IP público

**Mensaje de alerta:**

*El intervalo de direcciones IP de la red virtual en la que habilitó Azure AD Domain Services está en un intervalo IP público. Azure AD Domain Services debe estar habilitado en una red virtual con un intervalo de direcciones IP privado. Esta configuración afecta a la capacidad de Microsoft para supervisar, administrar, revisar y sincronizar el dominio administrado.*

**Resolución:**

> [!NOTE]
> Para solucionar este problema, debe eliminar el dominio administrado existente y volver a crearlo en una red virtual con un intervalo de direcciones IP privado. Este proceso puede ser perjudicial.

Antes de comenzar, lea la sección sobre **el espacio de direcciones IPv4 privado** en [este artículo](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Dentro de la red virtual, las máquinas pueden realizar solicitudes a los recursos de Azure que se encuentran en el mismo intervalo de direcciones IP que los configurados para la subred. Sin embargo, puesto que la red virtual se configura para este intervalo, esas solicitudes se enrutarán dentro de la red virtual y no llegarán a los recursos web deseados. Esta configuración puede conducir a errores impredecibles con Azure AD Domain Services.

**Si es propietario del intervalo de direcciones IP que está configurado en la red virtual, puede omitir esta alerta. Sin embargo, no se puede confirmar Azure AD Domain Services para el [Acuerdo de nivel de servicio](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] con esta configuración, ya que puede conducir a errores impredecibles.**


1. [Elimine el dominio administrado](active-directory-ds-disable-aadds.md) del directorio.
2. Corrija el intervalo de direcciones IP de la subred.
   1. Navegue a la [página de redes virtuales en Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
   2. Seleccione la red virtual que planea usar para Azure AD Domain Services.
   3. En Configuración, haga clic en **Espacio de direcciones**.
   4. Para actualizar el intervalo de direcciones, haga clic en el existente y edítelo o agregue un intervalo de direcciones adicional. Asegúrese de que el intervalo de direcciones nuevo esté en un intervalo IP privado. Guarde los cambios.
   5. En el menú de navegación de la izquierda, haga clic en **Subredes**.
   6. Haga clic en la subred que desea editar en la tabla.
   7. Actualice el intervalo de direcciones y guarde los cambios.
3. Siga la [guía de introducción al uso de Azure AD Domain Services](active-directory-ds-getting-started.md) para volver a crear el dominio administrado. Asegúrese de elegir una red virtual con un intervalo de direcciones IP privado.
4. Para unir las máquinas virtuales al dominio nuevo, siga [esta guía](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Para asegurarse de que la alerta se resuelve, compruebe el estado de su dominio transcurridas dos horas.

## <a name="aadds106-your-azure-subscription-is-not-found"></a>AADDS106: Su suscripción de Azure no se encuentra

**Mensaje de alerta:**

*Se ha eliminado la suscripción de Azure asociada a su dominio administrado.  Azure AD Domain Services requiere una suscripción activa para seguir funcionando correctamente.*

**Resolución:**

Azure AD Domain Services requiere una suscripción para funcionar y no puede moverse a otra suscripción. Dado que se ha eliminado la suscripción de Azure a la que estaba asociada el dominio administrado, ha de volver a crear una suscripción de Azure y una instancia de Azure AD Domain Services.

1. Cree una suscripción a Azure
2. [Elimine el dominio administrado](active-directory-ds-disable-aadds.md) del directorio de Azure AD existente.
3. Siga la guía de [introducción](active-directory-ds-getting-started.md) para volver a crear un dominio administrado.

## <a name="aadds107-your-azure-subscription-is-disabled"></a>AADDS107: Su suscripción de Azure está deshabilitada

**Mensaje de alerta:**

*La suscripción de Azure asociada a su dominio administrado no está activa.  Azure AD Domain Services requiere una suscripción activa para seguir funcionando correctamente.*

**Resolución:**


1. [Renueve su suscripción de Azure](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable).
2. Cuando haya renovado la suscripción, Azure AD Domain Services recibirá una notificación de Azure para que vuelva a habilitar el dominio administrado.

## <a name="aadds108-subscription-moved-directories"></a>AADDS108: La suscripción ha movido los directorios

**Mensaje de alerta:**

*La suscripción que utiliza Azure AD Domain Services se ha movido a otro directorio. Azure AD Domain Services necesita tener una suscripción activa en el mismo directorio para que funcione correctamente.*

**Resolución:**

Puede mover la suscripción asociada con Azure AD Domain Services al directorio anterior, o necesita [eliminar el dominio administrado](active-directory-ds-disable-aadds.md) del directorio existente y volver a crearlo en el directorio elegido (ya sea con una nueva suscripción o mediante el cambio del directorio en el que se encuentra la instancia de Azure AD Domain Services).

## <a name="aadds109-resources-for-your-managed-domain-cannot-be-found"></a>AADDS109: No se encuentran recursos del dominio administrado

**Mensaje de alerta:**

*Se ha eliminado un recurso que se usa con el dominio administrado. Este recurso es necesario para que Azure AD Domain Services funcione correctamente.*

**Resolución:**

Azure AD Domain Services crea recursos específicos durante la implementación para poder funcionar correctamente, lo que incluye las direcciones IP públicas, las NIC y un equilibrador de carga. Si se elimina alguno de los elementos designados, hace que el dominio administrado se encuentre en un estado incompatible e impide que se administre el dominio. Esta alerta se genera cuando un usuario que puede modificar los recursos de Azure AD Domain Services elimina un recurso necesario. Los siguientes pasos describen cómo restaurar el dominio administrado.

1. Navegación a la página de mantenimiento de Azure AD Domain Services
   1.    Vaya a la página [Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) de Azure Portal.
   2.    En el menú de navegación izquierdo, haga clic en **Mantenimiento**.
2. Comprobación de si la alerta tiene menos de cuatro horas de duración
   1.    En la página de mantenimiento, haga clic en la alerta con el identificador **AADDS109**
   2.    La alerta tendrá una marca de tiempo del momento en que se encontró por primera vez. Si la marca de tiempo tiene menos de cuatro horas de duración, existe la posibilidad de que Azure AD Domain Services vuelva a crear el recurso eliminado.
3. Si tiene más, el dominio administrado se encuentra en un estado irrecuperable. Debe eliminar la instancia de Azure AD Domain Services y volver a crearla.


## <a name="aadds110-the-subnet-associated-with-your-managed-domain-is-full"></a>AADDS110: La subred asociada al dominio administrado está completa

**Mensaje de alerta:**

*La subred seleccionada para la implementación de Azure AD Domain Services está llena y carece de espacio para el controlador de dominio adicional que hay que crear.*

**Resolución:**

Este error es irrecuperable. Para resolver esto, debe [eliminar el dominio administrado existente](active-directory-ds-disable-aadds.md) y [volver a crear el dominio administrado](active-directory-ds-getting-started.md).

## <a name="aadds111-service-principal-unauthorized"></a>AADDS111: Entidad de servicio no autorizada

**Mensaje de alerta:**

*Una entidad de servicio que Azure AD Domain Services utiliza para atender a su dominio no está autorizado para administrar recursos en la suscripción de Azure. La entidad de servicio debe tener permisos para atender al dominio administrado.*

**Resolución:**

Nuestras entidades de servicio necesitan acceso para poder administrar y crear recursos en su dominio administrado. Alguien ha denegado el acceso a la entidad de servicio y ahora no puede administrar los recursos. Siga los pasos para conceder acceso a la entidad de servicio.

1. Lea sobre [Control de RBAC y cómo conceder acceso a las aplicaciones en Azure Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
2. Revise el acceso de la entidad de servicio con identificador ```abba844e-bc0e-44b0-947a-dc74e5d09022``` y conceda el acceso que se ha denegado en una fecha anterior.


## <a name="aadds112-not-enough-ip-address-in-the-managed-domain"></a>AADDS112: No hay suficientes direcciones IP en el dominio administrado

**Mensaje de alerta:**

*Hemos identificado que la subred de la red virtual de este dominio no tiene suficientes direcciones IP. Azure AD Domain Services necesita como mínimo dos direcciones IP disponibles en la subred en que se habilita. Se recomienda tener al menos de tres a cinco direcciones IP de reserva en la subred. Esto puede deberse a que se hayan implementado otras máquinas virtuales dentro de la subred, agotando así el número de direcciones IP disponibles, o a que haya una restricción en el número de direcciones IP disponibles en la subred.*

**Resolución:**

1. Elimine el dominio administrado del inquilino.
2. Corrija el intervalo de direcciones IP de la subred.
   1. Navegue a la [página de redes virtuales en Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
   2. Seleccione la red virtual que planea usar para Azure AD Domain Services.
   3. En Configuración, haga clic en **Espacio de direcciones**.
   4. Para actualizar el intervalo de direcciones, haga clic en el existente y edítelo o agregue un intervalo de direcciones adicional. Guarde los cambios.
   5. En el menú de navegación de la izquierda, haga clic en **Subredes**.
   6. Haga clic en la subred que desea editar en la tabla.
   7. Actualice el intervalo de direcciones y guarde los cambios.
3. Siga la [guía de introducción al uso de Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) para volver a crear el dominio administrado. Asegúrese de elegir una red virtual con un intervalo de direcciones IP privado.
4. Para unir las máquinas virtuales al dominio nuevo, siga [esta guía](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).
5. Compruebe el estado del dominio en dos horas para asegurarse de que ha completado los pasos correctamente.

## <a name="aadds113-resources-are-unrecoverable"></a>AADDS113: Los recursos son irrecuperables

**Mensaje de alerta:**

*Se ha detectado que los recursos que utiliza Azure AD Domain Services tienen un estado inesperado y no se pueden recuperar.*

**Resolución:**

Este error es irrecuperable. Para resolver esto, debe [eliminar el dominio administrado existente](active-directory-ds-disable-aadds.md) y [volver a crear el dominio administrado](active-directory-ds-getting-started.md).

## <a name="aadds114-subnet-invalid"></a>AADDS114: Subred no válida

**Mensaje de alerta:**

*La subred seleccionada para la implementación de Azure AD Domain Services no es válida y no se puede utilizar.*

**Resolución:**

Este error es irrecuperable. Para resolver esto, debe [eliminar el dominio administrado existente](active-directory-ds-disable-aadds.md) y [volver a crear el dominio administrado](active-directory-ds-getting-started.md).

## <a name="aadds115-resources-are-locked"></a>AADDS115: Los recursos están bloqueados

**Mensaje de alerta:**

*Uno o varios de los recursos de red que se utilizan en el dominio administrado no pueden ejecutarse porque el ámbito de destino se ha bloqueado.*

**Resolución:**

1.  Revise los registros de operaciones de Resource Manager en los recursos de red (esto debería dar información sobre el bloqueo que impide la modificación).
2.  Quite los bloqueos en los recursos para que la entidad de servicio de Azure AD Domain Services puede operar en ellos.

## <a name="aadds116-resources-are-unusable"></a>AADDS116: Los recursos no son utilizables

**Mensaje de alerta:**

*Uno o varios de los recursos de red que se utilizan en el dominio administrado no pueden ejecutarse debido a restricciones de directiva.*

**Resolución:**

1.  Revise los registros de operaciones de Resource Manager en los recursos de red del dominio administrado.
2.  Debilite las restricciones de directiva en los recursos para que la entidad de servicio de AAD-DS puede operar en ellos.



## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Hace un tiempo que no se realiza una sincronización

**Mensaje de alerta:**

*El dominio administrado se sincronizó por última vez con Azure AD el [fecha]. Los usuarios no pueden iniciar sesión en el dominio o puede que las pertenencias a grupos no estén sincronizadas con Azure AD.*

**Resolución:**

[Compruebe el mantenimiento del dominio](active-directory-ds-check-health.md) para ver las alertas que podrían indicar problemas en la configuración del dominio administrado. En ocasiones, los problemas con la configuración pueden bloquear la capacidad de Microsoft para sincronizar el dominio administrado. Si puede resolver las alertas, espere dos horas y compruebe de nuevo para ver si se ha completado la sincronización.

Estas son algunas causas habituales de por qué se detiene la sincronización en los dominios administrados:
- La conexión de red se bloquea en el dominio administrado. Para más información sobre la comprobación de la red para detectar si hay problemas, consulte cómo [solucionar problemas de grupos de seguridad de red](active-directory-ds-troubleshoot-nsg.md) y los [requisitos de red para Azure AD Domain Services](active-directory-ds-networking.md).
-  La sincronización de contraseñas nunca se configuró ni se realizó. Para configurar la sincronización de contraseñas, lea [este artículo](active-directory-ds-getting-started-password-sync.md).

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Hace un tiempo que no se realiza una copia de seguridad

**Mensaje de alerta:**

*Se hizo copia de seguridad del dominio administrado por última vez el [fecha].*

**Resolución:**

[Compruebe el mantenimiento del dominio](active-directory-ds-check-health.md) para ver las alertas que podrían indicar problemas en la configuración del dominio administrado. En ocasiones, los problemas con la configuración pueden bloquear la capacidad de Microsoft para hacer una copia de seguridad del dominio administrado. Si puede resolver las alertas, espere dos horas y compruebe de nuevo para ver si se ha completado la copia de seguridad.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Suspensión debida a una suscripción deshabilitada

**Mensaje de alerta:**

*El dominio administrado se suspende debido a que la suscripción de Azure asociada al dominio no está activa.*

**Resolución:**

> [!WARNING]
> Si el dominio administrado se suspende durante un largo período, se expone a ser eliminado. Es mejor abordar la suspensión tan pronto como sea posible. Para más información, visite [este artículo](active-directory-ds-suspension.md).

Para restaurar el servicio, [renueve su suscripción de Azure](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) asociada con el dominio administrado.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Suspensión debida a una configuración no válida

**Mensaje de alerta:**

*El dominio administrado se suspende debido a una configuración no válida. El servicio lleva mucho tiempo sin poder administrar, actualizar o aplicar revisiones a los controladores de dominio en el dominio administrado.*

**Resolución:**

> [!WARNING]
> Si el dominio administrado se suspende durante un largo período, se expone a ser eliminado. Es mejor abordar la suspensión tan pronto como sea posible. Para más información, visite [este artículo](active-directory-ds-suspension.md).

[Compruebe el mantenimiento del dominio](active-directory-ds-check-health.md) para ver las alertas que podrían indicar problemas en la configuración del dominio administrado. Si puede resolver alguna de estas alertas, hágalo. Después, póngase en contacto con el servicio de soporte técnico para volver a habilitar la suscripción.


## <a name="contact-us"></a>Ponerse en contacto con nosotros
Póngase en contacto con el equipo de productos de Active Directory Domain Services para [compartir comentarios u obtener asistencia](active-directory-ds-contact-us.md).
