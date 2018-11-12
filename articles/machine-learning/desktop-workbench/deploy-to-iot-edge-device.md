---
title: Implementación de un modelo de Azure Machine Learning en un dispositivo de Azure IoT Edge | Microsoft Docs
description: En este documento se describe cómo se pueden implementar modelos de Azure Machine Learning en dispositivos de Azure IoT Edge.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 7322b07740d5dec85b6217e122fb262647527c96
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258415"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Implementación de un modelo de Azure Machine Learning en un dispositivo de Azure IoT Edge

Todos los modelos de Azure Machine Learning pueden almacenarse en contenedores como servicios web basados en Docker. Azure IoT Edge le permite implementar contenedores de forma remota en dispositivos. Utilice estos servicios juntos para ejecutar los modelos en Edge para reducir los tiempos de respuesta y la transferencia de datos. 

Se pueden encontrar scripts e instrucciones adicionales en el [kit de herramientas de AI para Azure IoT Edge](https://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Puesta en marcha del modelo

Los módulos de Azure IoT Edge se basan en las imágenes de contenedor. Para implementar el modelo de Machine Learning en un dispositivo IoT Edge, deberá crear una imagen de Docker.

Ponga en marcha el modelo mediante las instrucciones que se describen en [Implementación de un servicio web de administración de modelos de Azure Machine Learning](model-management-service-deploy.md) para crear una imagen de Docker con su modelo.

## <a name="deploy-to-azure-iot-edge"></a>Implementación en Azure IoT Edge

Una vez que tenga la imagen del modelo, puede implementarla en cualquier dispositivo de Azure IoT Edge. Todos los modelos de Machine Learning se pueden ejecutar en dispositivos IoT Edge. 

### <a name="set-up-an-iot-edge-device"></a>Configuración de un dispositivo de IoT Edge

Use la documentación de Azure IoT Edge para preparar un dispositivo. 

1. [Registre un dispositivo con Azure IoT Hub](../../iot-edge/how-to-register-device-portal.md). La salida de este proceso es una cadena de conexión que puede usar para configurar el dispositivo físico. 
2. Instale el entorno de ejecución de IoT Edge en el dispositivo físico y configúrelo con una cadena de conexión. Puede instalar el tiempo de ejecución en dispositivos [Windows](../../iot-edge/how-to-install-iot-edge-windows-with-windows.md) o [Linux](../../iot-edge/how-to-install-iot-edge-linux.md).  


### <a name="find-the-machine-learning-container-image-location"></a>Buscar la ubicación de la imagen de contenedor de Machine Learning
Necesita la ubicación de la imagen de contenedor de Machine Learning. Para buscar la ubicación de la imagen de contenedor:

1. Inicie sesión en [Azure Portal](http://portal.azure.com/).
2. En **Azure Container Registry**, seleccione el registro que desea inspeccionar.
3. En el registro, haga clic en **Repositorios** para ver una lista de todos los repositorios y sus imágenes.

Mientras mira el registro de contenedor en Azure Portal, recupere las credenciales del registro de contenedor. Estas credenciales deben proporcionarse en al dispositivo de IoT Edge para que pueda extraer la imagen del registro privado. 

1. En el registro de contenedor, haga clic en **Claves de acceso**. 
2. **Habilite** el usuario administrador, si aún no lo está. 
3. Guarde los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**. 

### <a name="deploy-the-container-image-to-your-device"></a>Implementación de la imagen de contenedor en el dispositivo

Con la imagen de contenedor y las credenciales del registro de contenedor, está listo para implementar el modelo de Machine Learning en su dispositivo de IoT Edge. 

Siga las instrucciones de [Implementación de módulos de Azure IoT Edge desde Azure Portal](../../iot-edge/how-to-deploy-modules-portal.md) para iniciar el modelo en su dispositivo IoT Edge. 











