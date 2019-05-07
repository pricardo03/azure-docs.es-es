---
title: Cómo instalar IoT Edge en Kubernetes | Microsoft Docs
description: Obtenga información sobre cómo instalar IoT Edge en Kubernetes mediante un entorno de clúster de desarrollo local
author: kgremban
manager: philmea
ms.author: veyalla
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 66aca7be9a2df93d846d7e78bc64c93279afc2d1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160701"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>Cómo instalar IoT Edge en Kubernetes (versión preliminar)

IoT Edge puede integrarse con Kubernetes utilizándolo como una capa de infraestructura resistentes, altamente disponible. Registra un IoT Edge *definición de recursos personalizado* (CRD) con el servidor de la API de Kubernetes. Además, proporciona un *operador* (agente de IoT Edge) que concilia estado deseado en la nube administrado con el estado del clúster local. 

El programador de Kubernetes, que mantiene la disponibilidad del módulo y elige su colocación administra la duración del módulo. IoT Edge administra la plataforma de aplicaciones de borde que se ejecutan en la parte superior, reconciliar continuamente el estado deseado especificado en IoT Hub con el estado en el clúster de edge. El modelo de aplicación edge sigue siendo el modelo conocido en función de las rutas y los módulos de IoT Edge. Realiza el operador del agente de IoT Edge *automática* traducción a Kubernetes nativos se construye como pods, implementaciones, servicios, etcetera.

Este es un diagrama de arquitectura de alto nivel:

![arquitectura de kubernetes](./media/how-to-install-iot-edge-kubernetes/k8s-arch.png)

Todos los componentes de la implementación de edge se limita a un espacio de nombres de Kubernetes específico del dispositivo, lo que permite compartir los mismos recursos de clúster entre varios dispositivos de edge y sus implementaciones.

>[!NOTE]
>IoT Edge en Kubernetes está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="install-locally-for-a-quick-test-environment"></a>Instale localmente para un entorno de prueba rápida

### <a name="prerequisites"></a>Requisitos previos

* Kubernetes 1,10 o versiones más recientes. Si no tiene una configuración de clúster existente, puede usar [Minikube](https://kubernetes.io/docs/setup/minikube/) para un entorno de clúster local. 

* [Helm](https://helm.sh/docs/using_helm/#quickstart-guide), el Administrador de paquetes de Kubernetes.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) para ver e interactuar con el clúster.

### <a name="setup-steps"></a>Pasos de configuración

1. Iniciar **Minikube**

    ``` shell
    minikube start
    ```

1. Inicializar el **Helm** componente de servidor (*tiller*) en el clúster

    ``` shell
    helm init
    ```

1. Agregar repositorio de IoT Edge y actualizar la instalación de helm

    ``` shell
    helm repo add edgek8s https://edgek8s.blob.core.windows.net/helm/
    helm repo update
    ```

1. [Crear un IoT Hub](../iot-hub/iot-hub-create-through-portal.md), [registrar un dispositivo IoT Edge](how-to-register-device-portal.md)y tenga en cuenta su cadena de conexión.

1. Instalar iotedged y agente de IoT Edge en el clúster

    ```shell
    helm install \
    --name k8s-edge1 \
    --set "deviceConnectionString=replace-with-device-connection-string" \
    edgek8s/edge-kubernetes
    ```
1. Abra el panel de Kubernetes en el explorador

    ```shell
    minikube dashboard
    ```

    En los espacios de nombres de clúster, verá uno para el dispositivo de IoT Edge siguen la convención *msiot -\<iothub-name >-\<edgedevice-name >*. Los pods de IoT Edge agent y iotedged deben estar en marcha en este espacio de nombres.

1. Agregar un módulo de sensor de temperatura simulados mediante los pasos descritos en la [implementar un módulo](quickstart-linux.md#deploy-a-module) sección del tutorial rápido. Administración del módulo de IoT Edge se realiza desde el portal de IoT Hub al igual que cualquier otro dispositivo de IoT Edge. No se recomienda realizar los cambios locales en configuración del módulo a través de herramientas de Kubernetes, ya que es posible que se sobrescriban.

1. En unos segundos, actualizando el **Pods** mostrará una página en el espacio de nombres de dispositivo de edge en el panel lista el centro de IoT Edge y los pods de sensor simulados como si se ejecutaran con el centro de IoT Edge pod ingesta de datos en IoT Hub.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar todos los recursos creados por la implementación de edge, use el comando siguiente con el nombre usado en el paso 5 de la sección anterior.

``` shell
helm delete --purge k8s-edge1
```

## <a name="next-steps"></a>Pasos siguientes

### <a name="deploy-as-a-highly-available-edge-gateway"></a>Implementar como una puerta de enlace de borde de alta disponibilidad 

El dispositivo de edge en un clúster de Kubernetes puede utilizarse como una puerta de enlace de IoT para dispositivos de nivel inferior. Se puede configurar para que sea resistente a errores de nodo, lo que proporciona alta disponibilidad para las implementaciones de edge. Vea este [tutorial detallado](https://github.com/Azure-Samples/iotedge-gateway-on-kubernetes) usar IoT Edge en este escenario.