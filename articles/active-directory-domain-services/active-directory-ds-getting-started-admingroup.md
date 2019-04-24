---
title: 'Azure Active Directory Domain Services: Introducción | Microsoft Docs'
description: Habilitación de Azure Active Directory Domain Services mediante Azure Portal
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ergreenl
ms.openlocfilehash: 734fb5ce641d48800cef68ea79cdb258e44ac267
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60417764"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Habilitación de Azure Active Directory Domain Services mediante Azure Portal

## <a name="task-3-configure-administrative-group"></a>Tarea 3: Configuración de un grupo administrativo

En esta tarea de configuración, se crea un grupo administrativo en el directorio de Azure AD. Este grupo administrativo especial se llama *Administradores de DC de AAD*. A los miembros de este grupo se les conceden permisos administrativos en máquinas que estén unidas al domino administrado. En equipos unidos a un dominio, este grupo se agrega al grupo de administradores. Además, los miembros de este grupo también podrá usar Escritorio remoto para conectarse de forma remota a las máquinas unidas a un dominio.

> [!NOTE]
> No tiene permisos de administrador de dominio o administrador de organización en el dominio administrado creado mediante Azure Active Directory Domain Services. En los dominios administrados, estos permisos están reservados por el servicio y no están disponibles para los usuarios del inquilino. Sin embargo, puede usar el grupo administrativo especial creado en esta tarea de configuración para realizar algunas operaciones con privilegios. Estas operaciones incluyen unir equipos al dominio, formar parte del grupo de administradores en los equipos unidos a un dominio y configurar una directiva de grupo.
>

El asistente crea automáticamente el grupo administrativo en el directorio de Azure AD. Este grupo se llama "Administradores de DC de AAD". Si tiene un grupo existente con este nombre en el directorio de Azure AD, el asistente lo selecciona. Puede configurar la pertenencia a grupos mediante la página **Grupo de administradores** del asistente.

1. Para configurar la pertenencia a grupos, haga clic en **Administradores de DC de AAD**.

    ![Configuración de la pertenencia a grupos](./media/getting-started/domain-services-blade-admingroup.png)

2. Haga clic en el botón **Agregar miembros** para agregar usuarios del directorio de Azure AD al grupo de administradores.

3. Cuando haya terminado, haga clic en **Aceptar** para ir a la página **Resumen** del asistente.

## <a name="configure-synchronization"></a>Configuración de la sincronización

Azure AD Domain Services permite la sincronización completa de todos los usuarios y grupos disponibles en Azure AD, o seleccionar la sincronización con ámbito para sincronizar solo grupos específicos. Si elige la sincronización completa, **no** podrá elegir la sincronización con ámbito en un momento posterior. Para obtener más información sobre la sincronización con ámbito, visite el artículo[de sincronización con ámbito de Azure AD Domain Services](active-directory-ds-scoped-synchronization.md).

### <a name="full-synchronization"></a>Sincronización completa

1. Para la sincronización completa, simplemente haga clic en "Aceptar" en la parte inferior de la pantalla, ya que la completa está ya seleccionada.
    ![Sincronización completa](./media/active-directory-domain-services-admin-guide/create-sync-all.PNG)

### <a name="scoped-synchronization"></a>Sincronización con ámbito

1. Active el botón de sincronización en "Ámbito" y aparecerá una página para seleccionar grupos. Aquí puede ver qué grupos ya se han seleccionado para sincronizarse con el dominio administrado.
    ![Sincronización con ámbito](media/active-directory-domain-services-admin-guide/create-sync-scoped.PNG)
2. Haga clic en **Seleccionar grupos** en la barra de navegación superior. Se abrirá un selector de grupos en el lateral. Úselo para seleccionar grupos adicionales para sincronizar con Azure AD Domain Services. Cuando termine, haga clic en **Seleccionar** para cerrar el selector de grupos y agregar los grupos a la lista seleccionada.
    ![Selección de grupos en la sincronización con ámbito](media/active-directory-domain-services-admin-guide/create-sync-scoped-groupselect.PNG)
3. Haga clic en **Aceptar** para avanzar a la página de resumen.

## <a name="deploy-your-managed-domain"></a>Implementación del dominio administrado

1. En la página **Resumen** del asistente, revise la configuración del dominio administrado. Puede volver a cualquier paso del asistente para realizar cambios, si es necesario. Cuando haya terminado, haga clic en **Aceptar** para crear el dominio administrado.

    ![Resumen](./media/getting-started/domain-services-blade-summary.png)

2. Verá una notificación que muestra el progreso de la implementación de la instancia de Azure AD Domain Services. Haga clic en la notificación para ver el progreso detallado de la implementación.

    ![Notificación: implementación en curso](./media/getting-started/domain-services-blade-deployment-in-progress.png)

## <a name="check-the-deployment-status-of-your-managed-domain"></a>Comprobación del estado de implementación del dominio administrado

El proceso de aprovisionamiento del dominio administrado puede tardar hasta una hora.

1. Mientras la implementación está en curso, puede buscar "servicios de dominio" en el cuadro de búsqueda **buscar recursos**. En el resultado de la búsqueda, seleccione **Azure AD Domain Services**. La hoja **Azure AD Domain Services** muestra el dominio administrado que se está aprovisionando.

    ![Búsqueda del dominio administrado que se va aprovisionar](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Haga clic en el nombre del dominio administrado (por ejemplo, "contoso100.com") para ver más detalles sobre el dominio administrado.

    ![Domain Services: estado de aprovisionamiento](./media/getting-started/domain-services-provisioning-state.png)

3. La pestaña **Información** muestra que el dominio administrado se está aprovisionando. No se puede configurar el dominio administrado hasta que está aprovisionado por completo. El dominio administrado puede tardar una hora en aprovisionarse por completo.

    ![Domain Services: pestaña Información general durante el estado de aprovisionamiento](./media/getting-started/domain-services-provisioning-state-details.png)

4. Cuando el dominio administrado está aprovisionado por completo, la pestaña **Información general** muestra el estado del dominio como **En ejecución**.

    ![Domain Services: pestaña Información general después del aprovisionamiento completo](./media/getting-started/domain-services-provisioned.png)
    >[!NOTE]
    >Durante el proceso de aprovisionamiento, Azure AD Domain Services crea las aplicaciones empresariales "Domain Controller Services" y "AzureActiveDirectoryDomainControllerServices" dentro de su directorio. Estas aplicaciones empresariales se necesitan para dar servicio al dominio administrado. Es imprescindible que no se eliminen en ningún momento.
    >

5. En la pestaña **Propiedades**, se ven dos direcciones IP en las que hay controladores de dominio disponibles para Virtual Network.

    ![Domain Services: pestaña Propiedades después del aprovisionamiento completo](./media/getting-started/domain-services-provisioned-properties.png)

## <a name="need-help"></a>¿Necesita ayuda?

Puede tardar una hora o dos para ambos controladores de dominio para aprovisionar el dominio administrado. Si se produjo un error en la implementación o se detuvo en el estado "Pendiente" durante más de dos horas, [póngase en contacto con el equipo del producto para obtener ayuda](active-directory-ds-contact-us.md).

## <a name="next-step"></a>Paso siguiente

[Tarea 4: Actualización de la configuración DNS en Azure Virtual Network](active-directory-ds-getting-started-dns.md)
