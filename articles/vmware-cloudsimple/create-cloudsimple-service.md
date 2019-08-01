---
title: Creación de una solución de VMware en Azure por CloudSimple
description: Describe cómo crear el servicio CloudSimple en Azure Portal.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a0ccce6f298270b2751307868fdf85697cb7e8ee
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154966"
---
# <a name="create-azure-vmware-solution-by-cloudsimple---service"></a>Creación de una solución de VMware en Azure por CloudSimple

Para empezar a trabajar con la solución de VMware en Azure por CloudSimple, cree este servicio en Azure Portal.

> [!NOTE]
> Antes de crear el servicio CloudSimple, debe registrar el proveedor de recursos Microsoft.VMwareCloudSimple en la suscripción de Azure. Siga los pasos indicados en [Registrar el proveedor de recursos Microsoft.VMwareCloudSimple en su suscripción de Azure](enable-cloudsimple-service.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-the-service"></a>Creación del servicio

1. Seleccione **Todos los servicios**.

2. Busque **Servicios de CloudSimple**.

    ![Búsqueda del servicio CloudSimple](media/create-cloudsimple-service-search.png)

3. Seleccione **Servicios CloudSimple**.

4. Haga clic en **Agregar** para crear un servicio.

    ![Adición del servicio CloudSimple](media/create-cloudsimple-service-add.png)

5. Seleccione la suscripción en la que quiera crear el servicio CloudSimple.

6. Seleccione el grupo de recursos del servicio. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.

7. Escriba un nombre para identificar el servicio.

8. Escriba el CIDR de la puerta de enlace de servicio. Especifique una subred /28 que no se superponga con ninguna de sus subredes existentes.  Estas incluyen subredes locales, subredes de Azure o cualquier subred planeada de CloudSimple. No puede cambiar el CIDR después de crear el servicio.

    ![Creación del servicio CloudSimple](media/create-cloudsimple-service.png)

9. Haga clic en **OK**.

El servicio se crea y se agrega a la lista de servicios.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Más información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)
