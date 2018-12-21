---
title: ¿Qué es Azure AD Connect y Connect Health? | Microsoft Docs
description: Describe las herramientas utilizadas para sincronizar y supervisar el entorno local con Azure AD.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2fbd6edb02dfc4080d7692e43324a5b3981091f9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110406"
---
# <a name="what-is-azure-ad-connect"></a>¿Qué es Azure AD Connect?

Azure AD Connect es la herramienta de Microsoft diseñada para satisfacer y lograr sus objetivos de identidad híbrida.  Ofrece las siguientes características:
    
- [Sincronización de hash de contraseñas](whatis-phs.md): un método de inicio de sesión que sincroniza el hash de la contraseña de un usuario de AD local con Azure AD.
- [Autenticación de paso a través](how-to-connect-pta.md): un método de inicio de sesión que permite a los usuarios usar la misma contraseña de forma local y en la nube, pero que no requiere la infraestructura adicional de un entorno federado.
- [Integración de federación](how-to-connect-fed-whatis.md): la federación es una parte opcional de Azure AD Connect y puede utilizarse para configurar un entorno híbrido mediante una infraestructura local de AD FS. También proporciona funcionalidades de administración de AD FS, como la renovación de certificados e implementaciones de servidor de AD FS adicionales.
- [Sincronización](how-to-connect-sync-whatis.md): responsable de la creación de usuarios, grupos y otros objetos.  También de asegurar que la información de identidad de los usuarios y los grupos de su entorno local coincide con la de la nube.  Esta sincronización también incluye los códigos hash de contraseña.
-   [Seguimiento de estado](whatis-hybrid-identity-health.md): Azure AD Connect Health puede proporcionar una sólida supervisión y una ubicación central en Azure Portal donde se puede ver esta actividad. 


![Qué es Azure AD Connect](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>¿Qué es Azure AD Connect Health?

Azure Active Directory (Azure AD) Connect Health proporciona una sólida supervisión de la infraestructura de identidad local. Permite mantener una conexión confiable con Office 365 y Microsoft Online Services.  Esta confiabilidad se consigue al proporcionar funcionalidades de supervisión para los componentes de identidad clave. Además, hace que los puntos de datos clave sobre estos componentes sean fácilmente accesibles.

Esta información se encuentra en el [portal de Azure AD Connect Health](https://aka.ms/aadconnecthealth). Utilice el portal de Azure AD Connect Health para ver alertas, supervisión del rendimiento y análisis de uso, entre otra información. Azure AD Connect Health habilita la lente de mantenimiento única para los componentes de identidad clave en un lugar único.

![Qué es Azure AD Connect Health](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)

## <a name="why-use-azure-ad-connect"></a>¿Por qué usar Azure AD Connect?
la integración de directorios locales con Azure AD hace que los usuarios sean más productivos proporcionando una identidad común para tener acceso a recursos de nube y locales. Los usuarios y las organizaciones pueden aprovechar lo siguiente:

* Los usuarios pueden usar una única identidad para acceder a aplicaciones locales y servicios en la nube, como Office 365.
* Una sola herramienta que proporciona una experiencia de implementación simplificada para la sincronización y el inicio de sesión.
* Proporciona las funcionalidades más novedosas para sus escenarios de trabajo. Azure AD Connect sustituye a las versiones anteriores de las herramientas de integración de identidades, como DirSync y Sincronización de Azure AD. Para más información, consulte [Identidades híbridas: comparación de las herramientas para la integración de directorios de identidades híbridas](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Motivos para usar Azure AD Connect Health
Cuando se integran con Azure AD, los usuarios son más productivos porque hay una identidad común para acceder tanto a los recursos en la nube como a los locales. Asegurar que el entorno sea confiable, para que los usuarios puedan acceder a estos recursos, se convierte en un reto.  Azure AD Connect Health ayuda a supervisar y a conocer mejor su infraestructura de identidad local, lo que garantiza la confiabilidad de este entorno. Es tan sencillo como instalar un agente en cada uno de los servidores de identidad locales.


## <a name="next-steps"></a>Pasos siguientes

- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Configuración personalizada](how-to-connect-install-custom.md)
- [Instalación de agentes de Azure AD Connect Health](how-to-connect-health-agent-install.md) 
