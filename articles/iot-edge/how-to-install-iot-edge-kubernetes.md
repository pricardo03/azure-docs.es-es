---
title: Cómo instalar IoT Edge en Kubernetes | Microsoft Docs
description: Obtenga información sobre cómo instalar IoT Edge en Kubernetes mediante un entorno de clústeres de desarrollo local
author: kgremban
manager: philmea
ms.author: veyalla
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a453779ffe4ae20acf55510d0ac9f9483763af21
ms.sourcegitcommit: c4700ac4ddbb0ecc2f10a6119a4631b13c6f946a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2019
ms.locfileid: "72964833"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>Cómo instalar IoT Edge en Kubernetes (versión preliminar)

IoT Edge puede integrarse con Kubernetes como un nivel de infraestructura resistente y altamente disponible. Registra una *definición de recursos personalizada* (CRD) de IoT Edge con el servidor de la API de Kubernetes. Además, proporciona un *operador* (agente de IoT Edge) que concilia el estado deseado administrado por la nube con el estado del clúster local. 

El programador de Kubernetes, que mantiene la disponibilidad del módulo y elige su colocación, administra la duración del módulo. IoT Edge administra la plataforma de aplicaciones perimetrales que se ejecutan en la parte superior, conciliando continuamente el estado deseado especificado en IoT Hub con el estado en el clúster de servidores perimetrales. El modelo de aplicaciones perimetrales sigue siendo el modelo conocido basado en los módulos y las rutas de IoT Edge. El operador de agente de IoT Edge realiza la traducción *automática* a las construcciones nativas de Kubernetes como, por ejemplo, pods, implementaciones, servicios, etc.

A continuación se muestra un diagrama de arquitectura de alto nivel:

![arquitectura de Kubernetes](./media/how-to-install-iot-edge-kubernetes/k8s-arch.png)

Todos los componentes de la implementación perimetral se limitan a un espacio de nombres de Kubernetes específico del dispositivo, lo que posibilita compartir los mismos recursos de clúster entre varios dispositivos perimetrales y sus implementaciones.

>[!NOTE]
>IoT Edge en Kubernetes se encuentra en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="install-locally-for-a-quick-test-environment"></a>Instalación local para un entorno de prueba rápida

### <a name="prerequisites"></a>Requisitos previos

* Kubernetes 1.10 o una versión más reciente. Si no tiene una configuración de clústeres, puede usar [Minikube](https://kubernetes.io/docs/setup/minikube/) para obtener un entorno de clústeres local. 

* [Helm](https://helm.sh/docs/using_helm/#quickstart-guide), el administrador de paquetes de Kubernetes.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) para ver e interactuar con el clúster.

### <a name="setup-steps"></a>Pasos de configuración

1. Iniciar **Minikube**

    ``` shell
    minikube start
    ```

1. Inicializar el componente de servidor **Helm** (*tiller*) en el clúster

    ``` shell
    helm init
    ```

1. Agregar un repositorio de IoT Edge y actualizar la instalación de Helm

    ``` shell
    helm repo add edgek8s https://edgek8s.blob.core.windows.net/helm/
    helm repo update
    ```

1. [Cree un IoT Hub](../iot-hub/iot-hub-create-through-portal.md), [registre un dispositivo de IoT Edge](how-to-register-device.md) y anote su cadena de conexión.

1. Instalar iotedged y el agente de IoT Edge en el clúster

    ```shell
    helm install \
    --name k8s-edge1 \
    --set "deviceConnectionString=replace-with-device-connection-string" \
    edgek8s/edge-kubernetes
    ```
1. Abrir el panel de Kubernetes en el explorador

    ```shell
    minikube dashboard
    ```

    En los espacios de nombres de clúster, verá uno para el dispositivo de IoT Edge que sigue la convención *msiot-\<iothub-name>-\<edgedevice-name>* . El agente de IoT Edge y los pods iotedged deben estar activos y en ejecución en este espacio de nombres.

1. Agregue un módulo de sensor de temperatura simulado mediante los pasos descritos en la sección [Implementar un módulo](quickstart-linux.md#deploy-a-module) del inicio rápido. La administración del módulo de IoT Edge se realiza desde el portal de IoT Hub como con cualquier otro dispositivo de IoT Edge. No se recomienda realizar cambios locales en la configuración del módulo mediante herramientas de Kubernetes, ya que es posible que se sobrescriban.

1. En unos segundos, al actualizar la página **Pods** del espacio de nombres del dispositivo en el panel, se indicará que el centro de IoT Edge y los pods de sensor simulados se ejecutan con los datos de ingestión del pod del centro de IoT Edge en IoT Hub.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para eliminar todos los recursos creados por la implementación perimetral, utilice el comando siguiente con el nombre usado en el paso 5 de la sección anterior.

``` shell
helm delete --purge k8s-edge1
```

## <a name="next-steps"></a>Pasos siguientes

### <a name="deploy-as-a-highly-available-edge-gateway"></a>Implementación como puerta de enlace perimetral de alta disponibilidad 

El dispositivo perimetral de un clúster de Kubernetes puede utilizarse como puerta de enlace de IoT para dispositivos de nivel inferior. Se puede configurar para que sea resistente a errores de nodo, lo que proporciona alta disponibilidad para las implementaciones perimetrales. Consulte este [tutorial detallado](https://github.com/Azure-Samples/iotedge-gateway-on-kubernetes) para utilizar IoT Edge en este escenario.