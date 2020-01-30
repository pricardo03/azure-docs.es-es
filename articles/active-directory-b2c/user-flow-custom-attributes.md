---
title: Definición de atributos personalizados en Azure Active Directory B2C | Microsoft Docs
description: Defina atributos personalizados para su aplicación en Azure Active Directory B2C a fin de recopilar información sobre sus clientes.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 433eff8f7ec22a3484e8e7f38dab2bb1c24e2fcc
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850811"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Definición de atributos personalizados en Azure Active Directory B2C

 Todas las aplicaciones orientadas al cliente tienen requisitos únicos para la información que debe recopilarse. El inquilino de Azure Active Directory B2C (Azure AD B2C) incluye un conjunto integrado de información almacenada en atributos, como el nombre propio, los apellidos, la ciudad y el código postal. Con Azure AD B2C, puede ampliar el conjunto de atributos que se almacenan en cada cuenta de cliente.

 Puede crear atributos personalizados en [Azure Portal](https://portal.azure.com/) y usarlos en sus flujos de usuario de registro, de inicio de sesión o de edición de perfiles. También puede leer y escribir estos atributos mediante [Graph API de Azure AD](manage-user-accounts-graph-api.md). Los atributos personalizados de Azure AD B2C usan las [Extensiones de esquema de directorio de Graph API de Azure AD](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

> [!NOTE]
> La compatibilidad con versiones más recientes de [Microsoft Graph API](https://docs.microsoft.com/graph/overview?view=graph-rest-1.0) para consultar el inquilino de Azure AD B2C se encuentra aún en proceso de desarrollo.
>

## <a name="create-a-custom-attribute"></a>Creación de un atributo personalizado

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**.

    ![Cambiar al inquilino de Azure AD B2C](./media/user-flow-custom-attributes/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Inquilino de B2C resaltado en el filtro de directorio y suscripción](./media/user-flow-custom-attributes/select-directory.PNG)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Atributos de usuario** y, a continuación, seleccione **Agregar**.
5. Proporcione un **Nombre** para el atributo personalizado (por ejemplo, "ShoeSize").
6. Elija un **Tipo de datos**. Solo están disponibles **String**, **Boolean** e **Int**.
7. Si lo desea, escriba una **Descripción** con fines informativos.
8. Haga clic en **Crear**.

El atributo personalizado ahora está disponible en la lista de **Atributos de usuario** y puede usarlo en los flujos de usuario. Solo se crea la primera vez que se utiliza en cualquier flujo de usuario y no cuando se agrega a la lista de **atributos de usuario**.


## <a name="use-a-custom-attribute-in-your-user-flow"></a>Uso de un atributo personalizado en el flujo de usuario

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
2. Haga clic en la directiva (por ejemplo, "B2C_1_SignupSignin") para abrirla.
4. Seleccione **Atributos de usuario** y, después, seleccione el atributo personalizado (por ejemplo, "ShoeSize"). Haga clic en **Save**(Guardar).
5. Seleccione **Notificaciones de aplicación** y, después, seleccione el atributo personalizado.
6. Haga clic en **Save**(Guardar).

Cuando haya creado un usuario mediante un flujo de usuario que usa el atributo personalizado recién creado, el objeto se puede consultar en el [Explorador de Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart). Como alternativa, puede usar la característica [**Ejecutar flujo de usuario**](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows) del flujo de usuario para comprobar la experiencia del cliente. Ahora debe ver **ShoeSize** en la lista de atributos que se recopilan durante el viaje de suscripción y en el token enviado de vuelta a la aplicación.

