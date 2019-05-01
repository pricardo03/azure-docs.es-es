---
title: Solución de VMware de Azure por CloudSimple Quickstart - crear servicio
description: Aprenda a crear el servicio CloudSimple, adquiere nodos y reserva de nodos
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9b3b95db24f4b0f9a0cf8f5102dfeea5dc51e29f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577576"
---
# <a name="quickstart---create-service"></a>Inicio rápido: creación de servicio

Para empezar, cree la solución Azure VMware CloudSimple en Azure portal.

## <a name="vmware-solution-by-cloudsimple---service-overview"></a>Solución de VMware CloudSimple - información general del servicio

El servicio CloudSimple permite utilizar la solución de VMware de Azure por CloudSimple.  Creación del servicio le permite adquirir nodos, reserva de nodos y crear nubes privadas.  Agregue el servicio CloudSimple en cada región de Azure donde está disponible el servicio CloudSimple.  El servicio define la red perimetral de la solución de VMware de Azure por CloudSimple.  Esta red perimetral se usa para los servicios que incluyen una conectividad VPN, ExpressRoute e Internet a sus nubes privadas.

Para agregar el servicio CloudSimple, debe crear una subred de puerta de enlace. La subred de puerta de enlace se usa al crear la red perimetral y requiere una de/28 bloque CIDR. El espacio de direcciones de subred de puerta de enlace debe ser único. No puede superponerse a ninguno de los espacios de direcciones de red local o el espacio de direcciones de red virtual de Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="enable-microsoftvmwarecloudsimple-resource-provider"></a>Habilitar a proveedor de recursos Microsoft.VMwareCloudSimple

Siga los pasos siguientes para habilitar al proveedor de recursos de servicio CloudSimple.

1. Seleccione **Todos los servicios**.
2. Busque y seleccione **suscripciones**.

    ![Selección de suscripciones](media/cloudsimple-service-select-subscriptions.png)

3. Seleccione la suscripción en el que desea habilitar el servicio CloudSimple
4. Haga clic en **proveedores de recursos** para la suscripción
5. Use **Microsoft.VMwareCloudSimple** para filtrar el proveedor de recursos
6. Seleccione el **Microsoft.VMwareCloudSimple** proveedor de recursos y haga clic en **registrar**

    ![Registro del proveedor de recursos](media/cloudsimple-service-enable-resource-provider.png)

## <a name="create-the-service"></a>Creación del servicio

>[!NOTE]
> Servicio de CloudSimple debe estar habilitado en su suscripción. Si no habilita la suscripción, recibirá un error al intentar crear el servicio.  Siga los pasos de [CloudSimple Habilitar servicio](https://docs.azure.cloudsimple.com/enable-cloudsimple-service) artículo para habilitar el servicio.

1. Seleccione **Todos los servicios**.
2. Busque **CloudSimple servicio**.

    ![Search CloudSimple Service](media/create-cloudsimple-service-search.png)

3. Seleccione **CloudSimple servicios**.
4. Haga clic en **agregar** para crear un nuevo servicio.

    ![Agregar servicio CloudSimple](media/create-cloudsimple-service-add.png)

5. Seleccione la suscripción en la que desea crear el servicio CloudSimple.
6. Seleccione el grupo de recursos para el servicio. Para agregar un nuevo grupo de recursos, haga clic en **crear nuevo**.
7. Escriba el nombre para identificar el servicio.
8. Escriba el CIDR de la puerta de enlace de servicio. Especifique una de/28 subred que no se superponga con ninguno de sus subredes locales, las subredes de Azure o planeada CloudSimple subredes. No se puede cambiar el CIDR después de crear el servicio.

    ![Creando el servicio CloudSimple](media/create-cloudsimple-service.png)

9. Haga clic en **OK**.

El servicio se crea y se agrega a la lista de servicios.

## <a name="purchase-nodes"></a>Nodos de la compra

Para configurar el pago a medida que vaya capacidad para un entorno de nube privada CloudSimple, aprovisionar los nodos en el portal de Azure.

1. Seleccione **Todos los servicios**.
2. Busque **CloudSimple nodos**.

    ![Búsqueda CloudSimple nodos](media/create-cloudsimple-node-search.png)

3. Seleccione **CloudSimple nodos**.
4. Haga clic en **agregar** para crear los nodos.

    ![Agregar nodos CloudSimple](media/create-cloudsimple-node-add.png)

5. Seleccione la suscripción que desea comprar CloudSimple nodos.
6. Seleccione el grupo de recursos para los nodos. Para agregar un nuevo grupo de recursos, haga clic en **crear nuevo**.
7. Escriba el prefijo para identificar los nodos.
8. Seleccione la ubicación de los recursos del nodo.
9. Seleccione la ubicación dedicada para hospedar los recursos del nodo.
10. Seleccione el tipo de nodo. Puede elegir el [opción CS28 o CS36](cloudsimple-node.md). Esta última opción incluye la capacidad de proceso y memoria máxima.
11. Seleccione el número de nodos para aprovisionar.
12. Seleccione **Revisar + crear**.
13. Revise la configuración. Para modificar la configuración, haga clic en **anterior**.
14. Seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de la nube privada y configurar el entorno](quickstart-create-private-cloud.md)
* Obtenga más información sobre [CloudSimple servicio](https://docs.azure.cloudsimple.com/cloudsimple-service)
