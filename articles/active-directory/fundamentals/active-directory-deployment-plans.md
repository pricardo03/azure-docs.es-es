---
title: Planes de implementación de Azure Active Directory | Microsoft Docs
description: Guía de un extremo a otro sobre cómo implementar muchas funcionalidades de Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.openlocfilehash: dc231fc17a864401873c2f5acefbac6aa4816215
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55102675"
---
# <a name="azure-active-directory-deployment-plans"></a>Planes de implementación de Azure Active Directory
¿Busca una guía de un extremo a otro sobre cómo implementar algunas de las funcionalidades de Azure Active Directory (Azure AD)? Los siguientes planes de implementación le guiarán por el valor de negocio, las consideraciones de planeamiento, el diseño y los procedimientos operativos necesarios para implementar correctamente algunas de las funcionalidades más comunes de Azure AD. 

En los documentos encontrará plantillas de correo electrónico, diagramas de arquitectura de sistemas, casos de prueba comunes y mucho más. 

Nos encantaría recibir sus comentarios sobre los documentos. Realice esta corta [encuesta](https://aka.ms/deploymentplanfeedback) acerca de cómo le han ayudado los documentos. 

|Escenario |DESCRIPCIÓN |
|-|-|
|[Inicio de sesión único](https://aka.ms/SSODPDownload)|El inicio de sesión único le ayuda a acceder a todas las aplicaciones y los recursos que necesita para hacer negocios, iniciando sesión una sola vez con una única cuenta de usuario. Una vez que ha iniciado sesión, puede ir de Microsoft Office a SalesForce o a Box sin tener que autenticarse (por ejemplo, escribiendo una contraseña) una segunda vez.|
|[Aprovisionamiento de usuarios de entrada basado en Workday](https://aka.ms/WorkdayDeploymentPlan)|El escenario "Aprovisionamiento de usuarios de entrada basado en Workday" para Active Directory establece las bases de un gobierno continuo de identidades y mejora la calidad de los procesos de negocio que se basan en datos de identidades fidedignos. Con esta característica, puede administrar fácilmente el ciclo de vida de las identidades de los empleados y los trabajadores temporales configurando reglas que asignen procesos de tipo alta, baja o traslado (como Nueva contratación, Fin de contrato y Traslado) a las acciones de aprovisionamiento de TI (como Crear, Habilitar, Deshabilitar y Eliminar cuentas).|
|[Panel de acceso](https://aka.ms/AccessPanelDPDownload)|Proporcione a sus usuarios un lugar centralizado y sencillo desde el que puedan acceder a todas las aplicaciones. Favorezca la productividad con funcionalidades de autoservicio, como la capacidad para solicitar acceso a nuevas aplicaciones y grupos o para administrar el acceso a estos recursos en nombre de otros.|
|[Aprovisionamiento de usuarios](https://aka.ms/UserProvisioningDPDownload)|Azure AD le ayuda a automatizar la creación, el mantenimiento y la eliminación de identidades de usuario en aplicaciones en la nube (SaaS) como Dropbox, Salesforce y muchas otras.|
|[Multi-Factor Authentication](https://aka.ms/MFADPDownload)|Azure Multi-Factor Authentication (MFA) es la solución de Microsoft de comprobación de dos pasos. El uso de métodos de autenticación aprobados por un administrador permite a Azure MFA ayudar a proteger el acceso a los datos y las aplicaciones, además de satisfacer la exigencia de un proceso de inicio de sesión simple.|
|[Acceso condicional](https://aka.ms/CADPDownload)|Con el acceso condicional, puede implementar decisiones de control de acceso automatizado sobre quién puede acceder a las aplicaciones en nube, en función de condiciones.|
|[Autenticación de ADFS para paso a través](https://aka.ms/ADFSTOPTADPDownload)|La autenticación de paso a través de Azure AD ayuda a los usuarios a iniciar sesión en aplicaciones basadas en la nube y locales con las mismas contraseñas. Esta característica proporciona a los usuarios una mejor experiencia (una contraseña menos que recordar) y reduce los costos del departamento de soporte técnico de TI dado que es menos probable que olviden cómo iniciar sesión. Cuando los usuarios inician sesión con Azure AD, esta característica valida sus contraseñas directamente con la instancia de Active Directory local.|
|[ADFS para sincronización de hash de contraseña](https://aka.ms/ADFSTOPHSDPDownload)|Con la sincronización de hash de contraseña, los valores de hash de las contraseñas de usuario se sincronizan desde Active Directory local a Azure AD, lo que permite a Azure AD autenticar a los usuarios sin interacción con Active Directory local|
|[SSO de conexión directa](https://aka.ms/SeamlessSSODPDownload)|El inicio de sesión único de conexión directa de Azure Active Directory (SSO de conexión directa de Azure AD) permite iniciar sesión automáticamente a los usuarios en dispositivos corporativos conectados a la red de la empresa. Después de habilitar esta característica, los usuarios no tienen que escribir la contraseña para iniciar sesión en Azure AD y, generalmente, ni siquiera los nombres de usuario. Esta característica proporciona a los usuarios un acceso sencillo a las aplicaciones en la nube sin necesidad de componentes locales adicionales.|
|[Restablecimiento de la contraseña de autoservicio](https://aka.ms/SSPRDPDownload)|El autoservicio de restablecimiento de contraseñas proporciona a los usuarios la capacidad de restablecer sus contraseñas, sin intervención de ningún administrador, en el momento y el lugar donde se precisa.|
|[Azure AD Application Proxy](https://aka.ms/AppProxyDPDownload)|Hoy en día, los empleados desean ser productivos en cualquier lugar, en cualquier momento y con cualquier dispositivo. Quieren trabajar en sus propios dispositivos, con independencia de que sean equipos portátiles, teléfonos o tabletas. Asimismo, esperan acceder a todas las aplicaciones, tanto las de SaaS en la nube como las de tipo empresarial en entornos locales. Para proporcionar acceso a las aplicaciones locales, siempre se han usado redes privadas virtuales (VPN) o redes perimetrales (DMZ). No obstante, estas soluciones no solo son complejas y difíciles de proteger, sino también costosas de configurar y administrar. Hay una forma mejor de realizar esto. - Azure AD Application Proxy|

