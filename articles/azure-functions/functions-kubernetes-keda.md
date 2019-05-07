---
title: Azure Functions en Kubernetes con KEDA
description: Aprender a ejecutar Azure Functions en Kubernetes en la nube o local mediante KEDA, escalado automático basada en eventos basados en Kubernetes.
services: functions
documentationcenter: na
author: jeffhollan
manager: jeconnoc
keywords: Azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor, kubernetes
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: jehollan
ms.openlocfilehash: c82ed7aa841f53f5c81f3281ed1b09926e565e75
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077626"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>Azure Functions en Kubernetes con KEDA

El tiempo de ejecución de Azure Functions proporciona flexibilidad en el hospedaje de dónde y cómo desea.  [KEDA](https://github.com/kedacore/kore) (basado en Kubernetes evento controlado por el escalado automático) de pares sin problemas con el tiempo de ejecución de Azure Functions y herramientas para proporcionar la escala basada en eventos de Kubernetes.

## <a name="how-kubernetes-based-functions-work"></a>Funciones del trabajo cómo basado en Kubernetes

El servicio de Azure Functions está formado por dos componentes clave: un tiempo de ejecución y un controlador de escala.  El tiempo de ejecución de Functions ejecuta y el código.  El tiempo de ejecución incluye lógica sobre cómo desencadenar, registrar y administrar las ejecuciones de función.  El otro componente es un controlador de escala.  El controlador de escala supervisa la tasa de eventos que va a dirigir la función y escala de forma proactiva el número de instancias que ejecutan la aplicación.  Para obtener más información, consulte [escalado y hospedaje Azure Functions](functions-scale.md).

Las funciones basadas en Kubernetes proporciona el tiempo de ejecución de funciones en un [contenedor de Docker](functions-create-function-linux-custom-image.md) controlado por eventos ajuste de escala a través de KEDA.  KEDA puede reducir verticalmente a 0 instancias (cuando no se produce ningún evento) y hasta *n* instancias. Para ello mediante la exposición de las métricas personalizadas del autoscaler Kubernetes (Horizontal Pod Escalador automático).  Uso de contenedores de funciones con KEDA hace posible replicar las capacidades de la función sin servidor en cualquier clúster de Kubernetes.  Estas funciones también se pueden implementar mediante [nodos virtuales de Azure Kubernetes Service (AKS)](../aks/virtual-nodes-cli.md) característica para la infraestructura sin servidor.

## <a name="managing-keda-and-functions-in-kubernetes"></a>Administración de KEDA y funciones en Kubernetes

Para ejecutar funciones en el clúster de Kubernetes, debe instalar el componente KEDA. Puede instalar este componente mediante [Azure Functions Core Tools](functions-run-local.md).

### <a name="installing-with-the-azure-functions-core-tools"></a>Instalación con Azure Functions Core Tools

De forma predeterminada, Core Tools instala componentes KEDA y Osiris, que admiten controlado por eventos y escalado de HTTP, respectivamente.  La instalación usa `kubectl` que se ejecuta en el contexto actual.

Instale KEDA en el clúster con el siguiente comando de instalación:

```cli
func kubernetes install --namespace keda
```

## <a name="deploying-a-function-app-to-kubernetes"></a>Implementar una aplicación de función en Kubernetes

Puede implementar cualquier aplicación de función en un clúster de Kubernetes KEDA.  Puesto que las funciones se ejecutan en un contenedor de Docker, su proyecto necesite un `Dockerfile`.  Si aún no tiene ninguna, puede agregar un archivo Dockerfile ejecutando el comando siguiente en la raíz de su proyecto de Functions:

```cli
func init --docker-only
```

Para crear una imagen e implementar las funciones en Kubernetes, ejecute el siguiente comando:

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> Reemplace `<name-of-function-deployment>` por el nombre de la aplicación de función.

Esto crea un Kubernetes `Deployment` recursos, un `ScaledObject` recursos, y `Secrets`, que incluye las variables de entorno que se importó desde su `local.settings.json` archivo.

## <a name="removing-a-function-app-from-kubernetes"></a>Quitar una aplicación de función de Kubernetes

Después de implementar puede quitar una función mediante la eliminación de asociado `Deployment`, `ScaledObject`, un `Secrets` creado.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Desinstalando KEDA de Kubernetes

Puede ejecutar el siguiente comando de las herramientas principales para quitar KEDA de un clúster de Kubernetes:

```cli
func kubernetes remove --namespace keda
```

## <a name="supported-triggers-in-keda"></a>Desencadenadores admitidos en KEDA

KEDA está actualmente en versión beta con compatibilidad con los siguientes desencadenadores de función de Azure:

* [Colas de Azure Storage](functions-bindings-storage-queue.md)
* [Colas de Service Bus](functions-bindings-service-bus.md)
* [HTTP](functions-bindings-http-webhook.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, consulte los siguientes recursos:

* [Creación de una función con una imagen personalizada](functions-create-function-linux-custom-image.md)
* [Codificación y comprobación de Azure Functions en un entorno local](functions-develop-local.md)
* [Cómo funciona el plan de consumo de la función de Azure](functions-scale.md)