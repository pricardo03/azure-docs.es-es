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
ms.openlocfilehash: 93ac5ef5f03f800a8f90259db3e382b3bc5c5e2c
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875636"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Almacenamiento de datos de identidad para los clientes europeos en Azure Active Directory
Azure AD almacena datos de identidad en una ubicación geográfica en función de la dirección proporcionada por su organización al suscribirse para un servicio de Microsoft Online como Office 365 y Azure. Para obtener información sobre dónde se almacenan los datos de identidad, puede usar el [dónde están los datos ubicados?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) sección de la Microsoft Trust Center.

Los clientes que se proporcionó una dirección en Europa, Azure AD mantiene la mayoría de los datos de identidad en los centros de datos Europeo. Este documento proporciona información sobre los datos que se almacenan fuera de Europa por servicios de Azure AD.

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Autenticación de Microsoft Azure Multi-factor Authentication (MFA)
    
- Autenticación en dos fases todo mediante llamadas telefónicas o SMS proceden de los centros de datos de EE. UU. y también se enruta mediante proveedores internacionales.
- Enviar notificaciones Push con la aplicación se originan en nosotros, los centros de datos de Microsoft Authenticator. Además, servicios específicos del proveedor de dispositivo también pueden entran en juego y estos servicios quizás fuera de Europa.
- Los códigos OATH se validan siempre en EE. UU. 

## <a name="microsoft-azure-active-directory-b2c-azure-ad-b2c"></a>Microsoft Azure Active Directory B2C (Azure AD B2C)

Azure AD B2C datos de configuración de directiva y los contenedores de claves se almacenan en los centros de datos de Estados Unidos. No tienen ningún dato personal del usuario. Para obtener más información sobre las configuraciones de directivas, consulte el artículo [Azure Active Directory B2C: directivas integradas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies).

## <a name="microsoft-azure-active-directory-b2b-azure-ad-b2b"></a>Microsoft Azure Active Directory B2B (Azure AD B2B) 
    
Invitaciones a almacenes de Azure AD B2B con canjear vinculan y redirección la información de dirección URL en los centros de datos de Estados Unidos. Además, la dirección de correo electrónico de los usuarios que cancelar la suscripción a recibir invitaciones de B2B también se almacenan en los centros de datos de Estados Unidos.

## <a name="microsoft-azure-active-directory-domain-services-azure-ad-ds"></a>Servicios de dominio de Active Directory (Azure AD DS) de Microsoft Azure

Azure AD DS almacena los datos de usuario en la misma ubicación que la instancia de Azure Virtual Network que seleccionó el cliente. Por lo tanto, si la red está fuera de Europa, los datos se replican y almacenan fuera de Europa.

## <a name="other-considerations"></a>Otras consideraciones

Servicios y aplicaciones que se integran con Azure AD tienen acceso a datos de identidad. Evalúe cada servicio y las aplicaciones usan para determinar cómo se procesan los datos de identidad por ese servicio específico y la aplicación y que cumplan los requisitos de almacenamiento de datos de su compañía.

Para obtener más información sobre la residencia de los datos de los servicios de Microsoft, consulte la sección [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Dónde se encuentran sus datos) de Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre cualquiera de las características y funcionalidades que se han descrito anteriormente, consulte estos artículos:
- [¿Qué es Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)

- [Autoservicio de restablecimiento de contraseña de Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)

- [¿Qué es Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)

- [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)

- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
