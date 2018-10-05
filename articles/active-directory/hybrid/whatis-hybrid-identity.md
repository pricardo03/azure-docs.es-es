---
title: Conexión de Active Directory con Azure Active Directory | Microsoft Docs
description: Azure AD Connect integrará sus directorios locales con Azure Active Directory. Esto le permite proporcionar una identidad común para las aplicaciones de Office 365, Azure y SaaS integradas con Azure AD.
keywords: introducción a Azure AD Connect, información general de Azure AD Connect, qué es Azure AD Connect, instalación de active directory
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 40ac3ca92c65607df056b883608dde60d816143e
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181793"
---
# <a name="hybrid-identity-and-microsofts-identity-solutions"></a>Soluciones de identidad híbrida y de identidad de Microsoft
Hoy en día, las empresas y las organizaciones se basan cada vez más en una combinación de aplicaciones locales y en la nube.  Tener aplicaciones y usuarios que requieren acceso a esas aplicaciones, locales y en la nube, se convierte en todo un desafío.

Las soluciones de identidad de Microsoft abarcan funcionalidades locales y de nube, de forma que se crea una sola identidad de usuario para la autenticación y la autorización en todos los recursos, sin importar su ubicación. A esto le llamamos identidad híbrida.

## <a name="what-is-azure-ad-connect"></a>¿Qué es Azure AD Connect?

Azure AD Connect es la herramienta de Microsoft diseñada para satisfacer y lograr sus objetivos de identidad híbrida.  Esto le permite proporcionar una identidad común para los usuarios de aplicaciones de Office 365, Azure y SaaS integradas con Azure AD.  Ofrece las siguientes características:
    
- [Sincronización](how-to-connect-sync-whatis.md): este componente es responsable de la creación de usuarios, grupos y otros objetos. También es responsable de asegurarse de que la información de identidad de los usuarios y los grupos de su entorno local coincide con la de la nube.  Es responsable de sincronizar los hash de contraseña con Azure AD.
-   [AD FS y la integración de federación](how-to-connect-fed-whatis.md): la federación es una parte opcional de Azure AD Connect y puede utilizarse para configurar un entorno híbrido mediante una infraestructura local de AD FS. También proporciona funcionalidades de administración de AD FS como la renovación de certificados e implementaciones de servidor de AD FS adicionales.
-   [Autenticación de paso a través](how-to-connect-pta.md): otro componente opcional que permite a los usuarios usar la misma contraseña de forma local y en la nube, pero que no requiere la infraestructura adicional de un entorno federado.
-   [Seguimiento de estado](whatis-hybrid-identity-health.md): Azure AD Connect Health puede proporcionar una sólida supervisión y una ubicación central en Azure Portal donde se puede ver esta actividad. 


![Qué es Azure AD Connect](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>¿Qué es Azure AD Connect Health?

Azure Active Directory (Azure AD) Connect Health le ayuda a supervisar y a conocer mejor su infraestructura de identidad local y los servicios de sincronización. Permite mantener una conexión confiable con Office 365 y Microsoft Online Services, ya que proporciona funcionalidades de supervisión para los principales componentes de identidad, como los servidores de Active Directory Federation Services (AD FS), los servidores de Azure AD Connect (también conocidos como motor de sincronización), los controladores de dominio de Active Directory, etc. También permite acceder con facilidad a los principales puntos de datos de estos componentes, con el fin de que pueda obtener datos de uso y otra información importante para tomar decisiones bien fundamentadas.

Esta información se encuentra en el [portal de Azure AD Connect Health](https://aka.ms/aadconnecthealth). En el portal de Azure AD Connect Health se pueden ver alertas, supervisión del rendimiento y análisis de uso, entre otros datos. Azure AD Connect Health habilita la lente de mantenimiento única para los componentes de identidad clave en un lugar único.

![Qué es Azure AD Connect Health](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)


A medida que aumentan las características de Azure AD Connect Health, el portal proporciona un único panel a través de la lente de identidad. Se obtiene un entorno aún más sólido, correcto e integrado para que los usuarios aumenten su capacidad de hacer las cosas.


## <a name="why-use-azure-ad-connect"></a>¿Por qué usar Azure AD Connect?
la integración de directorios locales con Azure AD hace que los usuarios sean más productivos proporcionando una identidad común para tener acceso a recursos de nube y locales. Los usuarios y las organizaciones pueden aprovechar lo siguiente:

* Los usuarios pueden usar una única identidad para acceder a aplicaciones locales y servicios en la nube, como Office 365.
* Una sola herramienta que proporciona una experiencia de implementación simplificada para la sincronización y el inicio de sesión.
* Proporciona las funcionalidades más novedosas para sus escenarios de trabajo. Azure AD Connect sustituye a las versiones anteriores de las herramientas de integración de identidades, como DirSync y Sincronización de Azure AD. Para más información, consulte [Identidades híbridas: comparación de las herramientas para la integración de directorios de identidades híbridas](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Motivos para usar Azure AD Connect Health
Cuando se integran los directorios locales con Azure AD, aumenta la productividad de los usuarios, ya que hay una identidad común para acceder tanto a los recursos en la nube como a los locales. Sin embargo, esta integración crea el desafío de garantizar que este entorno es correcto, con el fin de que los usuarios puedan acceder de manera confiable a los recursos tanto a nivel local como en la nube desde cualquier dispositivo. Azure AD Connect Health le ayuda a supervisar y obtener información sobre la infraestructura de identidades local, que se utiliza para acceder a Office 365 o a otras aplicaciones de Azure AD. Es tan sencillo como instalar un agente en cada uno de los servidores de identidad locales.

### <a name="azure-ad-connect-health-for-ad-fshow-to-connect-health-adfsmd"></a>[Azure AD Connect Health para AD FS](how-to-connect-health-adfs.md)
Azure AD Connect Health para AD FS es compatible con AD FS 2.0 en Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016. También permite supervisar el proxy de AD FS o los servidores proxy de la aplicación web que proporcionan compatibilidad con la autenticación para el acceso a la extranet. Con una instalación sencilla y rápida del agente de mantenimiento, Azure AD Connect Health para AD FS proporciona el siguiente conjunto de funcionalidades clave.

#### <a name="key-benefits-and-best-practices"></a>Principales ventajas y procedimientos recomendados

- *Mayor seguridad*
  - [Tendencias de bloqueo de extranet](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)
  - [Informe de inicios de sesión erróneos](how-to-connect-health-adfs.md#risky-ip-report-public-preview) 
  - [Cumple con los requisitos de privacidad](reference-connect-health-user-privacy.md)    
- *Reciba una alerta de todos los [problemas críticos del sistema de AD FS](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services)*
    - Disponibilidad y configuración del servidor 
    - [Rendimiento y conectividad](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs) 
  - Mantenimiento periódico    
- *Fácil de implementar y administrar*
  - Rápida [instalación del agente](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs) 
  - Actualización automática del agente a la versión más reciente 
  - Datos disponibles en el portal en cuestión de minutos    
- *[Métricas de uso](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs) enriquecidas* 
  - Uso de las aplicaciones principales
  - Ubicaciones de red y conexión TCP
  - Solicitudes de tokens por servidor    
- *Mejor experiencia del usuario* 
  - Modo de panel de Azure Portal
  - [Alertas a través de mensajes de correo electrónico](how-to-connect-health-adfs.md#alerts-for-ad-fs)    

#### <a name="feature-highlight"></a>Características resaltadas

*   Supervisión con alertas para saber si AD FS y los servidores proxy de AD FS funcionan correctamente
*   Notificaciones de correo electrónico para alertas críticas
*   Tendencias de los datos de rendimiento, que son útiles para planear la capacidad de AD FS
*   Análisis de uso de inicios de sesión de AD FS con elementos dinámicos (aplicaciones, usuarios, ubicación de red, etc.)
*   Informes para AD FS, como los primeros 50 usuarios con nombre de usuario o contraseña de inicio de sesión erróneos y su última dirección IP
*   Informe de direcciones IP de riesgo para inicios de sesión de AD FS con error

Aquí puede obtener información acerca del [Uso de Azure AD Connect Health con AD DS](how-to-connect-health-adfs.md)

### <a name="azure-ad-connect-health-for-synchow-to-connect-health-syncmd"></a>[Azure AD Connect Health para sincronización](how-to-connect-health-sync.md)
Azure AD Connect Health para sincronización supervisa y proporciona información acerca de las sincronizaciones que tienen lugar entre Active Directory local y Azure AD. Azure AD Connect Health para sincronización ofrece el siguiente conjunto de funcionalidades clave:

* Supervisión con alertas para saber en qué momento un servidor de Azure AD Connect, conocido también como motor de sincronización, no funciona correctamente
* Notificaciones de correo electrónico para alertas críticas
* Sincronización de la visión operativa, que incluye gráficos de latencia de las operaciones de sincronización y tendencias de distintas operaciones, como incorporaciones, actualizaciones y eliminaciones
* Información a primea vista de las propiedades de sincronización y de la última exportación correcta a Azure AD
* Los informes acerca de los errores de sincronización de nivel de objeto \(no requieren Azure AD Premium\)

Aquí puede obtener información acerca del [Supervisión de la sincronización de Azure AD Connect con Azure AD Connect Health](how-to-connect-health-sync.md)

### <a name="azure-ad-connect-health-for-ad-dshow-to-connect-health-addsmd"></a>[Azure AD Connect Health para AD DS](how-to-connect-health-adds.md)
Azure AD Connect Health para Active Directory Domain Services (AD DS) proporciona supervisión para los controladores de dominio instalados en Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016. La instalación de un agente de mantenimiento le permite supervisar el entorno de AD DS local desde la nube. Azure AD Connect Health para AD DS proporciona el siguiente conjunto de funcionalidades clave:

* Alertas de supervisión que detectan si los controladores de dominio no son correctos y las notificaciones de correo electrónico para alertas críticas
* El panel de controladores de dominio, que proporciona una vista rápida del mantenimiento y del estado operativo de los controladores de dominio
* El panel de estado de replicación que tiene la información de replicación más reciente y vínculos a guías de solución de problemas cuando se detectan errores
* Acceso rápido en cualquier lugar a los gráficos de datos de los contadores de rendimiento habituales, que son necesarios para la solución de problemas y la supervisión

Aquí puede obtener información acerca del [Uso de Azure AD Connect Health con AD DS](how-to-connect-health-adds.md)

## <a name="next-steps"></a>Pasos siguientes


- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Configuración personalizada](how-to-connect-install-custom.md)
- [Sincronización de hash de contraseñas](how-to-connect-password-hash-synchronization.md)|
- [Autenticación de paso a través](how-to-connect-pta.md)
- [Azure AD Connect y la federación](how-to-connect-fed-whatis.md)
- [Instalación de agentes de Azure AD Connect Health](how-to-connect-health-agent-install.md) 
- [Sincronización de Azure AD Connect](how-to-connect-sync-whatis.md)
- [Historial de versiones](reference-connect-version-history.md)
- [Comparación de las herramientas para la integración de directorios](plan-hybrid-identity-design-considerations-tools-comparison.md)
- [Preguntas más frecuentes sobre Azure AD Connect](reference-connect-faq.md)









