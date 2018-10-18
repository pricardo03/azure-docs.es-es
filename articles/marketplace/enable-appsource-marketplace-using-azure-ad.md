---
title: Habilitar una descripción de Microsoft AppSource y Azure Marketplace mediante Azure Active Directory | Azure
description: Habilite un tipo de aplicación mediante Azure Active Directory en Azure Marketplace y AppSource para los anunciantes de aplicaciones y servicios.
services: Azure, AppSource, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/12/2018
ms.author: qianw211
ms.openlocfilehash: d7fd09928c0a687755d216e7f10f7eac23677c63
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45987346"
---
# <a name="enable-a-microsoft-appsource-and-azure-marketplace-listing-by-using-azure-active-directory"></a>Habilitar una descripción de Microsoft AppSource y Azure Marketplace mediante Azure Active Directory

Microsoft Azure Active Directory (Azure AD) es un servicio de identidad en la nube que permite la autenticación con una cuenta de Microsoft mediante marcos estándar del sector.  Para más información sobre Azure AD, vea [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="benefits-of-using-azure-active-directory"></a>Beneficios de usar Azure Active Directory

Los clientes de Microsoft AppSource y Azure Marketplace usan las experiencias del producto para buscar los catálogos de aplicaciones, por lo que tendrán que volver a iniciar sesión en el producto.  Al integrar la aplicación con Azure AD, puede acelerar la involucración del usuario y optimizar la experiencia del cliente. Azure AD:

- Permite el inicio de sesión único (SSO) para millones de usuarios de empresas.
- Habilita una experiencia de inicio de sesión de usuario coherente en todas las aplicaciones que hayan publicado diferentes asociados.
- Proporciona una opción de autenticación escalable entre plataformas para sus aplicaciones móviles y en la nube.

Tal como se detalla en la sección siguiente, determinadas ofertas son necesarias para implementar Azure AD para poder publicar contenido en Marketplace.

## <a name="azure-active-directory-requirements"></a>Requisitos de Azure Active Directory

Hay diferentes [tipos de ofertas y opciones de aplicación](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) para Microsoft AppSource y Azure Marketplace.  Los requisitos de Azure AD para estas opciones de aplicación y tipos de oferta se muestran a continuación:

| **Tipo de oferta**    | **¿Es necesario el inicio de sesión único de AAD?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Ponerse en contacto conmigo | Versión de prueba | Versión de prueba | Transacción |
| Máquina virtual | N/D | Sin  | No | Sin  |
| Azure Apps (plantilla de solución)  | N/D | N/D | N/D | N/D |
| Aplicaciones administradas  | N/D | N/D | N/D | Sin  |
| SaaS  | Sin  | SÍ | Sí | SÍ |
| Contenedores  | N/D | N/D | N/D | Sin  |
| Servicios de consultoría  | Sin  | N/D | N/D | N/D |

Para obtener más información acerca de los requisitos técnicos de SaaS, consulte la [Guía de publicación de ofertas de aplicaciones de SaaS](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="integration-with-azure-active-directory"></a>Integración con Azure Active Directory

Para obtener información sobre cómo integrar con Azure AD para habilitar el inicio de sesión único, visite https://aka.ms/aaddev.

Para obtener más información acerca del inicio de sesión único de Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Habilitación de una aplicación Versión de evaluación en Azure Marketplace mediante Azure Active Directory

Una vez que un cliente selecciona la aplicación Versión de evaluación en Marketplace, se le remite al entorno de prueba. En el entorno de prueba, puede configurar el cliente directamente, sin necesidad de pasos de inicio de sesión adicionales. La aplicación u oferta recibe un token de Azure AD durante la autenticación. El token incluye información valiosa del usuario que se usa para crear una cuenta de usuario en la aplicación u oferta. Puede automatizar la instalación de cliente y aumentar la probabilidad de conversión.

Para más información sobre el token que se envía desde Azure AD durante la autenticación, vea [Tokens de ejemplo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Use Azure AD para habilitar la autenticación con un solo clic en la aplicación o en la versión de evaluación. Azure AD le ofrece las siguientes ventajas: 
*   Optimizar la experiencia del cliente de Marketplace a la versión de evaluación.
*   Mantenga la sensación de una "experiencia dentro del producto" incluso cuando se redirige al usuario desde Marketplace hasta el dominio o el entorno de evaluación.
*   Reducir la probabilidad de abandono tras el redireccionamiento al no haber pasos adicionales de inicio de sesión.
*   Reduzca las barreras de implementación para la gran población de usuarios de Azure AD.

### <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Verificación de la integración de Azure AD en Marketplace: aplicaciones multiinquilino
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

### <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Verificación de la integración de Azure AD en Marketplace: aplicaciones de inquilino único
Utilice Azure AD para admitir una de las opciones siguientes en la solución de inquilino único: 
*   Agregue usuarios al directorio como invitados mediante B2B de Azure Active Directory (Azure AD B2B). Para más información sobre Azure AD B2B, vea [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Configure manualmente las versiones de evaluación de los clientes mediante la opción de publicación Ponerse en contacto conmigo.
*   Desarrolle una versión de prueba por cliente.
*   Cree una aplicación de demostración de ejemplo para varios inquilinos que use el inicio de sesión único.

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, 
- [Regístrese](https://azuremarketplace.microsoft.com/sell) en Marketplace.

Si está registrado y está creando una oferta nueva o trabajando en una existente,
- [Inicie sesión en Cloud Partner Portal](https://cloudpartner.azure.com/) para crear o completar la oferta

