---
title: Habilitar evaluación gratuita con Azure AD | Azure
description: Habilite el tipo de anuncio de evaluación con Azure Active Directory (Azure AD) en Azure Marketplace y AppSource para los anunciantes de aplicaciones y servicios
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
ms.openlocfilehash: 4140ba98c0c65c22674c61dc7266818af904e777
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34826274"
---
# <a name="enable-trial-using-azure-ad"></a>Habilitar evaluación gratuita con Azure AD  
Azure Active Directory (Azure AD) es un servicio de identidad de nube que permite la autenticación con una cuenta profesional o educativa de Microsoft mediante marcos de trabajo estándar de la industria: OAuth y OpenID Connect.  
*   Para más información sobre Azure AD, visite la página de Azure Active Directory en [azure.microsoft.com/services/active-directory](https://azure.microsoft.com/services/active-directory).  

Usted y sus clientes se autentican en Marketplace con Azure AD. Una vez que el cliente selecciona el anuncio de evaluación en Marketplace, se le remite al entorno de evaluación.  En el entorno de evaluación, puede configurar el cliente directamente, sin necesidad de pasos de inicio de sesión adicionales. La aplicación u oferta reciben un token de Azure AD durante la autenticación que incluye información valiosa del usuario para crear una cuenta de usuario en la aplicación u oferta. Puede automatizar la instalación y aumentar la probabilidad de conversión.  
*   Para más información sobre el token enviado desde Azure AD durante la autenticación, viste la sección de tokens de muestra en [docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens)

Use Azure AD para habilitar la autenticación con un solo clic en la aplicación o en la versión de evaluación.  
*   Optimice la experiencia del cliente de Marketplace a la versión de evaluación.  
*   Mantenga la sensación de una "experiencia dentro del producto" incluso cuando se redirige al usuario desde Marketplace hasta el dominio o el entorno de evaluación.  
*   Reduzca la probabilidad de abandono en el redireccionamiento al no haber un paso adicional de inicio de sesión.  
*   Reduzca las barreras de implementación para la gran población de usuarios de Azure AD.  

## <a name="verify-your-azure-ad-integration-on-the-marketplace-multitenant-apps"></a>Verificación de la integración de Azure AD en Marketplace: aplicaciones multiinquilino  
Se admiten las siguientes opciones para la solución con Azure AD.  
*   Registre la aplicación en los escaparates de Marketplace.  
*   Habilite la característica de compatibilidad multiinquilino en Azure AD para obtener una experiencia de evaluación con un clic.  
    *   Para más información sobre el registro de aplicaciones, visite la página Integración de aplicaciones con Azure Active Directory en [docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Si no está familiarizado con el uso del inicio de sesión único (SSO) federado de Azure AD, siga estos pasos.  
1.  Registre la aplicación en Marketplace. 
2.  Desarrolle el SSO con Azure AD mediante el uso de OAuth 2.0 u OpenID Connect.  
    *   Para más información sobre OAuth 2.0, visite la página de OAuth 2.0 en [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).  
    *   Para más información sobre Open ID Connect, visite la página de OpenID Connect en [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  
3.  Habilite la característica de compatibilidad multiinquilino en Azure AD para proporcionar una experiencia de evaluación con un clic.  
    *   Para más información sobre la certificación de AppSource, visite la página de certificación de AppSource en [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-on-the-marketplace-single-tenant-apps"></a>Verificación de la integración de Azure AD en Marketplace: aplicaciones de un único inquilino  
Admita alguna de las siguientes opciones para la solución de un único inquilino.  
*   Agregue usuarios al directorio como invitados con B2B de Azure AD.  
    *   Para más información sobre B2B de Azure AD, visite la página ¿Qué es la colaboración B2B de Azure AD? en [docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Configure manualmente versiones de evaluación para los clientes mediante la opción de "Contacto".  
*   Desarrolle una versión de prueba por cliente.  
*   Cree una aplicación de demostración de ejemplo multiinquilino con SSO.  

## <a name="next-steps"></a>Pasos siguientes
*   Visite la página [Guía del anunciante de Azure Marketplace y AppSource](./marketplace-publishers-guide.md).  
 
---  

