---
title: ¿Qué es la sincronización de hash de contraseñas con Azure AD? | Microsoft Docs
description: Describe a sincronización de hash de contraseña.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 12/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a3e0c13312557503f4c2e1900f3c13eb45912345
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110469"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>¿Qué es la sincronización de hash de contraseñas con Azure AD?
La sincronización de hash de contraseñas es uno de los métodos de inicio de sesión utilizados para realizar la identidad híbrida. Azure AD Connect sincroniza un hash del hash de la contraseña de un usuario desde una instancia de Active Directory local con una instancia de Azure AD basada en la nube.

La sincronización de hash de contraseñas es una extensión de la característica de sincronización de directorios implementada por la sincronización de Azure AD Connect. Puede usar esta característica para iniciar sesión en servicios de Azure AD, como Office 365. Inicie sesión en el servicio con la misma contraseña que usa para iniciar sesión en la instancia local de Active Directory.

![Qué es Azure AD Connect](./media/how-to-connect-password-hash-synchronization/arch1.png)

La sincronización de hash de contraseñas le ayuda al reducir a una sola el número de contraseñas que los usuarios necesitan mantener. La sincronización de hash de contraseña puede:

* Mejorar la productividad de los usuarios.
* Reducir los costos de soporte técnico.  

Opcionalmente, puede configurar la sincronización de hash de contraseñas como respaldo si decide usar la [federación con Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) como método de inicio de sesión.

Para utilizar la sincronización de hash de contraseñas en su entorno, necesitará lo siguiente:

* Instalar Azure AD Connect.  
* Configure la sincronización de directorios entre su instancia de Active Directory local y la de Azure Active Directory.
* Habilite la sincronización de hash de contraseñas.



Para más información, consulte [¿Qué es la identidad híbrida?](whatis-hybrid-identity.md)




## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es la identidad híbrida?](whatis-phs.md)
- [¿Qué es Azure AD Connect y Connect Health?](whatis-azure-ad-connect.md)
- [¿Qué es la autenticación de paso a través (PTA)?](how-to-connect-pta.md)
- [¿Qué es la federación?](whatis-fed.md)
- [¿Qué es el inicio de sesión único?](how-to-connect-sso.md)
- [Funcionamiento de la sincronización de hash de contraseñas](how-to-connect-password-hash-synchronization.md)