---
title: Requisitos previos de la oferta de Dynamics 365 for Customer Engagement | Azure Marketplace
description: En este artículo se describen los requisitos previos para la publicación de una oferta de aplicación de Azure en Azure Marketplace.
services: Dynamics 365 for Customer Engagement offer, Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pabutler
ms.openlocfilehash: 0b14180c894977d822aa30ea5f46a2e21e247dc1
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942328"
---
# <a name="dynamics-365-for-customer-engagement-prerequisites"></a>Requisitos previos para Dynamics 365 for Customer Engagement

En este artículo se describen los requisitos previos técnicos y empresariales para la publicación de una oferta de aplicación de Dynamics 365 for Customer Engagement en AppSource Marketplace.  Si aún no lo ha hecho, revise el [Office 365, Dynamics 365, PowerApps y Power BI ofrecen guía para la publicación](../../appsource-offer-publishing-guide.md).


## <a name="technical-requirements"></a>Requisitos técnicos

Su aplicación de Dynamics 365 for Customer Engagement debe cumplir con las [directrices de revisión de aplicaciones de Microsoft AppSource](https://smp-cdn-prod.azureedge.net/documents/AppsourceGuidelines/Microsoft%20AppSource%20app%20review%20guidelines_v5.pdf), las cuales incluyen los siguientes requisitos:


|              Requisito             |        DESCRIPCIÓN           |
|            ---------------           |      ---------------         |
| Integración de Azure Active Directory   | La aplicación debe permitir el inicio de sesión único (SSO) federado de Azure Active Directory con consentimiento habilitado. Para más información, consulte [Obtención de AppSource certificado para Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-get-appsource-certified). |
| Integración con los servicios de Microsoft Cloud (opcional) | En aquellos casos en los que se necesite esta funcionalidad, la aplicación deberá integrarse con otros servicios de Microsoft Cloud como Microsoft Power BI, Microsoft Flow, o con servicios de Microsoft Azure como Machine Learning o Cognitive Services. |
| Centrarse en la línea de negocio            |  La aplicación debe centrarse en un proceso o problema de negocio bien definido, dirigirse principalmente a clientes empresariales y permitir que los usuarios inicien sesión con sus credenciales de trabajo (nombre de usuario y contraseña).  |
| Período de evaluación gratuita y experiencia de evaluación |  Un cliente debe poder usar la aplicación de forma gratuita durante un tiempo limitado mediante estas opciones de solicitud: "Obtenerla ahora" para aplicaciones gratis, una "Evaluación gratuita" durante un período especificado, un demostrador en "Versión de prueba" o con la opción "Ponerse en contacto conmigo".  |
| Poca configuración o ninguna                 | La aplicación debe ser fácil y rápida de configurar e instalar sin necesidad de realizar ningún desarrollo ni personalización.  |
| Servicio al cliente                     | La aplicación debe incluir un vínculo de soporte técnico mediante el cual los clientes puedan recibir asistencia.  |
| Disponibilidad/tiempo de actividad                  | La aplicación debe tener un tiempo de actividad de al menos un 99,9 %. |
|  |  |


## <a name="business-requirements"></a>Requisitos empresariales

Los requisitos empresariales incluyen las siguientes obligaciones contractuales, de procedimientos y legales:

* Debe estar registrado en [Microsoft Partner Network (MPN)](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx) o ser un anunciante registrado en Cloud Marketplace. Si no está registrado, siga los pasos que se indican en [Conviértase en anunciante de Cloud Marketplace](../../become-publisher.md).  (Para el tercer paso, use [el formulario de nominación de asociado de AppSource](https://appsource.microsoft.com/partners/signup) en su lugar). 

    >[!NOTE]
    >Debe usar la misma cuenta de registro de Microsoft Developer Center para iniciar sesión en Cloud Partner Portal. Solo debe tener una cuenta Microsoft para sus ofertas de Azure Marketplace. Esta cuenta no debería ser específica de servicios ni de ofertas individuales.

* Como AppSource no ofrece una opción de publicación habilitada para comercio, deberá usar la infraestructura actual de pedidos y facturación sin necesidad de realizar ninguna inversión ni cambios adicionales.
* Usted es el responsable de poner el soporte técnico a disposición de los clientes de forma comercialmente razonable. Puede ser gratuito, de pago o a través de métodos de la comunidad.
* Asimismo, es responsable de la concesión de licencias para su software y las dependencias de software de terceros.
* Debe crear el material de marketing asociado como, por ejemplo, un nombre de aplicación oficial, una descripción (en formato HTML), imágenes de logotipo en formato PNG (40 x 40, 90 x 90, 115 x 115 y 255 x 115 píxeles), y los términos de uso y una directiva de privacidad.  


## <a name="next-steps"></a>Pasos siguientes

Una vez que cumpla con todos estos requisitos, puede [crear una oferta de Dynamics 365 Customer Engagement](./cpp-create-offer.md) 
