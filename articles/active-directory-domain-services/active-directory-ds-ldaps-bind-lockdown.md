---
title: Enlace mediante LDAP seguro (LDAPS) a un dominio administrado de Azure AD Domain Services | Microsoft Docs
description: Enlace a un dominio administrado de Azure AD Domain Services con LDAP seguro (LDAPS)
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6871374a-0300-4275-9a45-a39a52c65ae4
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: df0b3d27eec478280a33be831a2431eccdf05a74
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483380"
---
# <a name="bind-to-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Enlace a un dominio administrado de Azure AD Domain Services con LDAP seguro (LDAPS)

## <a name="before-you-begin"></a>Antes de empezar
Realice la [Tarea 4: configuración de DNS para acceder al dominio administrado desde Internet](active-directory-ds-ldaps-configure-dns.md).


## <a name="task-5-bind-to-the-managed-domain-over-ldap-using-ldpexe"></a>Tarea 5: Enlazado al dominio administrado sobre LDAP mediante LDP.exe
Puede utilizar la herramienta LDP.exe que se incluye en el paquete de herramientas de administración de servidor remoto para enlazar y buscar con LDAP.

En primer lugar, abra LDP y conéctese al dominio administrado. Haga clic en **Conexión** y haga clic en **Conectar...** en el menú. Especifique el nombre de dominio DNS del dominio administrado. Especifique el puerto que se va a utilizar para las conexiones. Para las conexiones LDAP, utilice el puerto 389. Para las conexiones LDAPS, utilice el puerto 636. Haga clic en el botón **Aceptar** para conectarse al dominio administrado.

Después, enlace al dominio administrado. Haga clic en **Conexión** y haga clic en **Enlazar...**  en el menú. Proporcione las credenciales de una cuenta de usuario que pertenezca al grupo "Administradores del controlador de dominio de AAD".

> [!IMPORTANT]
> Los usuarios (y las cuentas de servicio) no pueden realizar enlaces LDAP simples si ha desactivado la sincronización de hash de contraseñas NTLM en la instancia de Azure AD Domain Services.  Para más información acerca de cómo deshabilitar la sincronización de hash de contraseñas NTLM, lea [Protección de un dominio administrado con Azure AD Domain Services](secure-your-domain.md).
>
>

Seleccione **Ver** y, después, seleccione **Árbol** en el menú. Deje en blanco el campo DN Base y haga clic en Aceptar. Vaya al contenedor en el que desea buscar, haga clic con el botón derecho en el contenedor y seleccione Buscar.

> [!TIP]
> - Los usuarios y grupos sincronizados desde Azure AD se almacenan en la unidad organizativa **AADDC Users**. La ruta de acceso de búsqueda de esta unidad organizativa se parece a ```OU=AADDC Users,DC=CONTOSO100,DC=COM```.
> - Las cuentas de equipo de los equipos unidos al dominio administrado se almacenan en la unidad organizativa **AADDC Computers**. La ruta de acceso de búsqueda de esta unidad organizativa se parece a ```OU=AADDC Computers,DC=CONTOSO100,DC=COM```.
>
>

Más información: [LDAP query basics](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter) (Conceptos básicos de las consultas LDAP)


## <a name="task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>Tarea 6: bloqueo del acceso LDAP seguro al dominio administrado a través de Internet
> [!NOTE]
> Si no ha habilitado el acceso LDAPS en el dominio administrado a través de Internet, omita esta tarea de configuración.
>
>

Antes de comenzar esta tarea, complete los pasos que se describen en la [tarea 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

Cuando habilita el acceso LDAPS sobre Internet al dominio administrado, se crea una amenaza de seguridad. Se puede alcanzar el dominio administrado desde Internet en el puerto usado para LDAP seguro (es decir, el puerto 636). Puede elegir restringir el acceso al dominio administrado a direcciones IP conocidas específicas. Cree un grupo de seguridad de red (NSG) y asócielo a la subred en la que habilitó Azure AD Domain Services.

El NSG de ejemplo de la tabla siguiente bloquea el acceso LDAP seguro sobre Internet. Las reglas del NSG permiten el acceso LDAP seguro de entrada a través del puerto TCP 636 solo desde un conjunto especificado de direcciones IP. La regla predeterminada 'DenyAll' se aplica a todo el tráfico de entrada de Internet. La regla NSG para permitir el acceso LDAPS a través de Internet desde direcciones IP especificadas tiene una prioridad mayor que la regla NSG DenyAll.

![NSG de muestra para el acceso LDAPS seguro a través de Internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Más información** - [Grupos de seguridad de red](../virtual-network/security-overview.md).


## <a name="related-content"></a>Contenido relacionado
* [Introducción a Azure AD Domain Services](create-instance.md)
* [Administrar un dominio de Azure AD Domain Services](manage-domain.md)
* [LDAP query basics](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter) (Conceptos básicos de las consultas LDAP)
* [Administración de la directiva de grupo para Azure AD Domain Services](manage-group-policy.md)
* [Grupos de seguridad de red](../virtual-network/security-overview.md)
* [Creación de un grupo de seguridad de red](../virtual-network/tutorial-filter-network-traffic.md)


## <a name="next-step"></a>Paso siguiente
[Solución de problemas de LDAP seguro en un dominio administrado](tshoot-ldaps.md)
