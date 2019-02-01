---
title: 'Azure Active Directory Domain Services: Características | Microsoft Docs'
description: Características de Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: ergreenl
ms.openlocfilehash: 9e4ab7aa4f61921d8b327404a266694349d78164
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163626"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Características
Las siguientes características están disponibles en los dominios administrados de Azure AD Domain Services.

* **Experiencia de implementación sencilla:** puede habilitar Azure AD Domain Services para el directorio de Azure AD mediante unos cuantos clics. El dominio administrado incluye las cuentas de usuario solo en la nube y las cuentas de usuario sincronizadas desde un directorio local.
* **Compatibilidad para la unión a dominio:** le resultará muy fácil unir a dominios equipos de la red virtual de Azure en la que está disponible el dominio administrado. La experiencia de unión a dominio en sistemas operativos de cliente y servidor de Windows funciona sin problemas en dominios ofrecidos por Azure AD Domain Services. También puede utilizar herramientas automatizadas de unión a dominio en esos dominios.
* **Instancia de un dominio por directorio de Azure AD:** puede crear un único dominio de Active Directory para cada directorio de Azure AD.
* **Crear dominios con nombres personalizados:** puede crear dominios con nombres personalizados (por ejemplo, "contoso100.com") mediante Azure AD Domain Services. Puede utilizar nombres de dominio comprobados o sin comprobar. Si lo desea, también puede crear un dominio con el sufijo de dominio integrado (es decir, *.onmicrosoft.com) que se ofrece a través del directorio de Azure AD.
* **Integración con Azure AD:** no es necesario configurar o administrar la replicación en Azure AD Domain Services. Las cuentas de usuario, las pertenencias a grupos y las credenciales de usuario (contraseñas) del directorio de Azure AD están disponibles automáticamente con Azure AD Domain Services. Los nuevos usuarios, grupos o cambios de atributos que se producen en el inquilino de Azure AD o en el directorio local se sincronizan automáticamente con Azure AD Domain Services.
* **Autenticación Kerberos y NTLM:** con compatibilidad para la autenticación Kerberos y NTLM, puede implementar las aplicaciones que dependen de la autenticación integrada de Windows.
* **Utilizar las credenciales y contraseñas corporativas:** las contraseñas para usuarios del inquilino de Azure AD funcionan con Azure AD Domain Services. Los usuarios pueden utilizar sus credenciales corporativas para máquinas de unión al dominio, iniciar sesión de forma interactiva o a través de escritorio remoto y autenticarse en el dominio administrado.
* **Enlace LDAP y compatibilidad con lectura LDAP**: puede utilizar las aplicaciones que dependen de enlaces LDAP para autenticar usuarios en dominios ofrecidos por Azure AD Domain Services. Además, las aplicaciones que utilizan operaciones de lectura LDAP para consultar los atributos de usuario y equipo desde el directorio también pueden trabajar con Servicios de dominio de Azure AD.
* **LDAP seguro (LDAPS):** puede habilitar el acceso al directorio sobre LDAP seguro (LDAPS). El acceso LDAP seguro está disponible de forma predeterminada dentro de la red virtual. Sin embargo, también tiene la opción de habilitar el acceso LDAP seguro a través de Internet.
* **Directiva de grupo:** puede aprovechar un único GPO integrado para los usuarios y contenedores de equipos para exigir el cumplimiento de directivas de seguridad necesarias para las cuentas de usuario, así como para los equipos unidos a dominios. También puede crear sus propios GPO personalizados y asignarlos a unidades organizativas personalizadas para [administrar la directiva de grupo](active-directory-ds-admin-guide-administer-group-policy.md).
* **Administrar DNS:** los miembros del grupo "Administradores de controlador de dominio de AAD" pueden administrar DNS para el dominio administrado mediante herramientas de administración de DNS conocidas, como el complemento MMC de Administración de DNS.
* **Crear a unidades organizativas (OU) personalizadas:** Los miembros del grupo "Administradores de controlador de dominio de AAD" pueden crear unidades organizativas personalizadas en el dominio administrado. A estos usuarios se les conceden privilegios administrativos completos sobre unidades organizativas personalizadas, por lo que pueden agregar o quitar cuentas de servicio, equipos, grupos, etc. dentro de estas unidades organizativas personalizadas.
* **Disponible en muchas regiones globales de Azure:** Consulte la página de [servicios de Azure por región](https://azure.microsoft.com/regions/#services/) para saber en qué regiones de Azure está disponible Azure AD Domain Services.
* **Alta disponibilidad:** Azure AD Domain Services proporciona alta disponibilidad para el dominio. Esta característica ofrece la garantía de mayor tiempo de actividad de servicio y resistencia a errores. La supervisión de estado integrada ofrece corrección automatiza de errores poniendo en marcha nuevas instancias para reemplazar instancias con errores y proporcionar un servicio continuado para el dominio.
* **Protección de bloqueo de cuentas de AD:** Las cuentas de usuario se bloquean durante 30 minutos si se usan cinco contraseñas no válidas en dos minutos. Las cuentas se desbloquean automáticamente pasados 30 minutos.
* **Usar herramientas de administración familiares:** puede utilizar herramientas de administración familiares de Windows Server Active Directory, como Centro de administración de Active Directory o Active Directory PowerShell, para administrar dominios administrados.
