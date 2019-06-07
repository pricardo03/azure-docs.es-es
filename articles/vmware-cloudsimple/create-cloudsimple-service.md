---
title: Crear solución de VMware CloudSimple - servicio de Azure
description: Describe cómo crear el servicio CloudSimple en el portal de Azure
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fc08ce48b3b5a4b2cd28cb66e6dd4d9eae3f3885
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676959"
---
# <a name="create-azure-vmware-solution-by-cloudsimple---service"></a>Crear solución de VMware de Azure por CloudSimple - servicio

Para empezar a trabajar con soluciones de VMware de Azure por CloudSimple, cree la solución de VMware de Azure por servicio CloudSimple en el portal de Azure.

> [!NOTE]
> Antes de crear el servicio CloudSimple, debe registrar el proveedor de recursos Microsoft.VMwareCloudSimple en su suscripción de Azure. Siga los pasos de [habilitar el proveedor de recursos Microsoft.VMwareCloudSimple en su suscripción de Azure](enable-cloudsimple-service.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-the-service"></a>Creación del servicio

1. Seleccione **Todos los servicios**.

2. Busque **CloudSimple servicios**.

    ![Search CloudSimple Service](media/create-cloudsimple-service-search.png)

3. Seleccione **CloudSimple servicios**.

4. Haga clic en **agregar** para crear un nuevo servicio.

    ![Agregar servicio CloudSimple](media/create-cloudsimple-service-add.png)

5. Seleccione la suscripción en la que desea crear el servicio CloudSimple.

6. Seleccione el grupo de recursos para el servicio. Para agregar un nuevo grupo de recursos, haga clic en **crear nuevo**.

7. Escriba el nombre para identificar el servicio.

8. Escriba el CIDR de la puerta de enlace de servicio. Especifique una de/28 subred que no se superponga con cualquiera de las subredes existentes.  Estos incluyen subredes locales, las subredes de Azure, o cualquier planeado CloudSimple subredes. No se puede cambiar el CIDR después de crear el servicio.

    ![Creando el servicio CloudSimple](media/create-cloudsimple-service.png)

9. Haga clic en **OK**.

El servicio se crea y se agrega a la lista de servicios.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Obtenga información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)
