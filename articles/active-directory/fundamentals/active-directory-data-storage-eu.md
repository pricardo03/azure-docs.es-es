---
title: Almacenamiento de datos de identidad para los clientes europeos en Azure Active Directory | Microsoft Docs
description: Obtenga información sobre dónde almacena Azure Active Directory los datos relacionados con la identidad de los clientes europeos.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/04/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: b21f82dc0a1eb8edf571da13e0d34fecae5f401b
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337692"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Almacenamiento de datos de identidad para los clientes europeos en Azure Active Directory
Azure Active Directory (Azure AD) facilita la administración de las identidades de los usuarios y la creación de directivas de acceso basadas en inteligencia para proteger los recursos de su organización. Los datos de identidad se almacenan en una ubicación que se basa la dirección que su organización proporcionó al suscribirse al servicio. Por ejemplo, al suscribirse a Office 365 o Azure. Para obtener información específica sobre dónde se almacenan los datos de identidad, puede consultar la sección [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Dónde se encuentran sus datos) de Microsoft Trust Center.

Aunque Azure más datos relacionados con AD identidad Europeo permanecen en los centros de datos europeos, hay algunos datos operacionales, específicos del servicio que se necesita para normal operación de Azure AD, que se almacenan en los Estados Unidos y no incluye ningún dato personal.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Datos almacenados fuera de los centros de datos europeos para los clientes europeos

La mayoría de los datos de identidad europeos relacionados con Azure AD, para organizaciones con direcciones basadas en Europa, se mantiene en centros de datos europeos. Los datos de Azure AD que se almacenan en los centros de datos europeos y también los que se replican en los centros de datos de Estados Unidos, incluyen:

- **Autenticación multifactor (MFA) de Microsoft Azure y restablecimiento de contraseña de autoservicio (SSPR) de Azure AD**
    
    MFA almacena todos los datos de usuario en reposo en centros de datos europeos. Sin embargo, algunos datos específicos del servicio de MFA se almacenan EE. UU., incluyendo:
    
    - Los datos de autenticación en dos fases y los datos personales relacionados podrían almacenarse en EE. UU si utiliza MFA o SSPR.

        - Si la autenticación en dos fases usa llamadas telefónicas o SMS, puede realizarse a través de operadores estadounidenses.
    
        - Las notificaciones push que usen la aplicación Microsoft Authenticator requieren notificaciones del servicio de notificaciones del fabricante (Apple o Google), que podría estar fuera de Europa.
    
        - Los códigos OATH se validan siempre en EE. UU. 
    
    - Algunos registros de MFA y SSPR se almacenan en EE. UU. durante 30 días, independientemente del tipo de autenticación.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Azure AD B2C almacena todos los datos de usuario en reposo en centros de datos europeos. Sin embargo, los registros operativos (de los que se han quitado los datos personales) permanecen en la ubicación desde donde la persona obtiene acceso a los servicios. Por ejemplo, si un usuario de B2C accede al servicio en Estados Unidos, los registros operativos permanecen en EE. UU. Además, todos los datos de configuración de directivas que no contengan datos personales se almacenan solo en EE. UU. Para obtener más información sobre las configuraciones de directivas, consulte el artículo [Azure Active Directory B2C: directivas integradas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies).

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Azure AD B2B almacena todos los datos de usuario en reposo en centros de datos europeos. Sin embargo, B2B almacena sus metadatos de carácter no personal en tablas en centros de datos de EE. UU. Estas tablas incluyen campos como redeemUrl, invitationTicket, resource tenant Id, InviteRedirectUrl e InviterAppId.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    Azure AD DS almacena los datos de usuario en la misma ubicación que la instancia de Azure Virtual Network que seleccionó el cliente. Por lo tanto, si la red está fuera de Europa, los datos se replican y almacenan fuera de Europa.

- **Servicios y aplicaciones integrados en Azure AD**

    Los servicios y las aplicaciones que se integran en Azure AD tienen acceso a datos de identidad. Evalúe cada servicio y aplicación para conocer la manera en que cada servicio y aplicación específico procesa los datos de identidad, y si cumple los requisitos de almacenamiento de datos de la empresa.

    Para obtener más información sobre la residencia de los datos de los servicios de Microsoft, consulte la sección [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Dónde se encuentran sus datos) de Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre cualquiera de las características y funcionalidades que se han descrito anteriormente, consulte estos artículos:
- [¿Qué es Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)

- [Autoservicio de restablecimiento de contraseña de Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)

- [¿Qué es Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)

- [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)

- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
