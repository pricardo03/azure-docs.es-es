---
title: 'En la nube de Azure AD rige la administración de cargas de trabajo local: Azure'
description: En este tema se describe la administración de nube rige para cargas de trabajo en el entorno local.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b7e54f6acfe1cbbe6e46fe92d132ebdaa91ff33
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742341"
---
# <a name="how-azure-ad-delivers-cloud-governed-management-for-on-premises-workloads"></a>Cómo ofrece Azure AD en la nube rige la administración de cargas de trabajo en el entorno local

Azure Active Directory (Azure AD) es una identidad completa como una solución de servicio (IDaaS) usada por millones de organizaciones que abarcan todos los aspectos de identidad, administración de acceso y seguridad. Azure AD contiene más de mil millones de usuario de identidades y ayuda a los usuarios iniciar sesión y obtener acceso seguro a ambos:

* Recursos externos, como Microsoft Office 365, Azure portal y miles de otras aplicaciones de Software-como-servicio (SaaS).
* Recursos internos, como las aplicaciones en la red corporativa y la intranet, junto con las aplicaciones en la nube desarrollado por la organización de la organización.

Las organizaciones pueden usar Azure AD si son 'en la nube pura' o como un "híbrido" implementación si tienen local las cargas de trabajo. Una implementación híbrida de Azure AD puede formar parte de una estrategia de una organización a migrar sus activos de TI a la nube o continuar la integración de infraestructura local existente junto con los nuevos servicios en la nube.

Históricamente, las organizaciones de "híbrido" han experimentado Azure AD como una extensión de sus actuales infraestructura local. En estas implementaciones, la administración de gobierno de identidades local, Windows Server Active Directory u otros sistemas internos de directorio, son los puntos de control y los usuarios y grupos se sincronizan desde dichos sistemas en un directorio en la nube como Azure AD. Una vez que esas identidades se encuentran en la nube, que pueden estar disponibles para Office 365, Azure y otras aplicaciones.

![Ciclo de vida de las identidades](media/cloud-governed-management-for-on-premises//image1.png)

Dado que las organizaciones más de su infraestructura de TI junto con sus aplicaciones a la nube, muchos están buscando la seguridad mejorada y capacidades de administración simplificada de administración de identidades como un servicio. Las características de IDaaS proporciona la nube de Azure AD aceleran la transición para la administración controlada en la nube proporcionando las soluciones y capacidades que permiten a las organizaciones adoptar rápidamente y mover más de la administración de identidades de locales tradicionales sistemas con Azure AD, mientras sigue siendo compatible con las aplicaciones existentes, así como nuevas.

Este documento describe la estrategia de Microsoft para la implementación híbrida IDaaS de cómo las organizaciones pueden usar Azure AD para sus aplicaciones existentes.

## <a name="the-azure-ad-approach-to-cloud-governed-identity-management"></a>El enfoque de Azure AD a la nube de administración de identidades regulado

Como la transición de las organizaciones a la nube, que necesitan garantías de que disponen de controles a través de su entorno completo - seguridad más y más visibilidad de las actividades, compatible con automatización e información proactiva. "**En la nube rige administración**" se describe cómo las organizaciones administran y controlan sus usuarios, aplicaciones, grupos y dispositivos desde la nube.

En este mundo moderno, las organizaciones necesitan poder administrar de forma eficaz a escala, debido a la proliferación de aplicaciones SaaS y el papel creciente de colaboración y las identidades externas. El nuevo panorama de riesgo de la nube significa que una organización debe tener más capacidad de respuesta: un actor malintencionado que pone en peligro un usuario en la nube podría afectar a las aplicaciones en la nube y locales.

En concreto, híbrido, las organizaciones necesitan poder delegar y automatizar las tareas, que históricamente IT hizo manualmente. Para automatizar las tareas, que necesitan las API y los procesos que organizar el ciclo de vida de los distintos recursos relacionados con la identidad (usuarios, grupos, aplicaciones, dispositivos), por lo que puede delegar la administración diaria de esos recursos a más personas fuera de principales de personal de TI. Azure AD aborda estos requisitos a través de la administración de cuentas de usuario y la autenticación nativa para los usuarios sin necesidad de infraestructura de identidad local. Creando la infraestructura local y no puede beneficiar a las organizaciones que tienen nuevas comunidades de usuarios, como socios comerciales, que no se origina en su directorio local, pero cuya administración de acceso es fundamental para conseguir los resultados empresariales.

Además, la administración no está completa sin gobierno---y gobierno en este nuevo mundo es una parte integrada del sistema de identidad en lugar de un complemento. Gobierno de identidades permite a las organizaciones la capacidad de administrar la identidad y acceso del ciclo de vida a través de los empleados, socios y proveedores y servicios y aplicaciones.

Incorporación de gobierno de identidades facilita la organización para realizar la transición a la nube de administración controlada, permite IT pudiese escalar, aborda nuevos desafíos con los invitados y proporciona más profunda insights y automatización que lo que los clientes tenían con infraestructura local. Sistema de gobierno en este nuevo mundo significa la capacidad de una organización tenga transparencia, visibilidad y los controles adecuados en el acceso a los recursos dentro de la organización. Con Azure AD, las operaciones de seguridad y auditoría equipos tienen acceso de visibilidad sobre quién tiene---y quién debe tener - a qué recursos en la organización (en qué dispositivos), lo que hacen los usuarios con los que el acceso y si la organización tiene y usos adecuados controles para quitar o restringir el acceso de acuerdo con la empresa o directivas de disposiciones legales.

El nuevo modelo de administración beneficia a las organizaciones con SaaS y aplicaciones de línea de negocio (LOB), ya que pueden más fácilmente administrar y proteger el acceso a esas aplicaciones. Mediante la integración de aplicaciones con Azure AD, las organizaciones podrán usar y administrar el acceso a través de la nube y locales originaron identidades de forma coherente. Administración del ciclo de vida de aplicaciones se convierte en automatiza aún más y Azure AD proporciona información detallada sobre el uso de aplicaciones que no puede realizar fácilmente en la administración de identidad local. A través de Azure AD, grupos de Office 365 y las características de autoservicio de los equipos, las organizaciones fácilmente pueden crear grupos para la colaboración y administración de acceso y agregar o quitar usuarios en la nube para permitir la colaboración y tener acceso a los requisitos de administración.

Selección de Azure AD de la derecha y la capacidad de nube gobierno de administración depende de las aplicaciones que se usará, cómo esas aplicaciones se integrará con Azure AD. Las siguientes secciones describen los enfoques que se realizará para las aplicaciones integradas en AD y aplicaciones que usan protocolos de federación (por ejemplo, SAML, OAuth u OpenID Connect).

## <a name="cloud-governed-management-for-ad-integrated-applications"></a>Administración regulada para las aplicaciones integradas en AD en la nube

Azure AD mejora la administración de local la organización aplicaciones integradas en Active Directory a través de acceso remoto seguro y el acceso condicional para esas aplicaciones. Además, Azure AD también proporciona administración del ciclo de vida de cuentas y administración de credenciales para cuentas de AD existentes del usuario, incluidos:

* **Proteger el acceso condicional para aplicaciones locales y acceso remoto**

Para muchas organizaciones, el primer paso para administrar el acceso desde la nube para aplicaciones basadas en escritorio remotas y de web de integradas en AD local es implementar la [proxy de aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) frente a esas aplicaciones para proporcionar seguros acceso remoto.

Después de un inicio de sesión único en Azure AD, los usuarios pueden acceder a las aplicaciones locales y en la nube mediante una dirección URL externa o un portal de aplicaciones interno. Por ejemplo, Application Proxy proporciona acceso remoto y single sign-on a Escritorio remoto, SharePoint, así como aplicaciones, como Tableau y Qlik y línea de negocio (LOB). Además, las directivas de acceso condicional pueden incluir mostrando el [términos de uso](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) y [ellos lo que garantiza el usuario ha aceptado](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou) antes de poder tener acceso a una aplicación.

![Arquitectura de Proxy de aplicación](media/cloud-governed-management-for-on-premises/image2.png)

* **Administración automática del ciclo de vida para las cuentas de Active Directory**

¿Gobierno de identidades permite a las organizaciones a lograr un equilibrio entre *productividad* ---rápidamente cómo una persona puede tener acceso a los recursos que necesitan, por ejemplo, cuando se unen a la organización? ¿---y *seguridad* ---cómo debe cambiar su acceso con el tiempo, por ejemplo, cuando cambia el estado de empleo de esta persona? La administración del ciclo de vida de las identidades es la piedra angular de la gobernanza de identidades y, para que esa gobernanza resulte eficaz a diferentes escalas, es preciso modernizar la infraestructura de administración del ciclo de vida de las identidades en las aplicaciones.

Para muchas organizaciones, ciclo de vida de identidad para los empleados está asociado a la representación del usuario en un sistema de gestión del capital humano (HCM). Para las organizaciones usar Workday como su sistema HCM, Azure AD puede asegurarse de las cuentas de usuario en AD [automáticamente aprovisionar y desaprovisionar para los trabajadores de Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial). Realizar por lo que lleva a la productividad del usuario mejorada mediante la automatización de birthright cuentas y controla los riesgos mediante la aplicación acceso se actualiza automáticamente cuando un usuario cambia los roles o abandona la organización. El aprovisionamiento de usuarios orientada a Workday [plan de implementación](https://aka.ms/WorkdayDeploymentPlan) es una guía paso a paso que muestra las organizaciones a través de la implementación de prácticas recomendada de Workday a la solución de aprovisionamiento de usuarios de Active Directory en un proceso de cinco pasos.

Azure AD Premium también incluye Microsoft Identity Manager, que puede importar registros de otros sistemas HCM en el entorno local, como SAP, Oracle eBusiness y Oracle PeopleSoft.

Cada vez más, colaboración de negocio a negocio requiere conceder acceso a los usuarios ajenos a su organización. [B2B de Azure AD](https://docs.microsoft.com/azure/active-directory/b2b/) colaboración permite a las organizaciones compartir de forma segura sus aplicaciones y servicios con los usuarios invitados y socios externos sin perder el control sobre sus propios datos corporativos.

Azure AD puede [crear automáticamente las cuentas de AD para los usuarios invitados](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises) según sea necesario, la habilitación de los invitados de empresa tener acceso a integrada en AD aplicaciones locales sin necesidad de otra contraseña. Las organizaciones pueden configurar [directivas de autenticación multifactor (MFA) para el usuario invitado](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)s, por lo que MFA comprueba que se realizan durante la autenticación de proxy de aplicación. Asimismo, cualquier [las revisiones de acceso](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews) que se realizan en la nube B2B a los usuarios se aplican a los usuarios locales. Por ejemplo, si se elimina el usuario en la nube a través de directivas de administración del ciclo de vida, también se elimina el usuario en el entorno local.

**Administración de cuentas de Active Directory de credenciales** Azure AD de autoservicio de restablecimiento de contraseña permite a los usuarios que han olvidado sus contraseñas para volver a autenticarse y restablecer sus contraseñas, con las contraseñas cambiadas [escritos en en Active Directory local](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback). El proceso de restablecimiento de contraseña también puede usar las directivas de contraseñas de Active Directory local: Cuando un usuario restablece su contraseña, se comprueba para asegurarse de que cumple la directiva de Active Directory local antes de enviarla a ese directorio. Restablece la contraseña de autoservicio [plan de implementación](https://aka.ms/deploymentplans/sspr) se describen los procedimientos recomendados para desplegar autoservicio restablecimiento de contraseña a los usuarios a través de web y experiencias de Windows integrada.

![Arquitectura de Azure AD SSPR](media/cloud-governed-management-for-on-premises/image3.png)

Por último, para las organizaciones que permiten a los usuarios cambiar sus contraseñas en Active Directory, AD puede configurarse para usar la misma directiva de contraseña como en Azure AD a través de la organización usa el [característica de protección de contraseña de Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), actualmente en versión preliminar pública.

Cuando esté lista para mover una aplicación integrada en AD para la nube moviendo el sistema operativo que hospeda la aplicación en Azure, una organización [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) proporciona servicios de dominio compatible con AD (por ejemplo, unión a un dominio, Directiva de grupo, LDAP y Kerberos/NTLM, autenticación). Azure AD Domain Services se integra con el inquilino de Azure AD existente de la organización, lo que permite a los usuarios iniciar sesión con sus credenciales corporativas. Además, los grupos existentes y las cuentas de usuario se pueden usar para proteger el acceso a los recursos, lo que garantiza un más suave 'mediante lift-and-shift"de los recursos locales a servicios de infraestructura de Azure.

![Azure AD Domain Services](media/cloud-governed-management-for-on-premises/image4.png)

## <a name="cloud-governed-management-for-on-premises-federation-based-applications"></a>Administración regulada para las aplicaciones basado en federación local en la nube

Para una organización que ya usa un proveedor de identidades local, mover aplicaciones a Azure AD permite un acceso más seguro y una experiencia administrativa más fácil para la administración de la federación. Azure AD permite configurar los controles de acceso granular de cada aplicación, incluido Azure Multi-factor Authentication, con acceso condicional de Azure AD. Azure AD es compatible con más funcionalidades, incluidos los certificados de firma de token de específicos de la aplicación y fechas de caducidad de certificado configurables. Estas capacidades, herramientas y orientación permiten a las organizaciones a retirar sus proveedores de identidades en el entorno local. De Microsoft IT, para obtener un ejemplo, se ha movido 17,987 aplicaciones de Microsoft interna Active Directory Federation Services (AD FS) a Azure AD.

![Evolución de Azure AD](media/cloud-governed-management-for-on-premises/image5.png)

Para comenzar a migrar las aplicaciones federadas con Azure AD como proveedor de identidades, consulte https://aka.ms/migrateapps que incluye vínculos a:

* Las notas del producto [migrar sus aplicaciones a Azure Active Directory](https://aka.ms/migrateapps/whitepaper), que presenta las ventajas de la migración y describe cómo planear la migración en cuatro fases que se describen con claridad: detección, clasificación, migración, y administración continua. Se le guiará a través de cómo piense en el proceso y dividir el proyecto en fragmentos fáciles de consumir. Este documento incluye vínculos a recursos importantes que le ayudarán a lo largo de este proceso.

* La Guía de solución [migrar de autenticación de aplicación de servicios de federación de Active Directory a Azure Active Directory](https://aka.ms/migrateapps/adfssolutionguide) explora con más detalle el mismo cuatro fases de planeamiento y ejecución de un proyecto de migración de aplicaciones . En esta guía, obtendrá información sobre cómo aplicar esas fases con el objetivo específico de mover una aplicación de servicios de federación de Active Directory (AD FS) a Azure AD.

* El [Active Directory Federation Services migración Readiness Script](https://aka.ms/migrateapps/adfstools) se puede ejecutar en servidores de servicios de federación de Active Directory (AD FS) en el entorno local existentes para determinar la preparación de aplicaciones para la migración a Azure AD.

## <a name="ongoing-access-management-across-cloud-and-on-premises-applications"></a>Administración del acceso continuo a través de las aplicaciones en la nube y locales

Las organizaciones necesitan un proceso para administrar el acceso que sea escalable. Los usuarios siguen se acumulan los derechos de acceso y acabar con más allá de lo que inicialmente se aprovisionó para ellos. Además, las organizaciones empresariales deben ser capaz de escalar de manera eficaz para desarrollar y aplicar la directiva de acceso y controles de forma continuada.

Normalmente, el departamento de TI delega las decisiones sobre la aprobación de los accesos en los responsables de la toma de decisiones de la empresa. Por otro lado, el departamento de TI puede involucrar él mismo a los usuarios. Por ejemplo, los usuarios que tienen acceso a los datos confidenciales de los clientes en una aplicación de marketing de una compañía en Europa tienen que conocer las directivas de la compañía. Los usuarios invitados también pueden no ser conscientes de los requisitos de control de datos de una organización a la que ha sido invitados.

Las organizaciones pueden automatizar el proceso de ciclo de vida de acceso a través de tecnologías como [grupos dinámicos](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership), junto con el aprovisionamiento de usuarios a [aplicaciones SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list), o [aplicaciones integrado con el sistema para Cross-Domain Identity Management (SCIM](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)) estándar. Las organizaciones también pueden controlar qué [los usuarios invitados tengan acceso a aplicaciones locales](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises). Estos derechos de acceso se pueden revisar periódicamente utilizando [revisiones de acceso de Azure AD](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) recurrentes.

## <a name="future-directions"></a>Caminos hacia el futuro

En entornos híbridos, la estrategia de Microsoft consiste en habilitar las implementaciones donde el **en la nube es el plano de control de identidad**y en directorios locales y otros sistemas de identidad, como Active Directory y otros de forma local las aplicaciones, son el destino para el aprovisionamiento de usuarios con acceso. Esta estrategia seguirá garantizando los derechos, identidades y acceso en esas aplicaciones y cargas de trabajo que se basan en ellos. En este estado final, las organizaciones podrán para impulsar la productividad del usuario final completamente desde la nube.

![Arquitectura de Azure AD](media/cloud-governed-management-for-on-premises/image6.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo empezar a trabajar en este viaje, consulte los planes de implementación de Azure AD, ubicados en <https://aka.ms/deploymentplans> . Proporcionan instrucciones de extremo a otro sobre cómo implementar capacidades de Azure Active Directory (Azure AD). Cada plan explica el valor de negocio, planeación de consideraciones de diseño y procedimientos operativos necesarios para implementar correctamente las funcionalidades comunes de Azure AD. Microsoft actualiza continuamente los planes de implementación con los procedimientos recomendados aprendidos a las implementaciones de clientes y otros comentarios cuando se agregan nuevas capacidades para administración de la nube con Azure AD.
