---
title: 'Tutorial: Escalado de un clúster de Red Hat OpenShift en Azure | Microsoft Docs'
description: Aprenda a escalar un clúster de Red Hat OpenShift en Microsoft Azure con la CLI de Azure
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/06/2019
ms.openlocfilehash: f86b60fbe20fc630863d6e177d45f2c1f06a7863
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079400"
---
# <a name="tutorial-scale-an-azure-red-hat-openshift-cluster"></a>Tutorial: Escalado de un clúster de Red Hat OpenShift en Azure

Este tutorial es la segunda parte de una serie. Aprenderá a crear un clúster de Red Hat OpenShift en Microsoft Azure con la CLI de Azure, escalarlo y eliminarlo para limpiar los recursos.

En la segunda parte de la serie, se aprende a:

> [!div class="checklist"]
> * Escalado de un clúster de Red Hat OpenShift

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md)
> * Escalado de un clúster de Red Hat OpenShift en Azure
> * [Eliminación de un clúster de Red Hat OpenShift en Azure](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Cree un clúster según las instrucciones del tutorial [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md).

## <a name="step-1-sign-in-to-azure"></a>Paso 1: Inicio de sesión en Azure

Si ejecuta la CLI de Azure de manera local, ejecute `az login` para iniciar sesión en Azure.

```bash
az login
```

Si tiene acceso a varias suscripciones, ejecute `az account set -s {subscription ID}` y reemplace `{subscription ID}` por la suscripción que quiere usar.

## <a name="step-2-scale-the-cluster-with-additional-nodes"></a>Paso 2: Escalado del clúster con nodos adicionales

Desde un terminal de Bash, establezca la variable CLUSTER_NAME en el nombre del clúster:

```bash
CLUSTER_NAME=yourclustername
```

Ahora vamos a escalar el clúster a cinco nodos con la CLI de Azure:

```bash
az openshift scale --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --compute-count 5
```

Después de unos minutos, `az openshift scale` se completará correctamente y devolverá un documento JSON con los detalles del clúster escalado.

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Escalado de un clúster de Red Hat OpenShift en Azure

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Eliminación de un clúster de Red Hat OpenShift en Azure](tutorial-delete-cluster.md)