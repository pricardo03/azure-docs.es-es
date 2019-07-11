---
title: 'Tutorial: Eliminación de un clúster de Red Hat OpenShift en Azure | Microsoft Docs'
description: En este tutorial, aprenderá a eliminar un clúster de Red Hat OpenShift en Azure mediante la CLI de Azure
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.topic: tutorial
ms.service: container-service
ms.date: 05/06/2019
ms.openlocfilehash: 0ad70f4c3681705377a350fee8b02a55c526f26c
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67669332"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-cluster"></a>Tutorial: Eliminación de un clúster de Red Hat OpenShift en Azure

Éste es el final del tutorial. Cuando haya terminado de probar el clúster de ejemplo, así podrá eliminarlo y eliminar sus recursos asociados para que no se le cobre lo que no está usando.

En la tercera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Eliminación de un clúster de Red Hat OpenShift en Azure

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md)
> * [Escalado de un clúster de Red Hat OpenShift en Azure](tutorial-scale-cluster.md)
> * Eliminación de un clúster de Red Hat OpenShift en Azure

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Cree un clúster según las instrucciones del tutorial [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md).

## <a name="step-1-sign-in-to-azure"></a>Paso 1: Inicio de sesión en Azure

Si ejecuta la CLI de Azure de manera local, ejecute `az login` para iniciar sesión en Azure.

```bash
az login
```

Si tiene acceso a varias suscripciones, ejecute `az account set -s {subscription ID}` y reemplace `{subscription ID}` por la suscripción que quiere usar.

## <a name="step-2-delete-the-cluster"></a>Paso 2: Eliminación del clúster

Abra un terminal de Bash y establezca la variable CLUSTER_NAME en el nombre del clúster:

```bash
CLUSTER_NAME=yourclustername
```

Ahora elimine el clúster:

```bash
az openshift delete --resource-group $CLUSTER_NAME --name $CLUSTER_NAME
```

Se le preguntará si quiere eliminar el clúster. Después de confirmar con `y`, la eliminación del clúster tardará varios minutos. Cuando finalice el comando, se eliminará todo el grupo de recursos y los recursos dentro de él (incluido el clúster).

## <a name="deleting-a-cluster-using-the-azure-portal"></a>Eliminación de un clúster mediante Azure Portal

De manera alternativa, puede eliminar el grupo de recursos asociado del clúster a través de Azure Portal en línea. El nombre del grupo de recursos es igual que el nombre del clúster.

Actualmente, el recurso `Microsoft.ContainerService/openShiftManagedClusters` que se crea al crear el clúster está oculto en Azure Portal. En la vista `Resource group`, compruebe `Show hidden types` para ver el grupo de recursos.

![Captura de pantalla de la casilla de tipo oculto](./media/aro-portal-hidden-type.png)

Si elimina el grupo de recursos, se eliminarán todos los recursos relacionados que se crean al crear un clúster de Red Hat OpenShift en Azure.

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:
> [!div class="checklist"]
> * Eliminación de un clúster de Red Hat OpenShift en Azure

Más información sobre cómo usar OpenShift con la [documentación oficial de Red Hat OpenShift](https://docs.openshift.com/aro/welcome/index.html)