---
title: Habilitar la solución de VMware de Azure por servicio CloudSimple
description: Describe cómo habilitar el servicio CloudSimple en una suscripción de Azure y, a continuación, registrar el proveedor de recursos CLoudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cacd5510147ce997efec922f4b4656956a098d88
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676944"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Registrar el proveedor de recursos Microsoft.VMwareCloudSimple en su suscripción de Azure

El servicio CloudSimple permite utilizar la solución de VMware de Azure por CloudSimple. Para usar el servicio CloudSimple, debe habilitarse primero en su suscripción de Azure. A continuación, puede registrar el servicio Microsoft.VMwareCloudSimple como el proveedor de recursos.

## <a name="enable-the-cloudsimple-service"></a>Habilitar el servicio CloudSimple

Para habilitar el servicio CloudSimple en su suscripción de Azure, abra una solicitud de soporte técnico con [soporte técnico de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Seleccione las opciones siguientes al enviar la solicitud.

* Tipo de problema: **Técnico**
* Suscripción: **El identificador de suscripción**
* Tipo de servicio: **Solución de VMware CloudSimple**
* Tipo de problema: **Cuota de nodos dedicado**
* Subtipo del problema: **Aumentar la cuota de nodos dedicados**
* Asunto: **Habilitar servicio CloudSimple**

También puede ponerse en contacto con su representante de cuenta de Microsoft en [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com). Especifique el identificador de suscripción de Azure en el correo electrónico.  

Después de habilita el servicio CloudSimple de su suscripción, puede habilitar al proveedor de recursos en la suscripción.

## <a name="register-the-resource-provider"></a>Registrar el proveedor de recursos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Seleccione **Todos los servicios**.

3. Busque y seleccione **suscripciones**.

    ![Selección de suscripciones](media/cloudsimple-service-select-subscriptions.png)

4. Seleccione la suscripción en el que desea habilitar el servicio CloudSimple.

5. Haga clic en **proveedores de recursos** para la suscripción.

6. Use **Microsoft.VMwareCloudSimple** para filtrar el proveedor de recursos.

7. Seleccione el proveedor de recursos y haga clic en **registrar**.

    ![Registro del proveedor de recursos](media/cloudsimple-service-enable-resource-provider.png)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear un servicio CloudSimple](create-cloudsimple-service.md)
* Obtenga información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)