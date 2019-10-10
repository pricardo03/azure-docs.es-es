---
title: 'Tutorial: Creación de un inquilino de Azure Active Directory B2C'
description: Aprenda a preparar el registro de sus aplicaciones mediante la creación de un inquilino de Azure Active Directory B2C desde Azure Portal.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: de29929c8fda476fe276f91d4a68ce4d632503fd
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71345214"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Creación de un inquilino de Azure Active Directory B2C

Para que sus aplicaciones puedan interactuar con Azure Active Directory B2C (Azure AD B2C), deben estar registradas en un inquilino que administre.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción
> * Cambio al directorio que contiene el inquilino de Azure AD B2C
> * Incorporación del recurso de Azure AD B2C como *Favorito* en Azure Portal

En el siguiente tutorial aprenderá a registrar una aplicación.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Asegúrese de que está usando el directorio que contiene su suscripción.

    Seleccione el filtro **Directorio y suscripción** en el menú superior y, a continuación, seleccione el directorio que contiene la suscripción. Se trata de un directorio diferente al que contendrá al inquilino de Azure AD B2C.

    ![Filtro de directorio y suscripción con el inquilino de suscripción seleccionado](media/tutorial-create-tenant/portal-01-select-directory.png)

1. Seleccione **Crear un recurso** en la esquina superior izquierda de Azure Portal.
1. Busque y seleccione **Active Directory B2C** y, a continuación, seleccione **Crear**.
1. Seleccione **Crear un nuevo inquilino de Azure AD B2C**.

    ![Creación de un nuevo inquilino de Azure AD B2C seleccionado en Azure Portal](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. Rellene los campos **Nombre de la organización** y **Nombre de dominio inicial**. Seleccione el **país o la región** (no se puede cambiar más adelante) y seleccione **Crear**.

    El nombre de dominio se usa como parte del nombre de dominio del inquilino completo. En este ejemplo, el nombre del inquilino es *contosob2c.onmicrosoft.com*:

    ![Creación de un formulario del inquilino con valores de ejemplo en Azure Portal](media/tutorial-create-tenant/portal-03-tenant-naming.png)

1. Una vez completada la creación de inquilinos, seleccione el vínculo **Crear nuevo inquilino B2C o vincular al inquilino existente** en la parte superior de la página de creación de inquilinos.

    ![Vinculación del vínculo de la ruta de navegación del inquilino resaltado en Azure Portal](media/tutorial-create-tenant/portal-04-select-link-sub-link.png)

1. Seleccione **Vincular un inquilino de Azure AD B2C existente a mi suscripción de Azure**.

   ![Vinculación de una selección de suscripción existente en Azure Portal](media/tutorial-create-tenant/portal-05-link-subscription.png)

1. Seleccione el **inquilino de Azure AD B2C** que creó y, a continuación, seleccione su **suscripción**.

    En **Grupo de recursos**, seleccione **Crear nuevo**. Escriba un **nombre** para el grupo de recursos que contendrá el inquilino, seleccione la **ubicación del grupo de recursos** y, a continuación, seleccione **Crear**.

    ![Vinculación del formulario de configuración de la suscripción en Azure Portal](media/tutorial-create-tenant/portal-06-link-subscription-settings.png)

## <a name="select-your-b2c-tenant-directory"></a>Selección de su directorio de inquilinos B2C

Para empezar a usar su nuevo inquilino de Azure AD B2C, debe cambiar al directorio que contiene el inquilino.

Seleccione el filtro **Directorio y suscripción** en el menú superior de Azure Portal y seleccione el directorio que contiene el inquilino de Azure AD B2C.

Si al principio no ve el nuevo inquilino de Azure B2C en la lista en la lista, actualice la ventana del explorador y seleccione el filtro **Directorio y suscripción** de nuevo en el menú superior.

![Directorio que contiene el inquilino B2C seleccionado en Azure Portal](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Incorporación de Azure AD B2C como favorito (opcional)

Este paso opcional facilita la selección de su inquilino de Azure AD B2C en los siguientes tutoriales (y todos los posteriores).

En lugar de buscar "Azure AD B2C" en **Todos los servicios** cada vez que desee trabajar con su inquilino, puede usar el recurso como favorito. A continuación, puede seleccionarlo en el menú **Favoritos** izquierdo para ir rápidamente a su inquilino de Azure AD B2C.

Solo tiene que realizar esta operación una vez. Antes de realizar estos pasos, asegúrese de que ha cambiado al directorio que contiene su inquilino de Azure AD B2C como se describe en la sección anterior, [Selección de su directorio de inquilinos B2C](#select-your-b2c-tenant-directory).

1. Seleccione **Todos los servicios** en el menú izquierdo de [Azure Portal](https://portal.azure.com).
1. Escriba *Azure AD B2C* en el cuadro de texto de búsqueda.
1. Seleccione la **estrella** para agregar Azure AD B2C a sus favoritos.
1. *Azure AD B2C* ahora aparece en el menú izquierdo **Favoritos**. A continuación, puede seleccionarlo y arrastrarlo más alto en la lista, si lo desea, tal como se muestra en la siguiente imagen:

![Pasos para agregar Azure AD B2C como favorito en Azure Portal](media/tutorial-create-tenant/portal-08-favorite-b2c.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción
> * Cambio al directorio que contiene el inquilino de Azure AD B2C
> * Incorporación del recurso de Azure AD B2C como *Favorito* en Azure Portal

A continuación, aprenda a registrar una aplicación web en el nuevo inquilino.

> [!div class="nextstepaction"]
> [Registro de las aplicaciones >](tutorial-register-applications.md)
