---
title: 'Kit de adopción del acceso condicional: Azure Active Directory'
description: Adopción del acceso condicional de Azure AD para acceder a los recursos
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: c75ec0ec212c99e474d13892178acbdc14e5f386
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71075287"
---
#  <a name="adopting-azure-ad-conditional-access"></a>Adopción del acceso condicional de Azure AD

En un mundo que da prioridad a los dispositivos móviles y la nube, los usuarios pueden acceder a los recursos de su organización mediante diversos dispositivos y aplicaciones desde cualquier parte. Como resultado, ya no es suficiente con centrarse en quién puede acceder a un recurso. Puede controlar quién tiene acceso, identificar dónde está el usuario y qué dispositivo se está usando y mucho más.

Para proporcionar este control, el **acceso condicional de Azure Active Directory (AD)** le permite especificar las condiciones que deben cumplir los usuarios para acceder a una aplicación, como la autenticación multifactor (MFA). Con las directivas de acceso condicional se controla cómo los usuarios autorizados (los usuarios a los que se les ha concedido acceso a una aplicación en la nube) pueden acceder a una aplicación en la nube con condiciones específicas. Para más información, consulte [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)

## <a name="key-benefits"></a>Ventajas principales

Las principales ventajas de usar el acceso condicional de Azure AD son:

* **Aumento de la productividad:** Las directivas de acceso condicional (CA) permiten centrarse en el punto en el que los usuarios deberán usar MFA, tendrán bloqueado el acceso o deberán usar un dispositivo de confianza. Por ejemplo, puede establecer directivas como requerir que los usuarios deban utilizar MFA para iniciar sesión en una aplicación solo cuando están fuera de la red corporativa. La reducción de las solicitudes de MFA hace que los usuarios sean más productivos que si tienen que utilizar MFA cada vez que inician sesión. Además, el acceso condicional de Azure AD permite especificar directivas en función del usuario y también crear directivas específicas de la aplicación.
* **Administración del riesgo:** Habilitar directivas de acceso condicional proporciona protección de la identidad en la nube, funcionalidades de control de acceso basadas en el riesgo y compatibilidad con la autenticación multifactor nativa. El acoplamiento del acceso condicional con la protección de la identidad permite definir cuándo se bloquea o admite el acceso a una aplicación.
* **Solución para el cumplimiento y la gobernanza:** La auditoría de las solicitudes de acceso y las aprobaciones de la aplicación y la comprensión de uso general de la aplicación son más sencillos con Azure AD porque admite los registros de auditoría nativos para cada solicitud de acceso a la aplicación que se realiza. Los datos de auditoría incluyen la identidad del solicitante, la fecha de solicitud, la justificación del negocio, el estado de aprobación y la identidad del aprobador. Estos datos también están disponibles desde una API, lo que permitirá la importación de estos datos en el sistema de Administración de eventos e información de seguridad (SIEM) de su elección.
* **Administración del costo:** Al mover las directivas de acceso a Azure AD se reduce la dependencia en soluciones personalizadas o soluciones locales, como Servicios de federación de Active Directory (AD FS) para el acceso condicional, lo que reduce el costo de ejecución de dicha infraestructura.

## <a name="customer-case-studies"></a>Casos prácticos de clientes

Descubra cómo utilizan la mayoría de las organizaciones el acceso condicional de Azure AD para definir e implementar decisiones de control de acceso automatizado para acceder a las aplicaciones en la nube en función de condiciones. Las historias destacadas siguientes muestran cómo se cumplen estas necesidades de los clientes.

* [**Wipro** impulsa la productividad móvil con las herramientas de seguridad en la nube de Microsoft para mejorar las relaciones con el cliente.](https://customers.microsoft.com/story/wipro-professional-services-enterprise-mobility-security) Las directivas de acceso condicional de Azure AD permiten a la empresa compartir documentos, recursos y aplicaciones con entidades de confianza externas, que pueden usar sus propias credenciales, sin perder el control sobre sus propios datos corporativos.
* [**Accenture** protege su traslado a la nube con Microsoft Cloud App Security](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security). Accenture está evaluando la característica [Control de aplicaciones de acceso condicional](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad) de Cloud App Security, que usa el acceso condicional de Azure Active Directory Active, para controlar el acceso a las aplicaciones según determinadas condiciones. LePenske indica que esta característica puede resultar útil, por ejemplo, para habilitar el acceso de solo lectura a los archivos mientras que al tiempo se prohíben las descargas.
* [**Aramex**, empresa internacional de transporte y logística, crea oficinas conectadas a la nube con la solución de administración de identidades y acceso](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en). Garantizar el acceso seguro era especialmente difícil con los empleados remotos de Aramex. La compañía ahora aplica el acceso condicional para permitir que estos empleados remotos puedan acceder a las aplicaciones SaaS desde fuera de la red. La regla de acceso condicional decidirá si se debe aplicar la autenticación multifactor, lo que proporciona el acceso correcto solo a las personas adecuadas.

Para más información acerca de las experiencias de clientes y asociados con el acceso condicional de Azure AD, consulte [Vea las cosas increíbles que están haciendo otros con Azure](https://azure.microsoft.com/case-studies/?service=active-directory).

## <a name="announcements"></a>Anuncios

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día con los avances más recientes, consulte [¿Cuáles son las novedades de Azure Active Directory?](../fundamentals/whats-new.md).

Blogs recientes de la comunidad tecnológica y la División de identidad de Microsoft:

* 24 de septiembre de 2018, [Acceso condicional de Azure Active Directory en Azure Databricks](https://azure.microsoft.com/updates/azure-active-directory-conditional-access-in-azure-databricks/)
* 21 de septiembre de 2018, [Controles personalizados de acceso condicional de Azure AD en versión preliminar pública](https://azure.microsoft.com/updates/azure-ad-conditional-access-custom-controls-are-in-public-preview/)
* 21 de septiembre de 2018, [El acceso condicional de Azure AD ofrece ahora acceso limitado con Microsoft Cloud App Security](https://azure.microsoft.com/updates/azure-ad-conditional-access-support-for-limited-access-with-microsoft-cloud-app-security-is-now-available/)
* 21 de septiembre de 2018, [Acceso condicional de Azure AD: funcionalidad de explorador administrado para las plataformas iOS y Android en versión preliminar](https://azure.microsoft.com/updates/azure-ad-conditional-access-managed-browser-support-for-ios-android-platforms-now-in-preview/)
* 21 de septiembre de 2018, [El acceso condicional de Azure AD para códigos de país está en versión preliminar pública](https://azure.microsoft.com/updates/azure-ad-conditional-access-for-country-codes-is-in-public-preview/)
* 21 de septiembre de 2018, [Ya están disponibles los Términos de uso de Azure AD](https://azure.microsoft.com/updates/azure-ad-terms-of-use-now-available/)

## <a name="learning-resources"></a>Recursos de aprendizaje

Siga los vínculos siguientes para obtener información general sobre cómo funciona el acceso condicional de Azure AD.

* [¿Qué es el acceso condicional de Azure Active Directory?](overview.md)
* [¿Qué son las condiciones en el acceso condicional de Azure Active Directory?](conditions.md)
* [¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?](location-condition.md)
* [¿Qué son los controles de acceso en el acceso condicional de Azure Active Directory?](controls.md)
* [¿Qué es la herramienta What If en el acceso condicional de Azure Active Directory?](what-if-tool.md)
* Siga el artículo [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md).

Además, consulte los siguientes vínculos para obtener instrucciones para proteger el acceso a todos los servicios que se integran con Azure Active Directory.

* [¿Qué es la protección de base de referencia (versión preliminar)?](baseline-protection.md) La protección de base de referencia garantiza que al menos tiene el nivel de base de referencia de seguridad habilitado en el entorno de Azure Active Directory.
* [Configuraciones de acceso de dispositivos e identidades](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations) Describe cómo configurar el acceso seguro a los servicios en la nube con los productos Enterprise Mobility + Security mediante la implementación de un entorno y una configuración recomendados, incluidos un conjunto recomendado de directivas de acceso condicional y las funcionalidades relacionadas.
* [Referencia de configuración del acceso condicional de Azure Active Directory](technical-reference.md). Obtenga información sobre:
   * ¿Qué aplicaciones usan el acceso condicional?
   * ¿Qué servicios están habilitados con el acceso condicional?
* [Enable Azure Active Directory Conditional Access to secure user access ](https://www.youtube.com/watch?v=eLAYBwjCGoA) (Habilitar el acceso condicional de Azure Active Directory para proteger el acceso del usuario). En este vídeo puede obtener información sobre cómo el acceso condicional desempeña un papel en otras cargas de trabajo de Enterprise and Mobility Suite.

### <a name="training-videos"></a>Vídeos de aprendizaje

**Acceso condicional en Enterprise Mobility + Security**
   > [!VIDEO https://www.youtube.com/embed/A7IrxAH87wc]

**Acceso condicional basado en dispositivos**
   > [!VIDEO https://www.youtube.com/embed/AdM0zYB-3WQ]

**Habilitar el acceso condicional de Azure Active Directory para proteger el acceso del usuario**
   > [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

### <a name="online-courses"></a>Cursos en línea

Consulte los siguientes cursos de acceso condicional y mucho más en [Pluralsight.com](https://www.pluralsight.com/):

* Pluralsight.com: [Design Identity Management in Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-identity-management-design) (Diseño de la administración de identidades en Microsoft Azure)
   * "Este curso le guía en los elementos clave que necesita conocer para diseñar su solución de administración de identidades con Azure AD". El acceso condicional de Azure AD se trata en el módulo "Using Roles and Access Control with Azure AD" (Uso de roles y control de acceso con Azure AD).

* Pluralsight.com: [Design Authentication for Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authentication-design) (Diseño de la autenticación en Microsoft Azure)
   * "Este curso explica cómo usar Azure AD para resolver todos los requisitos de autenticación en la nube". El acceso condicional de Azure AD se trata en el módulo "Authentication Requirements for Different Scenarios" (Requisitos de autenticación para diferentes escenarios).

* Pluralsight.com: [Design Authorization for Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authorization-design) (Diseño de la autorización en Microsoft Azure)
   * "Este curso enseña las opciones de autorización disponibles con Azure y Azure AD". El acceso condicional de Azure AD se trata en el módulo "Authorization with Azure Resource Manager and Azure AD" (Autorización con Azure Resource Manager y Azure AD).

### <a name="books"></a>Libros

* O'Reilly: [Implementing Azure Solutions - Second Edition](https://www.oreilly.com/library/view/implementing-azure-solutions/9781789343045/b7ead3db-eb1c-4ace-897e-86ee25ea86be.xhtml) (Implementación de soluciones de Azure, segunda edición).
   * "Empezar a trabajar con los servicios de Azure e información sobre cómo implementarlos en su organización". El acceso condicional de Azure AD se trata en el capítulo [Deploying and Synchronizing Azure Active Directory](https://learning.oreilly.com/library/view/implementing-azure-solutions/9781789343045/02ca8bba-08cf-4691-a7d0-1b96e286e7ea.xhtml) (Implementación y sincronización de Azure Active Directory).

* Wiley: [Mastering Microsoft Azure Infrastructure Services](https://www.wiley.com/Mastering+Microsoft+Azure+Infrastructure+Services-p-9781119003298) (Dominar los servicios de infraestructura de Microsoft Azure)
   * "Aquí encontrará todo lo que necesita para entender, evaluar, implementar y mantener entornos que usan Microsoft Azure".

## <a name="white-papers"></a>Notas del producto

* 18 de diciembre de 2018, [Crear una estrategia de administración del control de acceso resistente con Azure Active Directory](../authentication/concept-resilient-controls.md)
   * En este documento se proporcionan instrucciones sobre las estrategias que una organización puede adoptar para proporcionar resistencia para reducir el riesgo de bloqueo durante interrupciones imprevistas.

* 18 de septiembre de 2018, [Recursos para migrar aplicaciones a Azure Active Directory](../manage-apps/migration-resources.md)
   * Estas notas del producto incluyen una lista de recursos que le ayudarán a migrar el acceso y la autenticación de aplicaciones a Azure Active Directory (Azure AD).

* 12 de julio de 2018, [Plano técnico de seguridad y cumplimiento de Azure: Hospedaje de aplicaciones web PaaS para cargas de trabajo de la clasificación OFFICIAL (Reino Unido)](../../security/blueprints/ukofficial-paaswa-overview.md)
   * Los planos técnicos de Azure constan de documentos de guía y plantillas de automatización que implementan arquitecturas basadas en la nube para ofrecer soluciones para escenarios que tienen requisitos de acreditación o de cumplimiento.

## <a name="guidance-for-it-administrators"></a>Guía para administradores de TI

Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global, administrador de seguridad o administrador de acceso condicional. Consulte [Permisos de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

Como administrador de TI, puede usar el [acceso condicional de Azure AD](overview.md) para hacer que los usuarios se autentiquen mediante la autenticación multifactor de Azure o que inicien sesión desde una red o dispositivo de confianza.

Estos son algunos vínculos útiles que le ayudarán a empezar a trabajar:

* [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md)
* [Uso de revisiones de acceso de Azure AD para administrar usuarios a los que se les ha excluido de las directivas de acceso condicional](../governance/conditional-access-exclusion.md)
* [How To: Planear la implementación del acceso condicional en Azure Active Directory](plan-conditional-access.md)
* [Inicio rápido: Exigir MFA para aplicaciones específicas con el acceso condicional de Azure Active Directory](app-based-mfa.md)
* [Inicio rápido: Solicitar la aceptación de los términos de uso antes de acceder a aplicaciones en la nube](require-tou.md)
* [Inicio rápido: Bloqueo del acceso cuando se detecta riesgo en una sesión con el acceso condicional de Azure Active Directory](app-sign-in-risk.md)
* [Preguntas frecuentes sobre el acceso condicional de Azure AD](faqs.md)
   * Para más preguntas, también puede visitar el [foro de MSDN](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&sort=relevancedesc&brandIgnore=True&searchTerm=password+reset+azure).
   * Si no encuentra la respuesta a un problema, nuestros equipos de soporte técnico están siempre disponibles para ayudarle. Use [Contacto con el soporte técnico de Microsoft](../authentication/active-directory-passwords-troubleshoot.md#contact-microsoft-support).

### <a name="tutorials"></a>Tutoriales

* [**Guía de inicio rápido: Exigir MFA para aplicaciones específicas con el acceso condicional de Azure Active Directory**](app-based-mfa.md)
   * En esta guía de inicio rápido se muestra cómo configurar una directiva de acceso condicional de Azure AD que exija la autenticación multifactor para una aplicación en la nube específica de su entorno.

* [**Guía de inicio rápido: Solicitud de la aceptación de los términos de uso antes de acceder a aplicaciones en la nube**](require-tou.md)
   * En esta guía de inicio rápido se muestra cómo configurar una directiva de acceso condicional de Azure AD que exija la aceptación de unas condiciones de uso para una aplicación en la nube seleccionada de su entorno.

* [**Guía de inicio rápido: Bloqueo del acceso cuando se detecta riesgo en una sesión con el acceso condicional de Azure Active Directory**](app-sign-in-risk.md)
   * En esta guía de inicio rápido se muestra cómo configurar una directiva de acceso condicional que bloquee un inicio de sesión cuando se haya detectado un nivel de riesgo de inicio de sesión configurado.

* [Tutorial: **Migración de una directiva clásica que requiere autenticación multifactor en Azure Portal**](policy-migration-mfa.md)
   * En este tutorial se muestra cómo migrar una directiva clásica que requiere autenticación multifactor para una aplicación en la nube.

## <a name="end-user-readiness-and-communication"></a>Preparación del usuario final y comunicación

El acceso condicional usa otras funcionalidades de Azure AD que pueden afectar a la experiencia del usuario final. Por ejemplo, puede usar la autenticación multifactor de Azure para habilitar una autenticación segura para los usuarios. En ese caso, usará las plantillas de usuario final de Azure MFA.

## <a name="next-steps"></a>Pasos siguientes

* Inicie la implementación con la [documentación de planeación de la implementación del acceso condicional](plan-conditional-access.md).
