---
title: Migración de una directiva clásica que requiere autenticación multifactor en Azure Portal
description: En este artículo se muestra cómo migrar una directiva clásica que requiere autenticación multifactor en Azure Portal.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: tutorial
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6661cee8ba6176bd706d31a10a8f20549e29e4d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795056"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Migración de una directiva clásica que requiere autenticación multifactor en Azure Portal

En este tutorial se muestra cómo migrar una directiva clásica que requiere **autenticación multifactor** para una aplicación en la nube. Aunque no es un requisito previo, le recomendamos que lea [Migración de directivas clásicas en Azure Portal](policy-migration.md) antes de iniciar la migración de las directivas clásicas.

## <a name="overview"></a>Información general

En el escenario de este artículo se muestra cómo migrar una directiva clásica que requiere **autenticación multifactor** para una aplicación en la nube.

![Azure Active Directory](./media/policy-migration/33.png)

El proceso de migración consta de los pasos siguientes:

1. [Abra la directiva clásica](#open-a-classic-policy) para obtener las opciones de configuración.
1. Cree una nueva directiva de acceso condicional de Azure AD para reemplazar la directiva clásica. 
1. Deshabilite la directiva clásica.

## <a name="open-a-classic-policy"></a>Abrir una directiva clásica

1. En [Azure Portal](https://portal.azure.com), en la barra de navegación izquierda, haga clic en **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration-mfa/01.png)

1. En la página **Azure Active Directory**, en la sección **Administrar**, haga clic en **Acceso condicional**.

   ![Acceso condicional](./media/policy-migration-mfa/02.png)

1. En la sección **Administrar**, haga clic en **Directivas clásicas (vista previa)**.

   ![Directivas clásicas](./media/policy-migration-mfa/12.png)

1. En la lista de directivas clásicas, haga clic en la directiva que requiera **autenticación multifactor** para una aplicación en la nube.

   ![Directivas clásicas](./media/policy-migration-mfa/13.png)

## <a name="create-a-new-conditional-access-policy"></a>Crear una nueva directiva de acceso condicional

1. En [Azure Portal](https://portal.azure.com), en la barra de navegación izquierda, haga clic en **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration/01.png)

1. En la página **Azure Active Directory**, en la sección **Administrar**, haga clic en **Acceso condicional**.

   ![Acceso condicional](./media/policy-migration/02.png)

1. En la página **Acceso condicional**, en la barra de herramientas de la parte superior, haga clic en **Agregar** para abrir la página **Nuevo**.

   ![Acceso condicional](./media/policy-migration/03.png)

1. En la página **Nuevo**, en el cuadro de texto **Nombre**, escriba un nombre para la directiva.

   ![Acceso condicional](./media/policy-migration/29.png)

1. En la sección **Asignaciones**, haga clic en **Usuarios y grupos**.

   ![Acceso condicional](./media/policy-migration/05.png)

   1. Si tiene todos los usuarios seleccionados en la directiva clásica, haga clic en **Todos los usuarios**. 

   ![Acceso condicional](./media/policy-migration/35.png)

   1. Si tiene grupos seleccionados en la directiva clásica, haga clic en **Seleccionar usuarios y grupos** y, después, seleccione los grupos y usuarios necesarios.

   ![Acceso condicional](./media/policy-migration/36.png)

   1. Si tiene los grupos excluidos, haga clic en la pestaña **Excluir** y, después, seleccione los grupos y usuarios necesarios. 

   ![Acceso condicional](./media/policy-migration/37.png)

1. En la página **Nuevo**, en la sección **Asignación**, haga clic en **Aplicaciones en la nube** para abrir la página **Aplicaciones en la nube**.

1. En la página **Aplicaciones en la nube**, siga estos pasos:

   ![Acceso condicional](./media/policy-migration/08.png)

   1. Haga clic en **Seleccionar aplicaciones**.

   1. Haga clic en **Seleccionar**.

   1. En la página **Seleccionar**, seleccione la aplicación en la nube y haga clic en **Seleccionar**.

   1. En la página **Aplicaciones en la nube**, haga clic en **Listo**.

1. Si seleccionó **Requerir autenticación multifactor**:

   ![Acceso condicional](./media/policy-migration/26.png)

   1. En la sección **Controles de acceso**, haga clic en **Conceder**.

   ![Acceso condicional](./media/policy-migration/27.png)

   1. En la página **Conceder**, haga clic en **Conceder acceso** y, después, haga clic en **Requerir autenticación multifactor**.

   1. Haga clic en **Seleccionar**.

1. Haga clic en **On** (Activar) para habilitar la directiva.

   ![Acceso condicional](./media/policy-migration/30.png)

## <a name="disable-the-classic-policy"></a>Deshabilitar la directiva clásica

Para deshabilitar la directiva clásica, haga clic en **Deshabilitar** en la vista **Detalles**.

![Directivas clásicas](./media/policy-migration-mfa/14.png)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la migración de directivas clásicas, consulte [Migración de directivas clásicas en Azure Portal](policy-migration.md).
- Si quiere saber cómo configurar una directiva de acceso condicional, consulte [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).
- Si está listo para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md).
