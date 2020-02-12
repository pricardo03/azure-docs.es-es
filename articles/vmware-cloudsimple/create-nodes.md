---
title: 'Aprovisionamiento de nodos de VMware Solutions (AVS): Azure'
description: Obtenga información sobre cómo agregar nodos a VMWare con la implementación de AVS.
author: dikamath
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 94ca681153f1e3ccd9ff628b41a9d0e547be5802
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024813"
---
# <a name="provision-nodes-for-azure-vmware-solutions-avs"></a>Aprovisionamiento de nodos de Azure VMware Solutions (AVS)

Aprovisione nodos en Azure Portal. Después puede configurar la capacidad de pago por uso de su entorno de nube privada de AVS.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="add-a-node-to-your-avs-private-cloud"></a>Incorporación de un nodo a la nube privada de AVS

1. Seleccione **Todos los servicios**.
2. Busque **AVS Nodes** (Nodos de AVS).

   ![Buscar nodos de AVS](media/create-cloudsimple-node-search.png)

3. Seleccione **AVS Nodes** (Nodos de AVS).
4. Haga clic en **Agregar** para crear los nodos.

    ![Agregar nodos de AVS](media/create-cloudsimple-node-add.png)

5. Seleccione la suscripción en la que quiere aprovisionar los nodos de AVS.
6. Seleccione el grupo de recursos de los nodos. Para agregar un nuevo grupo de recursos, haga clic en **Crear nuevo**.
7. Escriba el prefijo para identificar los nodos.
8. Seleccione la ubicación de los recursos de los nodos.
9. Seleccione la ubicación dedicada en la que hospedar los recursos de los nodos.
10. Seleccione el [tipo de nodo](cloudsimple-node.md).
11. Seleccione el número de nodos que quiere aprovisionar.
12. Seleccione **Revisar + crear**.
13. Revise la configuración. Para modificar la configuración, haga clic en **Anterior**.
14. Seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una nube privada de AVS](create-private-cloud.md)
