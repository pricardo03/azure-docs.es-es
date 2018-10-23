---
title: ¿Qué es Azure Active Directory (Azure AD)? | Microsoft Docs
description: Obtenga información sobre cómo usar Azure Active Directory para ampliar sus identidades locales existentes en la nube o desarrollar aplicaciones integradas de Azure AD.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
ms.date: 09/13/2018
ms.custom: it-pro
ms.openlocfilehash: 08d32a99e2f1a77bcc7a5900b47e6771c33ec356
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393328"
---
# <a name="what-is-azure-active-directory"></a>¿Qué es Azure Active Directory?
Azure Active Directory (Azure AD) es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Azure AD combina servicios de directorio fundamentales, administración de acceso de aplicaciones y protección de identidad en una única solución, ofrece una plataforma basada en estándares que permite a los desarrolladores ofrecer control de acceso a sus aplicaciones, según directivas y reglas centralizadas.

![Pila de Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="benefits-of-azure-ad"></a>Ventajas de Azure AD
Azure AD le ayuda a:

-   Crear y administrar una identidad única para cada usuario en toda la empresa, lo que mantiene a los usuarios, grupos y dispositivos sincronizados con [Azure AD Connect](../connect/active-directory-aadconnect.md).

-   Proporcionar acceso de inicio de sesión único a las aplicaciones, incluidas miles de aplicaciones SaaS integradas previamente, y proporcionar acceso remoto seguro a aplicaciones SaaS locales mediante [Azure AD Application Proxy](../manage-apps/application-proxy.md).

-   Habilitar la seguridad del acceso de las aplicaciones mediante la aplicación de directivas de [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) basadas en reglas para las aplicaciones locales y en la nube.

-   Mejorar la productividad del usuario con el [autoservicio de restablecimiento de contraseña](../user-help/user-help-reset-password.md) y las solicitudes de acceso de grupos y aplicaciones mediante el [portal MyApps](../user-help/active-directory-saas-access-panel-introduction.md).

-   Sacar provecho de la [alta disponibilidad y confiabilidad](https://docs.microsoft.com/azure/architecture/checklist/availability) de una solución a nivel mundial de administración de identidades y acceso basada en la nube de categoría empresarial.

## <a name="who-uses-azure-ad"></a>Quién usa Azure AD
Azure AD está pensado para administradores de TI, desarrolladores de aplicaciones y usuarios de Office 365, Azure o Dynamics CRM Online.

- **Administradores de TI.** Azure AD proporciona una solución más segura para su organización mediante el uso de una administración de identidades y un acceso de inicio de sesión único (SSO) más seguros a miles de [aplicaciones de SaaS en la nube](../saas-apps/tutorial-list.md) y de aplicaciones locales. Mediante estas aplicaciones también obtendrá seguridad de las aplicaciones en la nube, acceso ininterrumpido, colaboración mejorada y automatización del ciclo de vida de la identidad de los usuarios, lo que ayuda a mejorar tanto la seguridad como el cumplimiento de normas.

    Además, con [Azure AD Connect](../connect/active-directory-aadconnect-get-started-express.md), puede integrar Azure AD en una instancia existente de Windows Server Active Directory, lo que permite a su organización usar las inversiones en identidad locales existentes para administrar el acceso a aplicaciones SaaS basadas en la nube.

- **Para desarrolladores de aplicaciones.** Azure AD le ayuda a centrarse en la creación de aplicaciones, ya que proporciona integración en una solución de administración de identidades que usan millones de organizaciones de todo el mundo.

- **Para clientes de Office 365, Azure o Dynamics CRM Online.** Ya usa Azure AD. Todos los inquilinos de Office 365, Azure y Dynamics CRM Online son inquilinos de Azure AD, lo que le permite empezar a administrar de inmediato el acceso de los usuarios a las aplicaciones en la nube integradas.

## <a name="how-reliable-is-azure-ad"></a>¿Cuál es el grado de confiabilidad Azure AD?
El diseño multiempresa, distribuido geográficamente y de alta disponibilidad de Azure AD significa que puede confiar en él para sus necesidades empresariales más críticas. Azure AD se ejecuta en 28 centros de datos en todo el mundo con conmutación por error automatizada. Esto significa que incluso si un centro de datos deja de funcionar, las copias de los datos del directorio estarán disponibles en al menos dos centros de datos más dispersos por regiones para ofrecer un acceso instantáneo.

Para más información acerca de los contratos de nivel de servicio, consulte [Contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Elección de una edición
Todos los servicios de negocios de Microsoft Online dependen de Azure AD para el inicio de sesión y otras necesidades de identidad. Si se suscribe a alguno de los servicios de negocios de Microsoft Online (por ejemplo, Office 365 o Microsoft Azure), recibirá Azure AD automáticamente con acceso a todas las características gratuitas. Con la edición Azure Active Directory Free, puede administrar usuarios y grupos, sincronizar directorios locales, obtener inicio de sesión único entre Azure, Office 365 y miles de aplicaciones SaaS conocidas como Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox, etc. 

Para mejorar la implementación de Azure AD, también puede agregar funcionalidades de pago si actualiza a las ediciones Azure Active Directory Basic, Premium P1 o Premium P2. Las ediciones de pago de Azure AD se crean encima del directorio gratuito existente, y proporcionan funcionalidades de tipo empresarial que abarcan autoservicio, supervisión mejorada, informes de seguridad, Multi-Factor Authentication (MFA) y un acceso seguro para sus trabajadores móviles.

> [!NOTE]
> Para ver las opciones de precios de estas ediciones, consulte [Precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 y Azure Active Directory Basic no se admiten actualmente en China. Para más información acerca de los precios de Azure AD, puede ponerse en contacto con el foro de Azure Active Directory.

- **Azure Active Directory Basic.** Diseñada para trabajadores con necesidades básicas relacionadas con la nube, esta edición proporciona acceso a las aplicaciones centrado en la nube, así como soluciones de administración de identidades de autoservicio. Con la edición Basic, obtiene características de mejora de la productividad y reducción de costos, como administración de acceso basado en grupo, autoservicio de restablecimiento de contraseña para aplicaciones en la nube y Azure Active Directory Application Proxy (para publicar aplicaciones web locales con Azure AD), y todas ellas respaldadas por un Acuerdo de Nivel de Servicio empresarial con un tiempo de actividad del 99,9 %.

- **Azure Active Directory Premium P1.** Diseñada para atender las necesidades de las organizaciones con mayores exigencias de administración de acceso e identidades, la edición Azure Active Directory Premium aporta funciones de administración de identidades de nivel empresarial con todas las características que permite a los usuarios híbridos acceder sin problemas a las funcionalidades locales y basadas en la nube. Incluye todo lo que necesita para el trabajador de información y los administradores de identidades en entornos híbridos con relación al acceso a aplicaciones, identidad de autoservicio y administración de acceso (IAM), la protección de identidades y la seguridad en la nube. Admite recursos avanzados de administración y delegación, como grupos dinámicos y administración de grupos de autoservicio. Incluye Microsoft Identity Manager (un conjunto de aplicaciones de administración de identidades y acceso locales) y proporciona funcionalidades de escritura diferida en la nube que permiten soluciones como el autoservicio de restablecimiento de contraseña para los usuarios locales.

- **Azure Active Directory Premium P2.** Esta nueva oferta se ha diseñado con protección avanzada para todos los usuarios y administradores, e incluye todas las funcionalidades de Azure AD Premium P1, además de Identity Protection y Privileged Identity Management. Azure Active Directory Identity Protection aprovecha miles de millones de señales para proporcionar acceso condicional basado en el riesgo a las aplicaciones y los datos críticos de una empresa. También le ayudamos a administrar y proteger cuentas con privilegios con Azure Active Directory Privileged Identity Management, con el fin de que pueda detectar, restringir y supervisar no solo a los administradores sino también su acceso a los recursos, así como proporcionar acceso Just-In-Time cuando sea necesario.  

> [!NOTE]
> Hay varias funcionalidades de Azure Active Directory que también están disponibles mediante las ediciones de "pago por uso":<ul><li>**Azure Active Directory B2C.** Solución de administración de identidades y acceso para las aplicaciones orientados al consumidor. Para más información, vea [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).</li><li>**Azure Multi-Factor Authentication.** Lo usan proveedores por usuario o por autenticación. Para más información, vea [¿Qué es Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="as-an-admin-how-do-i-get-started"></a>Como administrador, ¿cómo puedo empezar?
Regístrese para obtener una evaluación gratuita de 30 días e implemente la primera solución en la nube; vea [prueba de Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="as-a-developer-how-do-i-get-started"></a>Como desarrollador, ¿cómo puedo empezar?
Regístrese para obtener una evaluación gratuita de 30 días y empiece a integrar las aplicaciones en Azure AD; vea [prueba de Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/). Para más información, vea la [guía para desarrolladores](../develop/azure-ad-developers-guide.md) de Azure Active Directory.

## <a name="next-steps"></a>Pasos siguientes
- [Más información sobre los conceptos básicos de la administración de identidades y accesos de Azure](identity-fundamentals.md)

- [Integración de Azure AD con Windows Server Active Directory](../hybrid/how-to-connect-install-express.md)
