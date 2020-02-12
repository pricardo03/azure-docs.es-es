---
title: 'Azure VMware Solutions (AVS): acceso al cliente de vSphere'
description: Describe cómo acceder a vCenter desde la nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ad018ea89b194d42ab1867a0569725c4c3680f7d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022671"
---
# <a name="access-your-avs-private-cloud-vcenter-portal"></a>Acceso al portal de vCenter de la nube privada de AVS

Puede iniciar el portal de vCenter de la nube privada de AVS desde Azure Portal o el portal de AVS. El portal de vCenter le permite administrar la infraestructura de VMware en la nube privada de AVS.

## <a name="before-you-begin"></a>Antes de empezar

Debe establecerse la conexión de red y la resolución de nombres DNS debe estar habilitada para acceder al portal de vCenter. Puede establecer la conexión de red a la nube privada de AVS con cualquiera de las opciones siguientes.

* [Conexión desde una red local a AVS mediante ExpressRoute](on-premises-connection.md)
* [Configuración de una conexión VPN a la nube privada de AVS](set-up-vpn.md)

Para configurar la resolución de nombres DNS de los componentes de la infraestructura de VMware de la nube privada de AVS, consulte [Configuración de DNS para la resolución de nombres en el acceso a vCenter de la nube privada de AVS desde estaciones de trabajo locales](on-premises-dns-setup.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-vcenter-from-azure-portal"></a>Acceso a vCenter desde Azure Portal

Puede iniciar el portal de vCenter de la nube privada de AVS desde Azure Portal.

1. Seleccione **Todos los servicios**.

2. Busque **AVS Services** (Servicios de AVS).

3. Seleccione el servicio de AVS de la nube privada de AVS al que desea conectarse.

4. En la página **Información general**, haga clic en **View VMware AVS Private Clouds** (Ver nubes privadas de AVS de VMware).

    ![Información general del servicio de AVS](media/cloudsimple-service-overview.png)

5. Seleccione la nube privada de AVS en la lista y, después, haga clic en **Iniciar el cliente de vSphere**.

    ![Inicio del cliente vSphere](media/cloudsimple-service-launch-vsphere-client.png)

## <a name="access-vcenter-from-avs-portal"></a>Acceso a vCenter desde el portal de AVS

Puede iniciar el portal de vCenter de la nube privada de AVS desde el portal de AVS.

1. Acceda al [portal de AVS](access-cloudsimple-portal.md).

2. En **Recursos** seleccione la nube privada de AVS a la que desea acceder y haga clic en **Iniciar el cliente de vSphere**.

    ![Iniciar cliente de vSphere: Recursos](media/cloudsimple-portal-resources-launch-vcenter.png)

3. También puede iniciar el portal de vCenter desde la pantalla de resumen de la nube privada de AVS.

    ![Iniciar cliente de vSphere: Resumen](media/cloudsimple-resources-summary-launch-vcenter.png)

## <a name="next-steps"></a>Pasos siguientes

* [Creación y administración de redes VLAN y subredes para las nubes privadas de AVS](create-vlan-subnet.md)
* [Modelo de permisos de la nube privada de AVS de VMware vCenter](learn-private-cloud-permissions.md)