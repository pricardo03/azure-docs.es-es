---
title: 'Azure Active Directory Domain Services: actualización de la configuración DNS para Azure Virtual Network | Microsoft Docs'
description: Introducción a Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: f683eeee05f264ca239b8f1da2bc5078e0146a17
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503492"
---
# <a name="enable-azure-active-directory-domain-services"></a>Habilitación de Azure Active Directory Domain Services

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Tarea 4: Actualización de la configuración DNS para la red virtual de Azure
En las tareas de configuración anteriores, ha habilitado correctamente Azure Active Directory Domain Services para el directorio. A continuación, permita a los equipos dentro de la red virtual que se conecten y consuman estos servicios. En este artículo, se actualiza la configuración del servidor DNS de la red virtual para que apunte a las direcciones IP en las que Azure Active Directory Domain Services está disponible en la red virtual.

Para actualizar la configuración del servidor DNS en la red virtual en la que ha habilitado Azure Active Directory Domain Services, realice los siguientes pasos:


1. En la pestaña **Información general** se muestra un conjunto de **pasos de configuración obligatorios** que se deben realizar después de que el dominio administrado está completamente aprovisionado. El primer paso de configuración es **Actualizar la configuración de servidores DNS de su red virtual**.

    ![Servicios de dominio - Pestaña Información general](./media/getting-started/domain-services-provisioned-overview.png)

    > [!TIP]
    > ¿No ve este paso de configuración? Si la configuración del servidor DNS para la red virtual está actualizada, no verá el icono "Actualizar la configuración de servidores DNS de su red virtual" en la pestaña Información general.
    >
    >

2. Haga clic en el botón **Configurar** para actualizar la configuración del servidor DNS de la red virtual.

> [!NOTE]
> Las máquinas virtuales de la red solo obtendrán la nueva configuración DNS después de reiniciarse. Si necesita tener la configuración de DNS actualizada inmediatamente, desencadene un reinicio mediante el portal, PowerShell o la CLI.
>
>

## <a name="next-step"></a>Paso siguiente
[Tarea 5: habilitación de la sincronización de los valores hash de las contraseñas con Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
