---
title: Planes de implementación de Azure Active Directory | Microsoft Docs
description: Guía de un extremo a otro sobre cómo implementar muchas funcionalidades de Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f172151b3b385cf936f7335048e172fa32ffa2ac
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562007"
---
# <a name="azure-active-directory-deployment-plans"></a>Planes de implementación de Azure Active Directory
¿Busca una guía de un extremo a otro sobre cómo implementar algunas de las funcionalidades de Azure Active Directory (Azure AD)? Los siguientes planes de implementación le guiarán por el valor de negocio, las consideraciones de planeamiento, el diseño y los procedimientos operativos necesarios para implementar correctamente algunas de las funcionalidades más comunes de Azure AD. 

En los documentos encontrará plantillas de correo electrónico, diagramas de arquitectura de sistemas, casos de prueba comunes y mucho más. 

Nos encantaría recibir sus comentarios sobre los documentos. Realice esta corta [encuesta](https://aka.ms/deploymentplanfeedback) acerca de cómo le han ayudado los documentos. 

## <a name="include-the-right-stakeholders"></a>Inclusión de la parte interesada correcta

Cuando comience a planear la implementación para una nueva funcionalidad, es importante incluir a las partes interesadas que son clave en su organización. Es recomendable identificar y documentar a la persona o personas que cumplen cada uno de los siguientes roles y trabajar con ellas para determinar su participación en el proyecto.  

Entre los roles, se incluyen los siguientes: 

|Role |DESCRIPCIÓN |
|-|-|
|Usuario final|Un grupo representativo de usuarios para el que se implementará la funcionalidad. A menudo previsualiza los cambios en un programa piloto.
|Administrador de soporte técnico de TI|Un representante de la organización de soporte técnico de TI que puede proporcionar información sobre la compatibilidad de este cambio desde una perspectiva del departamento de soporte técnico.  
|Arquitecto de identidades o administrador global de Azure|Un representante del equipo de administración de identidades responsable de definir cómo este cambio se alinea con la infraestructura de administración de identidades principal de su organización.|
|Propietario empresarial de la aplicación |El propietario empresarial global de las aplicaciones afectadas, que pueden incluir la administración del acceso.  También puede proporcionar información sobre la experiencia del usuario y la utilidad de este cambio desde la perspectiva del usuario final.
|Propietario de seguridad|Un representante del equipo de seguridad que puede aprobar que el plan cumplirá los requisitos de seguridad de la organización.|
|Administrador de cumplimiento|La persona de la organización responsable de garantizar el cumplimiento con requisitos corporativos, del sector o gubernamentales.|

**Los niveles de implicación podrían incluir:**

- **R**esponsible para implementar el plan del proyecto y el resultado 

- **A**probación del plan del proyecto y el resultado 

- **C**olaborador con el plan del proyecto y el resultado 

- **I**nformado del plan del proyecto y el resultado
 
## <a name="deployment-plans"></a>Planes de implementación



|Escenario |DESCRIPCIÓN |
|-|-|
|[Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)|Azure Multi-Factor Authentication (MFA) es la solución de Microsoft de comprobación de dos pasos. El uso de métodos de autenticación aprobados por un administrador permite a Azure MFA ayudar a proteger el acceso a los datos y las aplicaciones, además de satisfacer la exigencia de un proceso de inicio de sesión simple.|
|[Acceso condicional](https://aka.ms/deploymentplans/ca)|Con Acceso condicional, puede implementar decisiones de control de acceso automatizado sobre quién puede acceder a las aplicaciones en nube, en función de condiciones.|
|[Restablecimiento de la contraseña de autoservicio](https://aka.ms/deploymentplans/sspr)|El autoservicio de restablecimiento de contraseñas proporciona a los usuarios la capacidad de restablecer sus contraseñas, sin intervención de ningún administrador, en el momento y el lugar donde se precisa.|
|[Privileged Identity Management](https://aka.ms/deploymentplans/pim)|Azure AD Privileged Identity Management (PIM) le permite administrar roles con privilegios administrativos en Azure AD, los recursos de Azure y otros servicios de Microsoft Online Services. Asimismo, PIM ofrece soluciones como acceso de tipo Just-In-Time, flujos de trabajo de aprobación de solicitudes y revisiones de acceso completamente integradas para que pueda identificar, detectar y evitar en tiempo real las actividades malintencionadas de roles con privilegios.|
|[Inicio de sesión único](https://aka.ms/deploymentplans/sso)|El inicio de sesión único le ayuda a acceder a todas las aplicaciones y los recursos que necesita para hacer negocios, iniciando sesión una sola vez con una única cuenta de usuario. Una vez que ha iniciado sesión, puede ir de Microsoft Office a SalesForce o a Box sin tener que autenticarse (por ejemplo, escribiendo una contraseña) una segunda vez.|
|[SSO de conexión directa](https://aka.ms/SeamlessSSODPDownload)|El inicio de sesión único de conexión directa de Azure Active Directory (SSO de conexión directa de Azure AD) permite iniciar sesión automáticamente a los usuarios en dispositivos corporativos conectados a la red de la empresa. Después de habilitar esta característica, los usuarios no tienen que escribir la contraseña para iniciar sesión en Azure AD y, generalmente, ni siquiera los nombres de usuario. Esta característica proporciona a los usuarios un acceso sencillo a las aplicaciones en la nube sin necesidad de componentes locales adicionales.|
|[Panel de acceso](https://aka.ms/AccessPanelDPDownload)|Proporcione a sus usuarios un lugar centralizado y sencillo desde el que puedan acceder a todas las aplicaciones. Favorezca la productividad con funcionalidades de autoservicio, como la capacidad para solicitar acceso a nuevas aplicaciones y grupos o para administrar el acceso a estos recursos en nombre de otros.|
|[ADFS para sincronización de hash de contraseña](https://aka.ms/deploymentplans/adfs2phs)|Con la sincronización de hash de contraseña, los valores de hash de las contraseñas de usuario se sincronizan desde Active Directory local a Azure AD, lo que permite a Azure AD autenticar a los usuarios sin interacción con Active Directory local|
|[Autenticación de ADFS para paso a través](https://aka.ms/deploymentplans/adfs2pta)|La autenticación de paso a través de Azure AD ayuda a los usuarios a iniciar sesión en aplicaciones basadas en la nube y locales con las mismas contraseñas. Esta característica proporciona a los usuarios una mejor experiencia (una contraseña menos que recordar) y reduce los costos del departamento de soporte técnico de TI dado que es menos probable que olviden cómo iniciar sesión. Cuando los usuarios inician sesión con Azure AD, esta característica valida sus contraseñas directamente con la instancia de Active Directory local.|
|[Azure AD Application Proxy](https://aka.ms/deploymentplans/appproxy)|Hoy en día, los empleados desean ser productivos en cualquier lugar, en cualquier momento y con cualquier dispositivo. Quieren trabajar en sus propios dispositivos, con independencia de que sean equipos portátiles, teléfonos o tabletas. Asimismo, esperan acceder a todas las aplicaciones, tanto las de SaaS en la nube como las de tipo empresarial en entornos locales. Para proporcionar acceso a las aplicaciones locales, siempre se han usado redes privadas virtuales (VPN) o redes perimetrales (DMZ). No obstante, estas soluciones no solo son complejas y difíciles de proteger, sino también costosas de configurar y administrar. Hay una forma mejor de realizar esto. - Azure AD Application Proxy|
|[Aprovisionamiento de usuarios](https://aka.ms/UserProvisioningDPDownload)|Azure AD le ayuda a automatizar la creación, el mantenimiento y la eliminación de identidades de usuario en aplicaciones en la nube (SaaS) como Dropbox, Salesforce y muchas otras.|
|[Aprovisionamiento de usuarios de entrada basado en Workday](https://aka.ms/WorkdayDeploymentPlan)|El escenario "Aprovisionamiento de usuarios de entrada basado en Workday" para Active Directory establece las bases de una gobernanza continua de identidades y mejora la calidad de los procesos de negocio que se basan en datos de identidades fidedignos. Con esta característica, puede administrar fácilmente el ciclo de vida de las identidades de los empleados y los trabajadores temporales configurando reglas que asignen procesos de tipo alta, baja o traslado (como Nueva contratación, Fin de contrato y Traslado) a las acciones de aprovisionamiento de TI (como Crear, Habilitar, Deshabilitar y Eliminar cuentas).|
