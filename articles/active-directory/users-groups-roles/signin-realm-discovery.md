---
title: 'Búsqueda de nombre de usuario durante el inicio de sesión de autenticación: Azure Active Directory | Microsoft Docs'
description: En la pantalla de mensajería refleja la búsqueda de nombre de usuario durante el inicio de sesión
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b6846c5f907c41db16e99883be7041a68357586
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60468943"
---
# <a name="home-realm-discovery-for-azure-active-directory-sign-in-pages"></a>Detección del dominio de inicio para las páginas de inicio de sesión de Azure Active Directory

Vamos a cambiar el comportamiento de inicio de sesión en Azure Active Directory (Azure AD) para incorporar nuevos métodos de autenticación y mejorar la facilidad de uso. Durante el inicio de sesión, Azure AD determina dónde un usuario necesita autenticarse. Azure AD toma decisiones inteligentes según la lectura de la configuración de la organización y del usuario para saber el nombre de usuario escrito en la página de inicio de sesión. Se trata de un avance hacia un futuro sin contraseñas que permite otras credenciales como FIDO 2.0.

## <a name="home-realm-discovery-behavior"></a>Comportamiento de detección del dominio de inicio

Históricamente, detección de dominio de inicio se rige por el dominio que se proporciona en el inicio de sesión o mediante una directiva de detección del dominio principal para algunas aplicaciones heredadas. Por ejemplo, en nuestro comportamiento detección un usuario de Azure Active Directory podría comete su nombre de usuario pero todavía llegue a la pantalla de colección de credenciales de su organización. Esto se produce cuando el usuario proporciona correctamente el nombre de dominio de "contoso.com" la organización. Este comportamiento no permite la granularidad para personalizar las experiencias de un usuario individual.

Para admitir una gama más amplia de credenciales y aumentar la usabilidad, comportamiento de búsqueda de nombre de usuario de Azure Active Directory durante el proceso de inicio de sesión se ha actualizado. El nuevo comportamiento toma decisiones inteligentes mediante la lectura de inquilino y usuario de configuración del nivel según el nombre de usuario especificado en la página de inicio de sesión. Para que esto sea posible, Azure Active Directory comprobará si el nombre de usuario que se ha especificado en la página de inicio de sesión existe en su dominio especificado o redirige al usuario para proporcionar sus credenciales.

Una ventaja adicional de este trabajo es error mejorada de mensajería. Estos son algunos ejemplos del error al iniciar sesión en una aplicación que admite Azure Active Directory solo a los usuarios de mensajería mejorado.

1. Se escribió incorrectamente el nombre de usuario o el nombre de usuario aún no se haya sincronizado con Azure AD:
  
    ![se escribió incorrectamente o no se encuentra el nombre de usuario](./media/signin-realm-discovery/typo-username.png)
  
2. Se escribió incorrectamente el nombre de dominio:
  
    ![se escribió incorrectamente o no se encuentra el nombre de dominio](./media/signin-realm-discovery/typo-domain.png)
  
3. Usuario intenta iniciar sesión con un dominio de consumidor conocidos:
  
    ![Inicie sesión con un dominio de consumidor conocidos](./media/signin-realm-discovery/consumer-domain.png)
  
4. Se escribió incorrectamente la contraseña, pero el nombre de usuario es preciso:  
  
    ![contraseña se escribió incorrectamente con el nombre de usuario buena](./media/signin-realm-discovery/incorrect-password.png)
  
> [!IMPORTANT]
> Esta característica podría tener un impacto en los dominios federados confiar en el nivel del dominio antiguo Home Realm Discovery para forzar la federación. Para obtener actualizaciones de cuándo se agregará compatibilidad con dominios federados, vea [detección del territorio principal durante el inicio de sesión de servicios de Microsoft 365](https://azure.microsoft.com/en-us/updates/signin-hrd/). Mientras tanto, algunas organizaciones han entrenado a sus empleados para iniciar sesión con un nombre de usuario que no existe en Azure Active Directory pero que contiene el nombre de dominio adecuado, porque los nombres de dominio enruta a los usuarios actualmente al punto de conexión de dominio de su organización. El nuevo comportamiento de inicio de sesión no permite esto. Se notifica al usuario para corregir el nombre de usuario y que no se les permita iniciar sesión con un nombre de usuario que no existe en Azure Active Directory.
>
> Si usted o su organización tiene prácticas que dependen del comportamiento anterior, es importante que los administradores de organización para actualizar la documentación de inicio de sesión y autenticación empleado y para entrenar los empleados puedan usar su nombre de usuario de Azure Active Directory para iniciar sesión.
  
Si tiene problemas con el nuevo comportamiento, deje sus comentarios en el **comentarios** sección de este artículo.  

## <a name="next-steps"></a>Pasos siguientes

[Personalizar su inicio de sesión de marca](../fundamentals/add-custom-domain.md)
