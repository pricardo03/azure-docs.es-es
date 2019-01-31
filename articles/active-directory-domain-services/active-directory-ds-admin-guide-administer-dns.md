---
title: 'Azure Active Directory Domain Services: Administración de DNS en dominios administrados | Microsoft Docs'
description: Administración de DNS en dominios administrados de Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: d6705f9f7e324c915c38d01c54bdf16826c62380
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180201"
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Administración de DNS en un dominio administrado con Servicios de dominio de Azure AD
Azure Active Directory Domain Services incluye un servidor DNS (resolución de nombres de dominio) que proporciona una resolución DNS para el dominio administrado. En ocasiones, puede que necesite configurar DNS en el dominio administrado con el objetivo de crear registros DNS de máquinas que no están unidas al dominio, o configurar direcciones IP virtuales de los equilibradores de carga o reenviadores DNS externos. Por este motivo, se concede a los usuarios que pertenecen al grupo "Administradores del controlador de dominio de AAD" privilegios de administración de DNS en el dominio administrado.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Antes de empezar
Para completar las tareas de este artículo, necesitará lo siguiente:

1. Una **suscripción de Azure**válida.
2. Un **directorio de Azure AD** : sincronizado con un directorio local o solo en la nube.
3. **Servicios de dominio de Azure AD** deben estar habilitado en el directorio de Azure AD. Si no lo ha hecho, siga todas las tareas descritas en [Servicios de dominio de Azure AD (vista previa): introducción](active-directory-ds-getting-started.md).
4. Una **máquina virtual unida a un dominio** , desde la que administrará el dominio administrado de los Servicios de dominio de Azure AD. Si no tiene una máquina virtual, siga todas las tareas descritas en el artículo [Unión de una máquina virtual de Windows Server a un dominio administrado](active-directory-ds-admin-guide-join-windows-vm.md).
5. Necesitará las credenciales de una **cuenta de usuario que pertenezca al grupo "Administradores del controlador de dominio de AAD"** en el directorio con el fin de administrar DNS en el dominio administrado.

<br>

## <a name="task-1---create-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Tarea 1: Creación de una máquina virtual unida a un dominio para administrar de forma remota DNS en el dominio administrado
Los dominios administrados con Servicios de dominio de Azure AD se pueden controlar de forma remota mediante las conocidas herramientas administrativas de Active Directory, como el Centro de administración de Active Directory (ADAC) o AD PowerShell. De forma similar, DNS para el dominio administrado se puede administrar remotamente con las herramientas de administración del servidor DNS.

Los administradores de su directorio de Azure AD no tienen privilegios para conectarse a controladores de dominio en el dominio administrado a través del Escritorio remoto. Los miembros del grupo Administradores del controlador de dominio de AAD pueden administrar DNS para dominios administrados de forma remota mediante las herramientas del servidor DNS desde un equipo cliente/Windows Server que se ha unido al dominio administrado. Las herramientas de DNS Server forman parte de la característica opcional Herramientas de administración remota del servidor (RSAT).

El primer paso consiste en crear una máquina virtual Windows Server que esté unida al dominio administrado. Para ver instrucciones, consulte el artículo [Unión de una máquina virtual de Windows Server a un dominio administrado](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Tarea 2: Instalación de las herramientas del servidor DNS en la máquina virtual
Complete los pasos siguientes para instalar las herramientas de administración de DNS en la máquina virtual unida al dominio. Para más información sobre cómo [instalar y usar Herramientas de administración remota del servidor](https://technet.microsoft.com/library/hh831501.aspx), consulte TechNet.

1. Acceda a Azure Portal. Haga clic en **Todos los recursos** en el panel izquierdo. Busque y haga clic en la máquina virtual que creó en la tarea 1.
2. Haga clic en el botón **Conectar** en la pestaña Información general. Se crea y se descarga un archivo de Protocolo de Escritorio remoto (.rdp).

    ![Conexión a máquina virtual de Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Para conectarse a la máquina virtual, abra el archivo RDP descargado. Cuando se le solicite, haga clic en **Conectar**. Utilice las credenciales de un usuario que pertenezca al grupo "Administradores del controlador de dominio de AAD". Por ejemplo: 'bob@domainservicespreview.onmicrosoft.com'. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Haga clic en Sí o Continuar para conectarse.

4. En la pantalla Inicio, abra **Administrador del servidor**. Haga clic en **Agregar roles y características** en el panel central de la ventana Administrador del servidor.

    ![Inicio del Administrador del servidor en la máquina virtual](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. En la página **Antes de comenzar** del **Asistente para agregar roles y características**, haga clic en **Siguiente**.

    ![Página Antes de comenzar](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. En la página **Tipo de instalación**, deje la opción **Instalación basada en características o en roles** activada y haga clic en **Siguiente**.

    ![Página Tipo de instalación](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. En la página **Selección del servidor**, seleccione la máquina virtual actual del grupo de servidores y haga clic en **Siguiente**.

    ![Página Selección de servidor](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. En la página **Roles de servidor**, haga clic en **Siguiente**.
9. En la página **Características**, haga clic para expandir el nodo **Herramientas de administración remota del servidor** y, después, haga clic para expandir el nodo **Herramientas de administración de roles**. Seleccione la característica **Herramientas del servidor DNS** en la lista de herramientas de administración de roles.

    ![Página Características](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. En la página **Confirmación**, haga clic en **Instalar** para instalar la característica de herramientas del servidor DNS en la máquina virtual. Cuando finalice correctamente la instalación de la característica, haga clic en **Cerrar** para salir del **Asistente para agregar roles y características**.

    ![Página de confirmación](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Tarea 3: Inicio de la consola de administración de DNS para administrar DNS
Ahora, puede usar las herramientas de DNS de Windows Server para administrar DNS en el dominio administrado.

> [!NOTE]
> Tendrá que ser miembro del grupo Administradores del controlador de dominio de AAD para administrar DNS en el dominio administrado.
>
>

1. En la pantalla Inicio, haga clic en **Herramientas administrativas**. Debería ver la consola **DNS** instalada en la máquina virtual.

    ![Herramientas administrativas: consola DNS](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Haga clic en **DNS** para iniciar la consola de administración de DNS.
3. En el cuadro de diálogo **Conectar a servidor DNS**, haga clic en **El siguiente equipo** y escriba el nombre de dominio DNS del dominio administrado (por ejemplo, contoso100.com).

    ![Consola DNS: conexión al dominio](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. La consola DNS se conecta al dominio administrado.

    ![Consola DNS: administración del dominio](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. Ahora puede usar la consola DNS para agregar entradas de DNS para los equipos de la red virtual en la que ha habilitado Servicios de dominio de AAD.

> [!WARNING]
> Tenga cuidado al administrar DNS para el dominio administrado con las herramientas de administración de DNS. Asegúrese de **no eliminar ni modificar los registros DNS integrados que usan los Servicios de dominio en el dominio**. Los registros DNS integrados incluyen registros DNS de dominio, registros de servidor de nombres y otros registros usados para la ubicación del controlador de dominio. Si modifica estos registros, se interrumpirán los servicios de dominio en la red virtual.
>
>

Para obtener más información sobre la administración de DNS, consulte el artículo [Herramientas de DNS](https://technet.microsoft.com/library/cc753579.aspx)de TechNet.

## <a name="related-content"></a>Contenido relacionado
* [Introducción a Azure AD Domain Services](active-directory-ds-getting-started.md)
* [Unión de una máquina virtual de Windows Server a un dominio administrado](active-directory-ds-admin-guide-join-windows-vm.md)
* [Administración de un dominio administrado con Servicios de dominio de Azure AD](active-directory-ds-admin-guide-administer-domain.md)
* [Herramientas de DNS](https://technet.microsoft.com/library/cc753579.aspx)
