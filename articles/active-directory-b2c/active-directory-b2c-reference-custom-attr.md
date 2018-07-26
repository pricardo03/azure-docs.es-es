---
title: Definición de atributos personalizados en Azure Active Directory B2C | Microsoft Docs
description: Defina atributos personalizados para su aplicación en Azure Active Directory B2C a fin de recopilar información sobre sus clientes.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d5ef77ab0bbf00d4ddbb05b7a38516e3c3e7d800
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968781"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Definición de atributos personalizados en Azure Active Directory B2C

 Todas las aplicaciones orientadas al cliente tienen requisitos únicos para la información que debe recopilarse. El inquilino de Azure Active Directory (Azure AD) B2C incluye un conjunto integrado de información almacenada en atributos, como el nombre propio, los apellidos, la ciudad y el código postal. Con Azure AD B2C, puede ampliar el conjunto de atributos que se almacenan en cada cuenta de cliente. 
 
 Puede crear atributos personalizados en [Azure Portal](https://portal.azure.com/) y usarlos en sus directivas de registro, de inicio de sesión o de edición de perfiles. También puede leer y escribir estos atributos mediante [Graph API de Azure AD](active-directory-b2c-devquickstarts-graph-dotnet.md). Los atributos personalizados de Azure AD B2C usan las [Extensiones de esquema de directorio de Graph API de Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

## <a name="create-a-custom-attribute"></a>Creación de un atributo personalizado

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**. 

    ![Cambiar al inquilino de Azure AD B2C](./media/active-directory-b2c-reference-custom-attr/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Selección de directorio](./media/active-directory-b2c-reference-custom-attr/select-directory.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Atributos de usuario** y, a continuación, seleccione **Agregar**.
5. Proporcione un **Nombre** para el atributo personalizado (por ejemplo, "ShoeSize").
6. Elija un **Tipo de datos**. Solo están disponibles **String**, **Boolean** e **Int**.
7. Si lo desea, escriba una **Descripción** con fines informativos. 
8. Haga clic en **Create**(Crear).

El atributo personalizado ahora está disponible en la lista de **Atributos de usuario** y puede usarlo en las directivas. Solo se crea la primera vez que se utiliza en cualquier directiva y no cuando se agrega a la lista de **atributos de usuario**.

## <a name="use-a-custom-attribute-in-your-policy"></a>Uso de un atributo personalizado en la directiva

1. En el inquilino de Azure AD B2C, seleccione **Sign-up or sign-in policies** (Directivas de registro o inicio de sesión).
2. Haga clic en la directiva (por ejemplo, "B2C_1_SignupSignin") para abrirla. 
3. Haga clic en **Editar**.
4. Seleccione **Atributos de registro** y, después, seleccione el atributo personalizado (por ejemplo, "ShoeSize"). Haga clic en **OK**.
5. Seleccione **Notificaciones de aplicación** y, después, seleccione el atributo personalizado. Haga clic en **OK**.
6. Haga clic en **Save**(Guardar).

Puede usar la característica **Ejecutar ahora** en la directiva para comprobar la experiencia del cliente. Ahora debe ver **ShoeSize** en la lista de atributos que se recopilan durante el viaje de suscripción y en el token enviado de vuelta a la aplicación.

