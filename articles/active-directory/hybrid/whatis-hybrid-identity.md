---
title: Conexión de Active Directory con Azure Active Directory | Microsoft Docs
description: Azure AD Connect integrará sus directorios locales con Azure Active Directory. Esto le permite proporcionar una identidad común para las aplicaciones de Office 365, Azure y SaaS integradas con Azure AD.
keywords: introducción a Azure AD Connect, información general de Azure AD Connect, qué es Azure AD Connect, instalación de active directory
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 11/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 0b6bb3f950c4a442e898a6251a89246b31af78bb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55160430"
---
# <a name="what-is-hybrid-identity"></a>¿Qué es la identidad híbrida? 

Hoy en día, las empresas y las organizaciones se basan cada vez más en una combinación de aplicaciones locales y en la nube.  Los usuarios requieren acceso a esas aplicaciones tanto en el entorno local como en la nube. Este requisito se ha convertido en un escenario desafiante. 

Las soluciones de identidad de Microsoft abarcan funcionalidades locales y basadas en la nube.  Estas soluciones crean una identidad de usuario común para la autenticación y autorización en todos los recursos, independientemente de la ubicación. A esto lo llamamos **identidad híbrida**.

Para lograr la identidad híbrida, se puede usar uno de tres métodos de autenticación, en función de los escenarios.   Los tres métodos son: 

- **[Sincronización de hash de contraseña (PHS)](whatis-phs.md)**  
- **[Autenticación de paso a través (PTA)](how-to-connect-pta.md)**  
- **[Federación](whatis-fed.md)** 

Estos métodos de autenticación también proporcionan funcionales de [inicio de sesión único](how-to-connect-sso.md).  El inicio de sesión único permite que los usuarios inicien sesión automáticamente cuando usan sus dispositivos corporativos y están conectados a la red de la empresa.

Para información adicional, consulte [Selección del método de autenticación adecuado para la solución de identidad híbrida de Azure Active Directory](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). 

## <a name="common-scenarios-and-recommendations"></a>Escenarios comunes y recomendaciones 

Estos son algunos escenarios comunes de identidad híbrida y administración de acceso con recomendaciones sobre la opción (u opciones) de identidad que podría ser más adecuada. 

|Necesitará:|PHS y SSO<sup>1</sup>| PTA y SSO<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|Sincronice nuevas cuentas de usuarios, contactos y grupos creadas en Active Directory local con la nube automáticamente.|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Configurar mi inquilino para escenarios híbridos de Office 365|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Permitir que mis usuarios inicien sesión en Cloud Services y accedan con su contraseña local|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Implementar el inicio de sesión único utilizando credenciales corporativas|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| ![Recomendado](./media/whatis-hybrid-identity/ic195031.png) |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|  
|Asegurarse de que ningún hash de contraseña está almacenado en la nube| |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Habilitar soluciones de autenticación multifactor en la nube| |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)|![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Habilitar soluciones locales de autenticación multifactor| | |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Admitir la autenticación de tarjeta inteligente para mis usuarios<sup>4</sup>| | |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 
|Mostrar notificaciones de expiración de contraseña en el portal de Office y en el escritorio de Windows 10| | |![Recomendado](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Sincronización de hash de contraseñas con inicio de sesión único. 
> 
> <sup>2</sup> Autenticación de paso a través e inicio de sesión único.  
> 
> <sup>3</sup> Inicio de sesión único federado con AD FS.  
>  
> <sup>4</sup> AD FS se puede integrar con el PKI de la empresa para permitir el inicio de sesión con certificados. Estos certificados pueden ser certificados flexibles implementados a través de canales de aprovisionamiento de confianza, como certificados MDM o GPO o de tarjetas inteligentes (incluidas las tarjetas PIV/CAC) o Hello para empresas (cert-trust). Para más información sobre la compatibilidad con la autenticación de tarjetas inteligentes, consulte [este blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/). 
> 

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es Azure AD Connect y Connect Health?](whatis-azure-ad-connect.md) 
- [¿Qué es la sincronización de hash de contraseñas (PHS)?](whatis-phs.md) 
- [¿Qué es la autenticación de paso a través (PTA)?](how-to-connect-pta.md) 
- [¿Qué es la federación?](whatis-fed.md) 
- [¿Qué es el inicio de sesión único?](how-to-connect-sso.md) 

