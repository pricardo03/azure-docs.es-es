---
title: Administración de conceptos para Azure AD Domain Services | Microsoft Docs
description: Obtenga información sobre cómo administrar un dominio administrado de Azure Active Directory Domain Services y el comportamiento de las cuentas de usuario y las contraseñas
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: iainfou
ms.openlocfilehash: b82927efa9054e71379d01993d1669527bc71402
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249414"
---
# <a name="management-concepts-for-user-accounts-passwords-and-administration-in-azure-active-directory-domain-services"></a>Conceptos de administración para cuentas de usuario, contraseñas y administración en Azure Active Directory Domain Services

Al crear y ejecutar un dominio administrado de Azure Active Directory Domain Services (AD DS), hay algunas diferencias en el comportamiento en comparación con un entorno de AD DS local tradicional. Puede usar las mismas herramientas administrativas en Azure AD DS como dominio autoadministrado, pero no puede obtener acceso directo a los controladores de dominio (DC). Hay algunas diferencias en el comportamiento para las directivas de contraseña y los valores hash de contraseñas dependiendo del origen de la creación de cuentas de usuario.

En este artículo conceptual se detalla cómo administrar un dominio administrado de Azure AD DS y el comportamiento diferente de las cuentas de usuario dependiendo de cómo se crean.

## <a name="domain-management"></a>Administración de dominios

En Azure AD DS, los controladores de dominio (DC) que contienen todos los recursos, como usuarios y grupos, credenciales y directivas, forman parte del servicio administrado. Para ofrecer redundancia, se crean dos DC como parte de un dominio administrado de Azure AD DS. No puede iniciar sesión en estos DC para realizar tareas de administración. En su lugar, cree una máquina virtual de administración unida al dominio administrado de Azure AD DS y, a continuación, instale sus herramientas de administración de AD DS regulares. Puede usar los complementos del Centro de administración de Active Directory o Microsoft Management Console (MMC) como, por ejemplo, objetos de directiva de grupo o DNS.

## <a name="user-account-creation"></a>Creación de una cuenta de usuario

Las cuentas de usuario se pueden crear en Azure AD DS de varias formas. La mayoría de las cuentas de usuario se sincronizan desde Azure AD, lo que también puede incluir cuentas de usuario sincronizadas desde un entorno de AD DS local. También puede crear de forma manual cuentas directamente en Azure AD DS. Algunas características, como la directiva de contraseñas o la sincronización de contraseña inicial, se comportan de forma diferente dependiendo de cómo y dónde se crean las cuentas de usuario.

* La cuenta de usuario se puede sincronizar desde Azure AD. Esto incluye las cuentas de usuario solo en la nube creadas directamente en Azure AD y las cuentas de usuario híbridas sincronizadas desde un entorno de AD DS local mediante Azure AD Connect.
    * La mayoría de las cuentas de usuario en Azure AD se crean a través del proceso de sincronización de Azure AD.
* La cuenta de usuario se puede crear manualmente en un dominio administrado de Azure AD DS y no existe en Azure AD.
    * Si tiene que crear cuentas de servicio para aquellas aplicaciones que solo se ejecutan en Azure AD DS, puede crearlas manualmente en el dominio administrado. Como la sincronización es unidireccional desde Azure AD, las cuentas de usuario creadas en Azure AD DS no se vuelven a sincronizar con Azure AD.

## <a name="password-policy"></a>Directiva de contraseñas

Azure AD DS incluye una directiva de contraseñas predeterminada que define la configuración para elementos como el bloqueo de cuenta, la antigüedad máxima de contraseña y la complejidad de la contraseña. Opciones de configuración como la directiva de bloqueo de cuentas se aplican a todos los usuarios en Azure AD DS, independientemente de cómo se creó el usuario tal como se señala en la sección anterior. Algunos valores de configuración, como la longitud mínima de contraseña y la complejidad de la contraseña, solo se aplican a los usuarios creados directamente en Azure AD DS.

Puede crear sus propias directivas de contraseñas personalizadas para reemplazar la directiva predeterminada en Azure AD DS. Estas directivas personalizadas se pueden aplicar a continuación a grupos de usuarios específicos según sea necesario.

Para obtener más información sobre las diferencias en el modo en que se aplican las directivas de contraseñas dependiendo del origen de la creación de usuarios, consulte [Directivas de bloqueo de cuenta y contraseña en dominios administrados][password-policy].

## <a name="password-hashes"></a>Valores hash de contraseñas

Para autenticar a los usuarios en el dominio administrado, Azure AD DS necesita los hash de las contraseñas en un formato adecuado para la autenticación de NT LAN Manager (NTLM) y Kerberos. A menos que habilite Azure AD DS para el inquilino, Azure AD no genera ni almacena los hash de las contraseñas en el formato necesario para la autenticación NTLM o Kerberos. Por motivos de seguridad, Azure AD tampoco almacena las credenciales de contraseñas en forma de texto sin cifrar. Por consiguiente, Azure AD no tiene forma de generar automáticamente estos hash de las contraseñas de NTLM o Kerberos basándose en las credenciales existentes de los usuarios.

En el caso de las cuentas de usuario solo de nube, los usuarios deben cambiar sus contraseñas para poder usar Azure AD DS. Este proceso de cambio de contraseña hace que los valores hash de contraseña para la autenticación Kerberos y NTLM se generen y almacenen en Azure AD.

Para los usuarios sincronizados desde un entorno de AD DS local mediante Azure AD Connect, [habilite la sincronización de los valores hash de contraseñas][hybrid-phs].

> [!IMPORTANT]
> Azure AD Connect solo sincroniza los hashes de contraseña heredados al habilitar Azure AD DS para su inquilino de Azure AD. Los valores hash heredados no se usan si solo utiliza Azure AD Connect para sincronizar un entorno de AD DS local con Azure AD.
>
> Si las aplicaciones heredadas no utilizan la autenticación NTLM o los enlaces simples LDAP, se recomienda deshabilitar la sincronización de hash de contraseña de NTLM para Azure AD DS. Para más información, consulte [Deshabilitación de la sincronización de hash de credenciales NTLM y de conjuntos de cifrado débil][secure-domain].

Una vez configurados correctamente, los hash de las contraseñas que se pueden usar se almacenan en el dominio administrado de Azure AD DS. Si elimina el dominio administrado de Azure AD DS, también se eliminarán los hash de las contraseñas que haya almacenados en ese momento. La información de credenciales sincronizada en Azure AD no se puede volver a usar si posteriormente crea un dominio administrado de Azure AD DS: debe volver a configurar la sincronización de los hash de contraseña para almacenarlos de nuevo. Los usuarios o las máquinas virtuales anteriormente unidos a un dominio no podrán autenticarse de inmediato, ya que Azure AD debe generar y almacenar los hash de contraseña en el nuevo dominio administrado de Azure AD DS. Para más información, consulte [Proceso de sincronización de hash de contraseña para Azure AD DS y Azure AD Connect][azure-ad-password-sync].

## <a name="next-steps"></a>Pasos siguientes

Para empezar, [cree un dominio administrado de Azure AD DS][create-instance].

<!-- INTERNAL LINKS -->
[password-policy]: password-policy.md
[hybrid-phs]: tutorial-configure-password-hash-sync.md#enable-synchronization-of-password-hashes
[secure-domain]: secure-your-domain.md
[azure-ad-password-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[create-instance]: tutorial-create-instance.md
