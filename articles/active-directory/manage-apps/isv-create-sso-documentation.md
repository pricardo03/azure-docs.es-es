---
title: Crear y publicar documentación sobre el inicio de sesión único de la aplicación
description: Guía para proveedores de software independientes sobre la integración con Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53221343ac606b6076cc9cc3cff6e0f96c1a3ac3
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659454"
---
# <a name="create-and-publish-single-sign-on-documentation-for-your-application"></a>Crear y publicar documentación sobre el inicio de sesión único de la aplicación   

## <a name="documentation-on-your-site"></a>Documentación sobre el sitio

La facilidad de adopción es un factor importante para las decisiones relacionadas con el software empresarial. Una documentación clara y fácil de seguir ayuda a los clientes en su proceso de adopción y reduce los costos de soporte técnico. El trabajo con miles de proveedores de software ha permitido a Microsoft observar lo que funciona.

Se recomienda que la documentación sobre el sitio como mínimo incluya los siguientes elementos.

* Introducción a la funcionalidad de inicio de sesión único

  * Protocolos admitidos

  * SKU y versión

  * Lista de proveedores de identidades admitidos con vínculos de documentación

* Información de licencia de la aplicación

* Control de acceso basado en rol para configurar el inicio de sesión único

* Pasos de configuración de inicio de sesión único

  * Elementos de configuración de la interfaz de usuario para SAML con los valores esperados del proveedor

  * Información del proveedor de servicio que se va a pasar a los proveedores de identidades

* Si OIDC/OAuth

  * Lista de permisos necesarios para el consentimiento con justificaciones comerciales

* Pasos de prueba para usuarios piloto

* Información de solución de problemas, incluidos mensajes y códigos de error

* Mecanismos de soporte técnico para clientes

## <a name="documentation-on-the-microsoft-site"></a>Documentación sobre el sitio de Microsoft

Al publicar la aplicación con la galería de aplicaciones de Azure Active Directory, que también publica la aplicación en Azure Marketplace, Microsoft genera documentación que explica el proceso paso a paso para los clientes mutuos. Puede ver un ejemplo [aquí](https://aka.ms/appstutorial). Esta documentación se crea en función del envío a la galería y se puede actualizar fácilmente si se realizan cambios en la aplicación con la cuenta de GitHub.

## <a name="next-steps"></a>Pasos siguientes

[Registro de una aplicación en la galería de aplicaciones de Azure AD](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx)