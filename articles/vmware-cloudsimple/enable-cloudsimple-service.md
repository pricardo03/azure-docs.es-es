---
title: Habilitación la solución de VMware en Azure por CloudSimple
description: Describe cómo habilitar el servicio CloudSimple en una suscripción de Azure y, a continuación, cómo registrar el proveedor de recursos de CloudSimple.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2553aa95d5028c510b4e1a1b7f51a9f410bcea51
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154851"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Registro del proveedor de recursos Microsoft.VMwareCloudSimple en la suscripción de Azure

El servicio CloudSimple le permite consumir la solución de VMware en Azure de CloudSimple. Puede registrar el servicio Microsoft.VMwareCloudSimple como proveedor de recursos.

## <a name="register-the-resource-provider"></a>Registrar el proveedor de recursos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Seleccione **Todos los servicios**.

3. Busque y seleccione **Suscripciones**.

    ![Selección de suscripciones](media/cloudsimple-service-select-subscriptions.png)

4. Seleccione la suscripción en la que quiera habilitar el servicio CloudSimple.

5. Haga clic en **Proveedores de recursos** en la suscripción.

6. Use **Microsoft.VMwareCloudSimple** para filtrar el proveedor de recursos.

7. Seleccione el proveedor de recursos y haga clic en **Registrar**.

    ![Registro del proveedor de recursos](media/cloudsimple-service-enable-resource-provider.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear un servicio CloudSimple](create-cloudsimple-service.md).
* Más información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)
