---
title: 'Tutorial: Creación del primer paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Tutorial paso a paso sobre cómo crear el primer paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.subservice: compliance
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76ba284ec1a30322a24c762a1829b399f2583c6c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69032917"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management-preview"></a>Tutorial: Creación del primer paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Administrar el acceso a todos lol recursos que necesitan los empleados, como grupos, aplicaciones y sitios, es una función importante en las organizaciones. Querrá conceder a los empleados el nivel de acceso correcto que necesitan para ser productivos y eliminar su acceso cuando ya no se precise.

En este tutorial, trabajará para Woodgrove Bank como administrador de TI. Le han pedido que cree un paquete de recursos para un proyecto web al que los usuarios internos pueden acceder mediante una solicitud de autoservicio. Las solicitudes requieren aprobación y el acceso del usuario expira al cabo de 30 días. En este tutorial, los recursos del proyecto web son simplemente la pertenencia a un único grupo, pero podrían ser una colección de grupos, aplicaciones o sitios de SharePoint Online.

![Información general de escenario](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un paquete de acceso con un grupo como recurso
> * Designar un aprobador
> * Demostrar cómo un usuario interno puede solicitar el paquete de acceso
> * Aprobar la solicitud de acceso

Para consultar una demostración detallada del proceso de implementación de la administración de derechos de Azure Active Directory, incluida la creación del primer paquete de acceso, vea el siguiente vídeo:

>[!VIDEO https://www.youtube.com/embed/zaaKvaaYwI4]

## <a name="prerequisites"></a>Requisitos previos

Para usar la administración de derechos de Azure AD (versión preliminar), debe tener una de las licencias siguientes:

- Azure AD Premium P2
- Licencia de Enterprise Mobility + Security (EMS) E5

Si no tiene una licencia Azure AD Premium P2 o Enterprise Mobility + Security E5, cree una [prueba de Enterprise Mobility + Security E5](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1) gratuita.

## <a name="step-1-set-up-users-and-group"></a>Paso 1: Configuración de usuarios y grupos

Un directorio de recurso tiene uno o más recursos para compartir. En este paso, creará un grupo denominado **Grupo de ingeniería** en el directorio de Woodgrove Bank que es el recurso de destino de la administración de derechos. También configurará un solicitante interno.

**Rol necesario:** administrador global o administrador de usuarios.

![Creación de usuarios y grupos](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global o administrador de usuarios.  

1. En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

1. Cree o configure los siguientes dos usuarios. Puede usar estos nombres u otros diferentes. **Admin1** puede ser el usuario con el que inició sesión.

    | NOMBRE | Rol del directorio | DESCRIPCIÓN |
    | --- | --- | --- |
    | **Admin1** | Administrador global<br/>O bien<br/>Administrador limitado (Administrador de usuarios) | Administrador y aprobador |
    | **Solicitante1** | Usuario | Solicitante interno |

    En este tutorial, el administrador y el aprobador son la misma persona, pero normalmente se designan una o varias personas como aprobadores.

1. Cree un grupo de seguridad de Azure AD llamado **Grupo de ingeniería** con un tipo de pertenencia de **Asignado**.

    Este grupo será el recurso de destino para la administración de derechos. El grupo debe estar vacío para comenzar.

## <a name="step-2-create-an-access-package"></a>Paso 2: Creación de un paquete de acceso

Un *paquete de acceso* es una agrupación de todos los recursos que necesita un usuario para trabajar en un proyecto o realizar su trabajo. Los paquetes de acceso se definen en contenedores llamados *catálogos*. En este paso, creará un **paquete de acceso del proyecto web** en el catálogo **General**.

**Rol necesario:** administrador global o administrador de usuarios.

![Creación de un paquete de acceso](./media/entitlement-management-access-package-first/elm-access-package.png)

1. En la barra de navegación izquierda de Azure Portal, haga clic en **Azure Active Directory**.

1. En el menú izquierdo, haga clic en **Gobernanza de identidades**.

1. En el menú izquierdo, haga clic en **Paquetes de acceso**.  Si ve **Acceso denegado**, asegúrese de que haya una licencia Azure AD Premium P2 en ese directorio.

1. Haga clic en **New access package** (Nuevo paquete acceso).

    ![Administración de derechos en Azure Portal](./media/entitlement-management-access-package-first/access-packages-list.png)

1. En la pestaña **Datos básicos**, escriba el nombre **Paquete de acceso del proyecto web** y la descripción **Paquete de acceso para el proyecto web de ingeniería**.

1. Deje la lista desplegable **Catálogo** establecida en **General**.

    ![New access package (Nuevo paquete de acceso): pestaña Datos básicos](./media/entitlement-management-access-package-first/basics.png)

1. Haga clic en **Siguiente** para abrir la pestaña **Resource roles** (Roles de recursos).

    En esta pestaña, seleccione los permisos que se incluirán en el paquete de acceso.

1. Haga clic en **Grupos**.

1. En el panel Seleccionar grupos, busque y seleccione el grupo **Grupo de ingeniería** que creó anteriormente.

    De forma predeterminada, verá grupos dentro y fuera del catálogo **General**. Al seleccionar un grupo fuera del catálogo **General**, se agregará a dicho catálogo.

    ![New access package (Nuevo paquete de acceso): pestaña Resource roles (Roles de recursos)](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

1. Haga clic en **Seleccionar** para agregar el grupo a la lista.

1. En la lista desplegable **Rol**, seleccione **Miembro**.

    ![New access package (Nuevo paquete de acceso): pestaña Resource roles (Roles de recursos)](./media/entitlement-management-access-package-first/resource-roles.png)

1. Haga clic en **Siguiente** para abrir la pestaña **Directiva**.

1. Establezca el botón de alternancia **Create first policy** (Crear primera directiva) en **Más tarde**.

    Creará la directiva en la siguiente sección.

    ![New access package (Nuevo paquete de acceso): pestaña Directiva](./media/entitlement-management-access-package-first/policy.png)

1. Haga clic en **Siguiente** para abrir la pestaña **Revisar y crear**.

    ![New access package (Nuevo paquete de acceso): pestaña Revisar y crear](./media/entitlement-management-access-package-first/review-create.png)

1. Revise la configuración del paquete de acceso y, luego, haga clic en **Crear**.

    Puede que vea un mensaje que dice que el paquete de acceso no estará visible para los usuarios porque todavía no está habilitado el catálogo.

    ![New access package (Nuevo paquete de acceso): mensaje de no visible](./media/entitlement-management-access-package-first/not-visible.png)

1. Haga clic en **OK**.

    Transcurridos unos instantes, verá una notificación que dice que el paquete de acceso se ha creado correctamente.

## <a name="step-3-create-a-policy"></a>Paso 3: Para crear una directiva

Una *directiva* define las reglas o barreras para acceder a un paquete de acceso. En este paso, creará una directiva que permite que un usuario específico del directorio del recurso solicite el paquete de acceso. También especificará que se deben aprobar las solicitudes y quién será el aprobador.

![Creación de una directiva de paquete de acceso](./media/entitlement-management-access-package-first/elm-access-package-policy.png)

**Rol necesario:** administrador global o administrador de usuarios.

1. En el menú izquierdo del **paquete de acceso del proyecto web**, haga clic en **Directivas**.

    ![Lista de directivas del paquete de acceso](./media/entitlement-management-access-package-first/policies-list.png)

1. Haga clic en **Agregar directiva** para abrir la pestaña Crear directiva.

1. Escriba el nombre **Directiva de solicitante interno** y la descripción **Permite que los usuarios de este directorio soliciten acceso a recursos del proyecto web**.

1. En la sección **Users who can request access** (Usuarios que pueden solicitar acceso), haga clic en **For users in your directory** (Para usuarios del directorio).

    ![Creación de directiva](./media/entitlement-management-access-package-first/policy-create.png)

1. Desplácese hacia abajo hasta la sección **Seleccionar usuarios y grupos** y haga clic en **Add users and groups** (Agregar usuarios y grupos).

1. En el panel Seleccionar usuarios y grupos, seleccione el usuario **Solicitante1** que creó anteriormente y, luego, haga clic en **Seleccionar**.

1. En la sección **Solicitar**, establezca **Requerir aprobación** en **Sí**.

1. En la sección **Seleccionar aprobadores**, haga clic en **Add approvers** (Agregar aprobadores).

1. En el panel Seleccionar aprobadores, seleccione el usuario **Admin1** que creó anteriormente y, luego, haga clic en **Seleccionar**.

    En este tutorial, el administrador y el aprobador son la misma persona, pero puede designar otra persona como aprobador.

1. En la sección **Expiración**, establezca **Access package expires** (El paquete de acceso expira) en **Número de días**.

1. Establezca **Access expires after** (El acceso expira después de) en **30** días.

1. En **Habilitar directiva**, haga clic en **Sí**.

    ![Creación de la configuración de directiva](./media/entitlement-management-access-package-first/policy-create-settings.png)

1. Haga clic en **Crear** para crear la **directiva del solicitante interno**.

1. En el menú izquierdo del paquete de acceso del proyecto web, haga clic en **Información general**.

1. Copie el **vínculo del portal Mi acceso**.

    Este vínculo lo usará en el paso siguiente.

    ![Información general del paquete acceso: vínculo del portal Mi acceso](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-4-request-access"></a>Paso 4: Solicitar acceso

En este paso, realizará los pasos como **solicitante interno** y solicitará acceso al paquete acceso. Los solicitantes envían sus solicitudes mediante un sitio conocido como el portal Mi acceso. El portal Mi acceso permite a los solicitantes enviar solicitudes de paquetes de acceso, ver los paquetes de acceso a los que ya tienen acceso y ver sus historiales de solicitudes.

**Rol necesario:** solicitante interno

1. Cierre sesión en Azure Portal.

1. En una nueva ventana del explorador, vaya al vínculo del portal Mi acceso que copió en el paso anterior.

1. Inicie sesión en el portal Mi acceso como **Solicitante1**.

    Verá el **paquete de acceso del proyecto web**.

1. Si es necesario, en la columna **Descripción**, haga clic en la flecha para ver detalles sobre el paquete de acceso.

    ![Portal Mi acceso: paquetes de acceso](./media/entitlement-management-shared/my-access-access-packages.png)

1. Haga clic en la marca de verificación para seleccionar el paquete.

1. Haga clic en **Solicitar acceso** para abrir el panel Solicitar acceso.

1. En el cuadro **Justificación comercial**, escriba la justificación **Trabajando en el proyecto web**.

1. Establezca el botón de conmutación **Request for specific period** (Solicitar período específico) en **Sí**.

1. Establezca **Fecha de inicio** en hoy y **Fecha de finalización** en mañana.

    ![Portal Mi acceso: solicitud de acceso](./media/entitlement-management-shared/my-access-request-access.png)

1. Haga clic en **Enviar**.

1. En el menú izquierdo, haga clic en **Historial de solicitudes** para comprobar que la solicitud se ha enviado.

## <a name="step-5-approve-access-request"></a>Paso 5: Aprobación de la solicitud de acceso

En este paso, inicie sesión como usuario **aprobador** y apruebe la solicitud de acceso del solicitante interno. Los aprobadores usan el mismo portal Mi acceso que los solicitantes para enviar solicitudes. Mediante el portal Mi acceso, los aprobadores pueden ver las aprobaciones pendientes y aprobar o rechazar solicitudes.

**Rol necesario:** Aprobador

1. Cierre sesión en el portal Mi acceso.

1. Inicie sesión en el [portal Mi acceso](https://myaccess.microsoft.com) como **Admin1**.

1. En el menú izquierdo, haga clic en **Aprobaciones**.

1. En la pestaña **Pendientes**, busque **Solicitante1**.

    Si no ve la solicitud del Solicitante1, espere unos minutos y vuelva a intentarlo.

1. Haga clic en el vínculo **Ver** para abrir el panel Solicitud de acceso.

1. Haga clic en **Approve** (Aprobar).

1. En el cuadro **Causa**, escriba la causa **Acceso aprobado para proyecto web**.

    ![Portal Mi acceso: solicitud de acceso](./media/entitlement-management-shared/my-access-approve-request.png)

1. Haga clic en **Enviar** para enviar su decisión.

    Verá un mensaje que dice que se ha aprobado correctamente.

## <a name="step-6-validate-that-access-has-been-assigned"></a>Paso 6: Validación de que se ha asignado el acceso

Ahora que ha aprobado la solicitud de acceso, en este paso, confirme que se ha asignado el paquete de acceso al **solicitante interno** y que este es ahora miembro del grupo **Grupo de ingeniería**.

**Rol necesario:** administrador global o administrador de usuarios.

1. Cierre sesión en el portal Mi acceso.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con el usuario **Admin1**.

1. Haga clic en **Azure Active Directory** y, luego, haga clic en **Gobernanza de identidades**.

1. En el menú izquierdo, haga clic en **Paquetes de acceso**.

1. Busque el **paquete de acceso del proyecto web** y haga clic en él.

1. En el menú izquierdo, haga clic en **Solicitudes**.

    Verá el Solicitante1 y la directiva del solicitante interno con el estado **Entregado**.

1. Haga clic en la solicitud para ver los detalles.

    ![Paquete de acceso: detalles de la solicitud](./media/entitlement-management-access-package-first/request-details.png)

1. En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

1. Haga clic en **Grupos** y abra el grupo **Grupo de ingeniería**.

1. Haga clic en **Miembros**.

    Verá que **Solicitante1** aparece como miembro.

    ![Miembros del grupo de ingeniería](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-7-clean-up-resources"></a>Paso 7: Limpieza de recursos

En este paso, quitará los cambios realizados y eliminará el paquete de acceso **Paquete de acceso del proyecto web**.

**Rol necesario:**  administrador global o administrador de usuarios.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. Abra **Paquete de acceso del proyecto web**.

1. Haga clic en **Asignaciones**.

1. En **Solicitante1**, haga clic en el botón de puntos suspensivos ( **...** ) y, luego, en **Quitar acceso**.

    El estado cambiará de Entregado a Expirado.

1. Haga clic en **Directivas**.

1. En **Internal requestor policy** (Directiva del solicitante interno), haga clic en los puntos suspensivos ( **...** ) y, luego, en **Eliminar**.

1. Haga clic en **Roles de recursos**.

1. En **Grupo de ingeniería**, haga clic en el botón de puntos suspensivos ( **...** ) y, luego, en **Remove resource role** (Quitar rol de recursos).

1. Abra la lista de paquetes de acceso.

1. En **Paquete de acceso del proyecto web**, haga clic en los puntos suspensivos ( **...** ) y, luego, en **Eliminar**.

1. En Azure Active Directory, elimine los usuarios creados como **Solicitante1** y **Admin1**.

1. Elimine el grupo **Grupo de ingeniería**.

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo para conocer los pasos del escenario común de administración de derechos.
> [!div class="nextstepaction"]
> [Escenarios comunes](entitlement-management-scenarios.md)
