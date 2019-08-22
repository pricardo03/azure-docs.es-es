---
title: 'Aprovisionamiento de nodos de la solución de VMware de CloudSimple: Azure'
description: Obtenga información sobre cómo agregar nodos a VMWare con la implementación de CloudSimple.
author: dikamath
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2d5b2847109149701cb6453753e52fb671ba69d0
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533340"
---
# <a name="provision-nodes-for-vmware-solution-by-cloudsimple---azure"></a>Aprovisionamiento de nodos de la solución de VMware de CloudSimple: Azure

Aprovisione nodos en Azure Portal. Después puede configurar la capacidad de pago por uso de su entorno de nube privada de CloudSimple.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="add-a-node-to-your-cloudsimple-private-cloud"></a>Incorporación de un nodo a la nube privada de CloudSimple

1. Seleccione **Todos los servicios**.
2. Busque **Nodos CloudSimple**.

   ![Búsqueda de los nodos CloudSimple](media/create-cloudsimple-node-search.png)

3. Seleccione **Nodos CloudSimple**.
4. Haga clic en **Agregar** para crear los nodos.

    ![Adición de los nodos CloudSimple](media/create-cloudsimple-node-add.png)

5. Seleccione la suscripción en la que quiera aprovisionar los nodos CloudSimple.
6. Seleccione el grupo de recursos de los nodos. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.
7. Escriba el prefijo para identificar los nodos.
8. Seleccione la ubicación de los recursos de los nodos.
9. Seleccione la ubicación dedicada en la que hospedar los recursos de los nodos.
10. Seleccione el tipo de nodo. Puede elegir entre las [opciones CS28 o CS36](cloudsimple-node.md). Esta última opción incluye la capacidad de proceso y memoria máxima.
11. Seleccione el número de nodos que quiere aprovisionar.
12. Seleccione **Revisar + crear**.
13. Revise la configuración. Para modificar la configuración, haga clic en **Anterior**.
14. Seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

* [Create Private Cloud](create-private-cloud.md) (Creación de una nube privada)
