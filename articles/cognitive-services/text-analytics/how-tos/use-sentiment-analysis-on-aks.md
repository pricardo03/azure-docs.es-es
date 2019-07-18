---
title: Ejecución de Azure Kubernetes Services
titleSuffix: Text Analytics - Azure Cognitive Services
description: Implemente los contenedores de Text Analytics con la imagen de Análisis de sentimiento en Azure Kubernetes Services y pruébelos en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: a419ed3b9c0d2c4db9c552642dc5c662786f6730
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561248"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-services-aks"></a>Implementación de un contenedor de Análisis de sentimiento en Azure Kubernetes Services (AKS)

Aprenda cómo implementar el contenedor de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) de Cognitive Services con la imagen de Análisis de sentimiento en Azure Kubernetes Services (AKS). Este procedimiento ejemplifica la creación de un recurso de Text Analytics y la creación de una imagen de Análisis de sentimiento asociada, así como la posibilidad de utilizar esta orquestación de ambas acciones desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la gestión de la infraestructura y llevarles a centrarse en el desarrollo de aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

Este procedimiento requiere varias herramientas que se deben instalar y ejecutar localmente. No use Azure Cloud Shell.

* Use una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Use un editor de texto, como [Visual Studio Code](https://code.visualstudio.com/download)
* Instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Instale la [CLI de Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
* Un recurso de Azure con el plan de tarifa correcto. No todos los planes de tarifa funcionan con este contenedor:
    * El recurso **Text Analytics** solo con los planes de tarifas estándar o F0.
    * El recurso **Cognitive Services** con el plan de tarifas S0.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Kubernetes Services (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-text-analytics-container-to-an-aks-cluster"></a>Implementación del contenedor de Text Analytics en un clúster de AKS

1. Abra la CLI de Azure e inicie sesión en Azure.

    ```azurecli
    az login
    ```

1. Inicie sesión en el clúster de AKS (reemplace `your-cluster-name` y `your-resource-group` con los valores adecuados).

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Después de la ejecución de este comando, se muestra un mensaje similar al siguiente:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Si tiene varias suscripciones disponibles en su cuenta de Azure y el comando `az aks get-credentials` devuelve un error, puede deberse a un problema común de uso de la suscripción incorrecta. Basta con establecer el contexto de la sesión de la CLI de Azure para usar la misma suscripción con la que creó los recursos y volver a intentarlo.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Abra el editor de texto que prefiera, (en este ejemplo se usa __Visual Studio Code__):

    ```azurecli
    code .
    ```

1. En el editor de texto, cree un nuevo archivo denominado _sentiment.yaml_ y pegue el siguiente código YAML en él. Asegúrese de reemplazar `billing/value` y `apikey/value` por sus propios valores.

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: # < Your endpoint >
            - name: apikey
              value: # < Your API Key >
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. Guarde el archivo y cierre el editor de texto.
1. Ejecute el comando `apply` de Kubernetes con el archivo _sentiment.yaml_ como destino:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Una vez que el comando ha aplicado correctamente la configuración de implementación, se muestra un mensaje similar al siguiente:

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Compruebe que el POD esté implementado:

    ```console
    kubectl get pods
    ```

    Se mostrará el estado de ejecución del POD:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Compruebe que el servicio está disponible y obtiene la dirección IP:

    ```console
    kubectl get services
    ```

    Se mostrará el estado de ejecución del servicio de _opinión_ en el POD:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Pasos siguientes

* Uso de [Contenedores de Cognitive Services](../../cognitive-services-container-support.md)
* Uso del [servicio conectado de Text Analytics](../vs-text-connected-service.md)
