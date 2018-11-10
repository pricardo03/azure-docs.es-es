---
title: Dónde implementar modelos con el servicio Azure Machine Learning | Microsoft Docs
description: Descubra los diferentes métodos que puede usar para implementar los modelos en producción mediante el servicio de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/29/2018
ms.openlocfilehash: 97ac405db3de4fa2c6f1173f813eafd41a5361ad
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209452"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementación de modelos con el servicio Azure Machine Learning

El servicio Azure Machine Learning dispone de varios métodos que puede utilizar para implementar el modelo entrenado. En este documento, aprenderá a implementar el modelo como un servicio web en la nube de Azure o en dispositivos IoT Edge.

Puede implementar modelos en los destinos de proceso siguientes:

- [Azure Container Instances (ACI)](#aci): implementación rápida. Se recomienda para desarrollo o pruebas.
- [Azure Kubernetes Service (AKS)](#aks): se recomienda para implementaciones de producción a gran escala. Proporciona escalado automático y tiempos de respuesta rápidos.
- [Azure IoT Edge](#iotedge): implemente modelos en dispositivos IoT. Se produce inferencia en el dispositivo.
- [Matriz de puertas programables por campo (FPGA)](#fpga): latencia ultra baja para inferencia en tiempo real.

En este documento, se describe también cada una de estas opciones en detalle.

## <a id="aci"></a>Azure Container Instances

Utilice Azure Container Instances para implementar los modelos como un punto de conexión de API de REST si se dan una o varias de las siguientes condiciones:
- Debe implementar y validar rápidamente el modelo. La implementación de ACI finaliza en menos de 5 minutos.
- Desea implementar el modelo en un entorno de desarrollo o de prueba. ACI permite implementar 20 grupos de contenedores por suscripción. Para más información, consulte el documento [Disponibilidad de cuotas y regiones en Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

Para más información, consulte el documento [Implementación de un modelo en Azure Container Instances](how-to-deploy-to-aci.md).

## <a id="aks"></a>Azure Kubernetes Service

En el caso de escenarios de producción a gran escala, use Azure Kubernetes Service (AKS). Puede usar un clúster AKS existente o crear uno nuevo con el SDK de Azure Machine Learning, la CLI o Azure Portal.

Crear un clúster de AKS es un proceso único en el área de trabajo. Puede volver a usar este clúster con diferentes implementaciones. Si elimina el clúster, deberá crear uno nuevo la próxima vez que necesite implementar.

Azure Kubernetes Service proporciona las siguientes funcionalidades:

* Escalado automático
* Registro
* Recopilación de datos de modelos
* Tiempos de respuesta rápidos para los servicios web

El proceso de creación de un clúster de AKS tarda aproximadamente 20 minutos.

Para más información, consulte el documento [Implementación de un modelo en Azure Kubernetes Service](how-to-deploy-to-aks.md).

## <a id="iotedge"></a>Azure IoT Edge

Con los dispositivos IoT, es más rápido realizar la puntuación en el dispositivo en lugar de enviar los datos a la nube para su puntuación. Con Azure IoT Edge, puede hospedar el modelo en dispositivos perimetrales. Implemente el modelo en IoT Edge si necesita una o varias de las siguientes funcionalidades:
- Controlar las tareas prioritarias localmente, aunque no haya conexión con la nube.
- Trabajar con datos generados que son demasiado grandes como para extraerlos rápidamente de la nube.
- Habilitar el procesamiento en tiempo real utilizando la inteligencia de los dispositivos locales o de los dispositivos próximos.
- Adaptarse a los requisitos relacionados con la privacidad de los datos. 

Para más información, consulte el documento sobre la [implementación en Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).

Para más información sobre el servicio de IoT Edge, consulte la [documentación de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).


## <a id="fpga"></a>Matrices de puertas programables por campo (FPGA)

Los modelos acelerados mediante hardware con la tecnología Project Brainwave permiten conseguir una latencia muy baja en solicitudes de inferencia en tiempo real. Project Brainwave acelera las redes neurales profundas (DNN) implementadas en matrices de puertas programables por campo en la nube de Azure. Las DNN que se utilizan habitualmente están disponibles en forma de características para transferir conocimientos o pueden personalizarse con ponderaciones entrenadas con sus propios datos.

Para más información, consulte el documento sobre la [implementación en una FPGA](how-to-deploy-fpga-web-service.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de cómo implementar un modelo en un destino de proceso específico, consulte los siguientes documentos:

* [Implementación de un modelo en Azure Container Instances](how-to-deploy-to-aci.md)
* [Implementación de un modelo en Azure Kubernetes Service (AKS)](how-to-deploy-to-aks.md)
* [Implementación de un modelo en Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)
* [Implementación de un modelo en una FPGA](how-to-deploy-fpga-web-service.md)
