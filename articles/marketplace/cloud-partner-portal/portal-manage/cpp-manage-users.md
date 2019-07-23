---
title: Administración de usuarios en Cloud Partner Portal | Azure Marketplace
description: Administración de usuarios en Cloud Partner Portal
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
manager: pbutlerm
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: pabutler
ms.openlocfilehash: 21c49ad0d73721dd204a7a3899d11cf10d43521b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942078"
---
# <a name="managing-users-on-the-cloud-partner-portal"></a>Administración de usuarios en Cloud Partner Portal

[Cloud Partner Portal](https://cloudpartner.azure.com) le permite agregar usuarios con acceso basado en roles a las ofertas de máquina virtual. Esto ayuda a administrar el acceso y los permisos mientras varias personas trabajan en las ofertas.

## <a name="add-users-and-assign-roles"></a>Incorporación de usuarios y asignación de roles 

Siga los pasos a continuación para agregar usuarios y asignar roles para la publicación de ofertas.

1. Haga clic en la pestaña **Usuarios** del panel de navegación izquierdo.

    ![Pestaña Usuarios](./media/userstab.png)


2. Haga clic en **Agregar usuario**.

    ![Selección de Agregar usuario](./media/adduser.png)


3. Escriba la dirección de correo electrónico del usuario y seleccione una asignación de roles.  Puede agregar el nuevo usuario como "propietario" o como "colaborador".

    **Correo electrónico**: agregue las direcciones de correo electrónico de los compañeros del equipo que vayan a trabajar en la publicación de la oferta. Se admiten cuentas de Microsoft (Outlook, Hotmail y Live) e identificadores de organización.

    - Agregue un "grupo de seguridad o alias de correo electrónico de equipo o grupo" para protegerse frente a una persona que abandona la organización.
    - Asegúrese de que se supervisan los identificadores de correo electrónico indicados en Cloud Partner Portal en caso de que lleguen comunicaciones de Microsoft.
    
    ![Asignación de roles](./media/assignrole.png)

    **Rol**: consulte la tabla siguiente para identificar el tipo de rol de usuario correspondiente.

    ![Nivel de roles](./media/roleaccesslevel.png)

    Solo los propietarios pueden acceder a las pestañas Azure Payouts (Pagos de Azure) y Azure Customer (Cliente de Azure) en [Seller Insights](../../cloud-partner-portal-orig/si-getting-started.md).


4. Seleccione el nombre del anunciante para el que quiere agregar el nuevo usuario y haga clic en Agregar para completar el proceso.

    > [!NOTE]
    > Los "nombres de anunciante" para los que el correo electrónico se agrega como propietario o colaborador se mostrarán en la lista desplegable de **Anunciante** para la selección.


    ![Asignar rol: seleccionar el anunciante](./media/assignselectpublisher.png)


## <a name="delete-an-existing-user"></a>Eliminar un usuario existente

Para quitar un usuario existente, busque el correo electrónico en la pestaña **Usuarios** y, luego, presione el botón **Eliminar** a la derecha.


## <a name="next-steps"></a>Pasos siguientes

También puede usar el portal para [administrar los perfiles de anunciante](./cpp-manage-publisher-profile.md).
