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
ms.date: 04/03/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91d0dbaf9b648f42ef535d8b491df04b2c7042d7
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007632"
---
# <a name="home-realm-discovery-during-sign-in-for-microsoft-365-services"></a>Detección del dominio de inicio durante el inicio de sesión de servicios de Microsoft 365

Se está modificando nuestro comportamiento de inicio de sesión de Azure Active Directory (Azure AD) para dejar espacio para nuevos métodos de autenticación y mejorar la facilidad de uso. Durante el inicio de sesión, Azure AD determina dónde debe autenticarse un usuario. Azure AD toma decisiones inteligentes mediante la lectura de la configuración de organización y usuario para el nombre de usuario especificado en la página de inicio de sesión. Este es un paso hacia un futuro libre de contraseña que habilita las credenciales adicionales, como FIDO 2.0.

## <a name="home-realm-discovery-behavior"></a>Comportamiento de detección del dominio de inicio

Históricamente, detección de dominio de inicio se rige por el dominio que se proporciona en el inicio de sesión o mediante una directiva de detección del dominio principal para algunas aplicaciones heredadas. Por ejemplo, en el comportamiento de detección anterior un usuario de Azure Active Directory podría comete su nombre de usuario, pero todavía llegue a la pantalla de colección de credenciales de su organización. Esto se produce cuando el usuario proporciona correctamente el nombre de dominio de "contoso.com" la organización. Este comportamiento no permite la granularidad personalizar las experiencias para un usuario individual.

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
  
## <a name="additional-info"></a>Información adicional

Además de la experiencia de usuario de inicio de sesión mejorada, este cambio incluye mecanismos que pueden ayudar a mitigar el abuso de enumeración de nombre de usuario a gran escala.

Este cambio inicialmente está destinado a los dominios administrados y empieza a distribuir en mayo de 2019, pero no inicia implementando en dominios federados al final de 2019. Las fechas exactas de puesta en servicio de los dominios federados depende de los comentarios del cliente.

> [!IMPORTANT]
> Esta característica tendrán un impacto significativo en dominios federados confiar en el nivel del dominio antiguo Home Realm Discovery para forzar la federación. Algunas organizaciones han entrenado a sus empleados para iniciar sesión con un nombre de usuario que no existe en Azure Active Directory pero que contiene el nombre de dominio adecuado, porque los nombres de dominio enruta a los usuarios actualmente al punto de conexión de dominio de su organización. El nuevo comportamiento de inicio de sesión no permite esto. Se notifica al usuario para corregir el nombre de usuario y que no se les permita iniciar sesión con un nombre de usuario que no existe en Azure Active Directory.
>
> Si usted o su organización tiene prácticas que dependen del comportamiento anterior, es importante que los administradores de organización para actualizar la documentación de inicio de sesión y autenticación empleado y para entrenar los empleados puedan usar su nombre de usuario de Azure Active Directory para iniciar sesión.
  
Si tiene problemas con el nuevo comportamiento, deje sus comentarios en el **comentarios** sección de este artículo.  

## <a name="next-steps"></a>Pasos siguientes

[Personalizar su inicio de sesión de marca](../fundamentals/add-custom-domain.md)