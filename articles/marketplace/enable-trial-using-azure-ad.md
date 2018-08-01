---
title: Habilitación de una versión de evaluación en Azure Marketplace mediante Azure Active Directory | Azure
description: Habilite un tipo de aplicación Versión de evaluación mediante Azure Active Directory en Azure Marketplace y AppSource para los anunciantes de aplicaciones y servicios.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/04/2018
ms.author: ellacroi
ms.openlocfilehash: c5b7b4967c1acef733d366e651d50706db42aace
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160474"
---
# <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Habilitación de una aplicación Versión de evaluación en Azure Marketplace mediante Azure Active Directory

Azure Active Directory (Azure AD) es un servicio de identidad de nube que permite la autenticación con una cuenta profesional o educativa de Microsoft mediante marcos estándar del sector. Azure AD admite la autenticación de OAuth y OpenID Connect. [Azure Marketplace](https://azuremarketplace.microsoft.com) utiliza Azure AD para autenticarle a usted y sus clientes.

Para más información sobre Azure AD, vea [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

Una vez que un cliente selecciona la aplicación Versión de evaluación en Marketplace, se le remite al entorno de prueba. En el entorno de prueba, puede configurar el cliente directamente, sin necesidad de pasos de inicio de sesión adicionales. La aplicación u oferta recibe un token de Azure AD durante la autenticación. El token incluye información valiosa del usuario que se usa para crear una cuenta de usuario en la aplicación u oferta. Puede automatizar la instalación de cliente y aumentar la probabilidad de conversión.

Para más información sobre el token que se envía desde Azure AD durante la autenticación, vea [Tokens de ejemplo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Use Azure AD para habilitar la autenticación con un solo clic en la aplicación o en la versión de evaluación. Azure AD le ofrece las siguientes ventajas: 
*   Optimizar la experiencia del cliente de Marketplace a la versión de evaluación.
*   Mantenga la sensación de una "experiencia dentro del producto" incluso cuando se redirige al usuario desde Marketplace hasta el dominio o el entorno de evaluación.
*   Reducir la probabilidad de abandono tras el redireccionamiento al no haber pasos adicionales de inicio de sesión.
*   Reduzca las barreras de implementación para la gran población de usuarios de Azure AD.

## <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Verificación de la integración de Azure AD en Marketplace: aplicaciones multiinquilino
Utilice Azure AD para admitir las opciones siguientes en la solución:
*   Registre la aplicación en los escaparates de Marketplace.
*   Habilite la característica de compatibilidad multiinquilino en Azure AD para obtener una experiencia de evaluación con un clic.

Para más información sobre el registro de aplicaciones, vea [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

Si no está familiarizado con el uso del inicio de sesión único (SSO) federado de Azure AD, siga estos pasos
1.  Registre la aplicación en Marketplace. 
2.  Desarrolle el SSO con Azure AD mediante el uso de OAuth 2.0 u OpenID Connect.
    *   Para más información sobre OAuth 2.0, vea [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
    *   Para más información sobre Open ID Connect, vea [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
3.  Habilite la característica de compatibilidad multiinquilino en Azure AD para proporcionar una experiencia de evaluación con un clic.
    
    Para más información sobre la certificación de AppSource, vea [Obtención de AppSource certificado para Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Verificación de la integración de Azure AD en Marketplace: aplicaciones de inquilino único
Utilice Azure AD para admitir una de las opciones siguientes en la solución de inquilino único: 
*   Agregue usuarios al directorio como invitados mediante B2B de Azure Active Directory (Azure AD B2B).
    
    Para más información sobre Azure AD B2B, vea [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Configure manualmente las versiones de evaluación de los clientes mediante la opción de publicación Ponerse en contacto conmigo.
*   Desarrolle una versión de prueba por cliente.
*   Cree una aplicación de demostración de ejemplo multiinquilino que use SSO.

## <a name="next-steps"></a>Pasos siguientes
*   Revise la [Guía del anunciante de Azure Marketplace y AppSource](./marketplace-publishers-guide.md).
