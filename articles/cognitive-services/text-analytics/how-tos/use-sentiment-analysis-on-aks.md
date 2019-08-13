---
title: 'Ejecución de Azure Kubernetes Service: Text Analytics'
titleSuffix: Azure Cognitive Services
description: Implemente los contenedores de Text Analytics con la imagen de análisis de opiniones en Azure Kubernetes Service y pruébelos en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 44ee5fab5b4e8900b823453e5674fc9bdb5fe9ac
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552281"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-service"></a>Implementación de un contenedor de análisis de opiniones en Azure Kubernetes Service

Aprenda cómo implementar el contenedor de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) de Azure Cognitive Services con la imagen de análisis de opiniones en Azure Kubernetes Service (AKS). Este procedimiento muestra cómo crear un recurso de Text Analytics, cómo crear una imagen de análisis de opiniones asociada y cómo efectuar la orquestación de ambos elementos desde un explorador. El uso de contenedores puede desviar la atención de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

Este procedimiento requiere varias herramientas que se deben instalar y ejecutar localmente. No use Azure Cloud Shell. Necesita lo siguiente:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Un editor de texto; por ejemplo, [Visual Studio Code](https://code.visualstudio.com/download).
* La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalada.
* La [CLI de Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/) instalada.
* Un recurso de Azure con el plan de tarifa correcto. No todos los planes de tarifa funcionan con este contenedor:
    * El recurso **Azure Text Analytics** solo con los planes de tarifas Estándar o F0.
    * El recurso **Azure Cognitive Services** con el plan de tarifa S0.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics container on Azure Kubernetes Service (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-a-text-analytics-container-to-an-aks-cluster"></a>Implementación de un contenedor de Text Analytics en un clúster de AKS

1. Abra la CLI de Azure e inicie sesión en Azure.

    ```azurecli
    az login
    ```

1. Inicie sesión en el clúster de AKS. Reemplace `your-cluster-name` y `your-resource-group` por los valores adecuados.

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Después de la ejecución de este comando, se muestra un mensaje similar al siguiente:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Si tiene varias suscripciones disponibles en su cuenta de Azure y el comando `az aks get-credentials` devuelve un error, puede deberse a un problema común de uso de la suscripción incorrecta. Establezca el contexto de la sesión de la CLI de Azure para usar la misma suscripción con la que creó los recursos y vuelva a intentarlo.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Abra el editor de texto que prefiera. En este ejemplo se utiliza Visual Studio Code.

    ```azurecli
    code .
    ```

1. En el editor de texto, cree un nuevo archivo llamado _sentiment.yaml_ y pegue el siguiente código YAML en él. Asegúrese de reemplazar `billing/value` y `apikey/value` por su propia información.

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
1. Compruebe que el pod está implementado:

    ```console
    kubectl get pods
    ```

    La salida del estado de ejecución del pod:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Compruebe que el servicio está disponible y obtenga la dirección IP.

    ```console
    kubectl get services
    ```

    La salida del estado de ejecución del servicio de _opiniones_ en el pod:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the sentiment analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Pasos siguientes

* Uso de otros [contenedores de Cognitive Services](../../cognitive-services-container-support.md)
* Uso del [servicio conectado de Text Analytics](../vs-text-connected-service.md)
