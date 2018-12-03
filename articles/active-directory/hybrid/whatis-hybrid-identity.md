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
ms.date: 11/26/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: c8ab6b6e6bab7451de7d975dde644386fd4cb84e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311541"
---
# <a name="hybrid-identity-and-microsoft-identity-solutions"></a>Soluciones de identidad híbrida y de identidad de Microsoft
Las soluciones de identidad híbrida de [Microsoft Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) permiten sincronizar objetos de directorio locales con Azure AD mientras administra los usuarios locales. La primera decisión que hay que tomar cuando se planea sincronizar la instancia local de Windows Server Active Directory con Azure AD es si desea usar identidades administradas o una identidad federada. 

- **Identidades administradas**: cuentas de usuario y grupos que se sincronizan desde una instancia local de Active Directory; Azure administra la autenticación de usuarios.   
- Las **identidades federadas** permiten más control sobre los usuarios mediante la separación de la autenticación del usuario de Azure y la delegación de la autenticación en un proveedor de identidades local de confianza. 

Hay varias opciones disponibles para configurar la identidad híbrida. Mientras piensa en el modelo de identidad que mejor se adapte a las necesidades de su organización, debe pensar también en el tiempo, la infraestructura existente, la complejidad y el costo. Estos factores son diferentes para cada organización y pueden cambiar con el tiempo. Sin embargo, si cambian los requisitos, también cuenta con la flexibilidad para cambiar a otro modelo de identidad.

## <a name="managed-identity"></a>Identidad administrada 

La identidad administrada es la manera más sencilla de sincronizar los objetos de directorio locales (usuarios y grupos) con Azure AD. 

![Identidad híbrida sincronizada](./media/whatis-hybrid-identity/managed.png)

Aunque la identidad administrada es el método más fácil y rápido, los usuarios todavía tienen que mantener una contraseña independiente para los recursos basados en la nube. Para evitar esto, también puede (opcionalmente) [sincronizar un hash de contraseñas de usuario](how-to-connect-password-hash-synchronization.md) con su directorio Azure AD. La sincronización de hashes de contraseña permite a los usuarios iniciar sesión en recursos organizativos basados en la nube con el mismo nombre de usuario y contraseña que usan en local. Azure AD Connect busca periódicamente en el directorio local los cambios y mantiene sincronizado el directorio Azure AD. Cuando se cambia un atributo de usuario o una contraseña en Active Directory local, se actualiza automáticamente en Azure AD. 

Para la mayoría de las organizaciones que solo tienen que habilitar a sus usuarios para iniciar sesión en Office 365, aplicaciones SaaS y otros recursos basados en Azure AD, se recomienda la opción de sincronización de hash de contraseña predeterminada. Si esto no funciona, deberá decidir entre la autenticación de paso a través y AD FS.

> [!TIP]
> Las contraseñas de usuario se almacenan en Windows Server Active Directory local en forma de un valor hash que representa la contraseña real del usuario. Un valor hash es el resultado de una función matemática unidireccional (el algoritmo hash). No hay ningún método para volver del resultado de una función unidireccional a la versión de texto sin formato de una contraseña. El hash de contraseña no puede usarse para iniciar sesión en la red local. Si decide sincronizar hashes de contraseña, Azure AD Connect extrae estos de la instancia local de Active Directory y aplica un procesamiento de seguridad adicional al hash de contraseña antes de sincronizarlo con Azure AD. También se puede usar la sincronización de hash de contraseñas junto con la escritura diferida de contraseñas para habilitar el autoservicio de restablecimiento de contraseña en Azure AD. Además, puede habilitar el inicio de sesión único (SSO) para usuarios en equipos unidos a un dominio que están conectados a la red corporativa. Con el inicio de sesión único, los usuarios habilitados solo necesitan escribir un nombre de usuario para acceder a los recursos de nube de manera segura. 
>

## <a name="pass-through-authentication"></a>Autenticación de paso a través

La [autenticación de paso a través de Azure AD](how-to-connect-pta.md) proporciona una sencilla solución de validación de contraseñas para servicios basados en Azure AD mediante su instancia de Active Directory local. Si las directivas de seguridad y cumplimiento de su organización no permiten el envío de contraseñas de los usuarios, incluso en un formulario con algoritmo hash, y solo hay que admitir el SSO de escritorio para dispositivos unidos a un dominio, se recomienda que lo evalúe mediante la autenticación de paso a través. La autenticación de paso a través no requiere ninguna implementación en la red perimetral, lo que simplifica la infraestructura de implementación cuando se compara con AD FS. Cuando los usuarios inician sesión con Azure AD, este método de autenticación valida las contraseñas de los usuarios directamente en la instancia de Active Directory local.

![Autenticación de paso a través](./media/whatis-hybrid-identity/pass-through-authentication.png)

Con la autenticación de paso a través, no es necesario una infraestructura de red compleja ni tampoco almacenar contraseñas locales en la nube. Combinada con el inicio de sesión único, la autenticación de paso a través proporciona una experiencia realmente integrada cuando se inicie sesión en Azure AD o en otros servicios en la nube.

La autenticación de paso a través se configura mediante Azure AD Connect y utiliza un agente local simple que escucha las solicitudes de validación de contraseña. El agente se puede implementar fácilmente en varios equipos para proporcionar una alta disponibilidad y equilibrio de carga. Puesto que todas las comunicaciones son exclusivamente salientes, no es necesario que el conector se instale en una red perimetral. Los requisitos del equipo servidor para el conector son los siguientes:

- Windows Server 2012 R2 o superior
- Unión a un dominio en el bosque en el que se validan los usuarios

## <a name="federated-identity-ad-fs"></a>Identidad federada (AD FS)

Para tener más control sobre cómo los usuarios acceden a Office 365 y otros servicios en la nube, puede configurar la sincronización de directorios con el inicio de sesión único (SSO) mediante [Servicios de federación de Active Directory (AD FS)](how-to-connect-fed-whatis.md). La federación de los inicios de sesión del usuario con AD FS delega la autenticación a un servidor local que valida las credenciales del usuario. En este modelo, las credenciales de Active Directory local nunca se pasan a Azure AD.

![Identidad federada](./media/whatis-hybrid-identity/federated-identity.png)

También llamado federación de identidades, este método garantiza que toda la autenticación de usuario está controlada en local y permite a los administradores implementar niveles más rigurosos de control de acceso. La federación de identidades con AD FS es la opción más complicada y requiere la implementación de servidores adicionales en su entorno local. La federación de identidades también permite confirmar el soporte técnico ininterrumpido para su infraestructura de Active Directory y AD FS. Es necesario este elevado nivel de soporte técnico porque si el acceso de Internet local, el controlador de dominio o los servidores de AD FS no están disponibles, los usuarios no pueden iniciar sesión en servicios en la nube.

> [!TIP]
> Si opta por usar Federación con Servicios de federación de Active Directory (AD FS), tiene la posibilidad de configurar la sincronización de hash de contraseñas como copia de seguridad en caso de error en la infraestructura de AD FS.
>

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

## <a name="what-is-azure-ad-connect"></a>¿Qué es Azure AD Connect?

Azure AD Connect es la herramienta de Microsoft diseñada para satisfacer y lograr sus objetivos de identidad híbrida.  Esto le permite proporcionar una identidad común para los usuarios de aplicaciones de Office 365, Azure y SaaS integradas con Azure AD.  Ofrece las siguientes características:
    
- [Sincronización](how-to-connect-sync-whatis.md): este componente es responsable de la creación de usuarios, grupos y otros objetos. También es responsable de asegurarse de que la información de identidad de los usuarios y los grupos de su entorno local coincide con la de la nube.  Es responsable de sincronizar los hash de contraseña con Azure AD.
- [Sincronización de hash de contraseña](how-to-connect-password-hash-synchronization.md): un componente opcional que permite a los usuarios usar la misma contraseña de forma local y en la nube mediante la sincronización de un hash de contraseña de usuarios con Azure AD.
-   [AD FS y la integración de federación](how-to-connect-fed-whatis.md): la federación es una parte opcional de Azure AD Connect y puede utilizarse para configurar un entorno híbrido mediante una infraestructura local de AD FS. También proporciona funcionalidades de administración de AD FS como la renovación de certificados e implementaciones de servidor de AD FS adicionales.
-   [Autenticación de paso a través](how-to-connect-pta.md): otro componente opcional que permite a los usuarios usar la misma contraseña de forma local y en la nube, pero que no requiere la infraestructura adicional de un entorno federado.
-   [Integración de PingFederate y federación](how-to-connect-install-custom.md#configuring-federation-with-pingfederate): otra opción de federación que le permite usar PingFederate como proveedor de identidades.
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









