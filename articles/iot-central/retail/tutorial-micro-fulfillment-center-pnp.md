---
title: Tutorial de la plantilla de aplicación del centro de micrologística | Microsoft Docs
description: Un tutorial sobre la plantilla de aplicación del centro de micrologística para IoT Central
author: avneet723
ms.author: avneets
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 01/09/2020
ms.openlocfilehash: 2ed6f37bc3b00397fa6331a501e1b16c8d622b5f
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026228"
---
# <a name="tutorial-deploy-and-walk-through-a-micro-fulfillment-center-application-template"></a>Tutorial: Implementación y recorrido por una plantilla de aplicación del centro de micrologística

En este tutorial, aprovecharemos la plantilla de aplicación ***Micro-fulfillment center*** (Centro de micrologística) de Azure IoT Central para mostrarle cómo crear una solución para el comercio minorista. Aprenderá cómo implementar la plantilla del centro de micrologística, qué elementos se incluyen de fábrica y qué podría hacer a continuación.

En este tutorial, aprenderá a: 
> [!div class="checklist"]
> * Usar la plantilla **Micro-fulfillment center** (Centro de micrologística) de Azure IoT Central para crear una aplicación para el comercio minorista
> * Recorrer la aplicación 

## <a name="prerequisites"></a>Prerequisites
Para completar esta serie de tutoriales, necesitará lo siguiente:
* Suscripción a Azure. Opcionalmente, puede usar una evaluación gratuita de 7 días. Si no tiene una suscripción de Azure, puede crear una en la [página de suscripción a Azure](https://aka.ms/createazuresubscription).

## <a name="create-an-application"></a>Crear una aplicación 
En esta sección, creará una nueva aplicación de Azure IoT Central a partir de una plantilla. Usará esta aplicación a lo largo de la serie de tutoriales para crear una solución completa.

Para crear una nueva aplicación de Azure IoT Central:

1. Vaya al sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral).
1. Si tiene una suscripción a Azure, inicie sesión con las credenciales que usa para acceder a ella; si no la tiene, inicie sesión con una cuenta de Microsoft:

   ![Incorporación de la cuenta de la organización](./media/tutorial-in-store-analytics-create-app-pnp/sign-in.png)

1. Para empezar a crear una nueva aplicación de Azure IoT Central, seleccione **New Application** (Nueva aplicación).

1. Seleccione **Retail** (Comercio minorista).  En la página de comercio minorista se muestran varias plantillas de aplicación de venta al por menor.

Para crear una nueva aplicación del centro de micrologística que usa las características de la versión preliminar:  
1. Seleccione la plantilla de aplicación **Micro-fulfillment center** (Centro de micrologística). Esta plantilla incluye plantillas de dispositivo para todos los dispositivos usados en el tutorial. La plantilla también proporciona un panel de operadores para la supervisión de las condiciones del centro de logística, así como las condiciones de los operadores robóticos. 

    > [!div class="mx-imgBorder"]
    > ![Tipo de aplicación de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-homepage-mfc.png)
    
1. Opcionalmente, elija un valor descriptivo para **Application name** (Nombre de la aplicación).  La plantilla de la aplicación se basa en la compañía ficticia Northwind Traders. 

    > [!NOTE]
    > Aunque use un **nombre de aplicación** descriptivo, debe usar un valor único para la dirección **URL** de la aplicación.

1. Si tiene una suscripción de Azure, escriba el *directorio, la suscripción de Azure y la región*. Si no tiene una suscripción, puede activar **7-day free trial** (Evaluación gratuita de 7 días) y completar la información de contacto necesaria.  

    Para más información acerca de los directorios y las suscripciones, consulte la [guía de inicio rápido para crear una aplicación](../preview/quick-deploy-iot-central.md).

1. Seleccione **Crear**.

> [!div class="mx-imgBorder"]
> ![Creación de una aplicación de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-create-app-mfc.png)

## <a name="walk-through-the-application"></a>Recorrido por la aplicación. 

### <a name="dashboard"></a>Panel 

Después de implementar correctamente la plantilla de aplicación, llegará primero al **panel del centro de micrologística de Northwind Traders**. Northwind Traders es un minorista ficticio que tiene un centro de micrologística que se está administrando en esta aplicación de IoT Central. En este panel de operador, verá información y telemetría sobre los dispositivos de esta plantilla junto con un conjunto de comandos, trabajos y acciones que puede realizar. El panel se divide lógicamente en dos secciones (izquierda y derecha). A la izquierda, tiene la capacidad de supervisar las condiciones del entorno dentro de la estructura logística y, a la derecha, puede supervisar el estado de un operador robótico dentro de la instalación.  

Desde el panel se puede:
   * Consultar la telemetría de dispositivos, como el número de recogidas, el número de pedidos procesados y propiedades como el estado del sistema de la estructura, entre otros.  
   * Ver el **plan de la planta** y la ubicación de los operadores robóticos en la estructura logística.
   * Desencadenar comandos como restablecer el sistema de control, actualizar el firmware del operador, volver a configurar la red, entre otros.

> [!div class="mx-imgBorder"]
> ![Panel del centro de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard1.png)
   * Vea un ejemplo del panel que un operador puede aprovechar para supervisar las condiciones del centro logístico. 
   * Supervise el estado de las cargas que se ejecutan en el dispositivo de puerta de enlace en el centro logístico.    

> [!div class="mx-imgBorder"]
> ![Panel del centro de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard2.png)

## <a name="device-template"></a>Plantilla de dispositivo
Si hace clic en la pestaña Plantillas de dispositivo, verá que hay dos tipos de dispositivo diferentes que forman parte de la plantilla: 
   * **Operador robótico**: esta plantilla de dispositivo representa la definición de un operador robótico que se ha implementado en la estructura logística y realiza las operaciones adecuadas de almacenamiento y recuperación. Si hace clic en la plantilla, verá que el robot envía datos del dispositivo, como la temperatura, la posición del eje y las propiedades, como el estado del operador robótico, entre otros datos. 
   * **Supervisión de las condiciones de estructura**: esta plantilla de dispositivo representa una recopilación de dispositivos que permite supervisar la condición del entorno, así como el dispositivo de puerta de enlace que hospeda varias cargas de trabajo perimetrales para potenciar el centro de micrologística. El dispositivo envía datos de telemetría, como la temperatura, el número de recogidas o el número de pedidos, además del estado y el estado de las cargas de trabajo de proceso que se ejecutan en el entorno. 

> [!div class="mx-imgBorder"]
> ![Plantilla de dispositivo del centro de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/device-templates.png)

Si hace clic en la pestaña Grupos de dispositivos, también verá que estas plantillas de dispositivo tienen automáticamente grupos de dispositivos creados.

## <a name="rules"></a>Reglas
Al saltar a la pestaña Reglas, verá una regla de ejemplo que existe en la plantilla de aplicación para supervisar las condiciones de temperatura del operador robótico. Puede usar esta regla para avisar al operador si un robot específico de la instalación se está sobrecalentando y se debe desconectar para el mantenimiento. 

Aproveche la regla de ejemplo como inspiración para definir las reglas más adecuadas para sus funciones empresariales.

   - **El operador robótico se calienta demasiado:** esta regla se desencadenará si el operador robótico alcanza un umbral de temperatura durante un período de tiempo. 

> [!div class="mx-imgBorder"]
> ![Reglas del centro de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/rules.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine la plantilla de la aplicación. Para ello, vaya a **Administración** > **Configuración de la aplicación** y haga clic en **Eliminar**.

> [!div class="mx-imgBorder"]
> ![Limpieza de la aplicación del centro de micrologística](./media/tutorial-micro-fulfillment-center-app-pnp/delete.png)

## <a name="next-steps"></a>Pasos siguientes
* Más información acerca de la arquitectura de la [solución del centro de micrologística](./architecture-micro-fulfillment-center-pnp.md).
* Más información sobre otras [plantillas de venta minorista de IoT Central](./overview-iot-central-retail-pnp.md).
* Para obtener más información acerca de IoT Central, consulte [Introducción a IoT Central](../preview/overview-iot-central.md).