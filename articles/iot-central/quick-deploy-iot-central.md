---
title: Creación de una aplicación de Azure IoT Central | Microsoft Docs
description: Cree una aplicación de Azure IoT Central para administrar máquinas expendedoras refrigeradas. Visualice los datos de telemetría generados a partir de los dispositivos simulados.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/15/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 0199efbc1456206c67efb846b4381cae333e3749
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309049"
---
# <a name="create-an-azure-iot-central-application"></a>Creación de una aplicación de Azure IoT Central

Como _generador_, usará la interfaz de usuario de Azure IoT Central para definir la aplicación de Azure IoT Central. En esta guía de inicio rápido se muestra cómo crear una aplicación de Azure IoT Central que contenga un ejemplo de _plantilla de dispositivo_ y _dispositivos_ simulados.

## <a name="create-the-application"></a>Creación de la aplicación

Para completar esta guía de inicio rápido debe crear una aplicación de Azure IoT Central a partir de la plantilla de aplicación **Sample Contoso**.

Vaya a la página [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de Azure IoT Central. A continuación, escriba la dirección de correo electrónico y la contraseña que usa para acceder a la suscripción de Azure:

![Incorporación de la cuenta de la organización](media/quick-deploy-iot-central/sign-in.png)

Para empezar a crear una nueva aplicación de Azure IoT Central, elija **New Application** (Nueva aplicación):

![Página Application Manager (Administrador de aplicaciones) de Azure IoT Central](media/quick-deploy-iot-central/iotcentralhome.png)

Para crear una nueva aplicación de Azure IoT Central:

1. Elija el plan de pago **Free Trial Application** (Evaluación gratuita de la aplicación).
1. Elija un nombre de aplicación descriptivo, como **Contoso IoT**. Azure IoT Central genera un prefijo de dirección URL único. Puede cambiar este prefijo de dirección URL por algo más fácil de recordar.
1. Elija la plantilla de aplicación **Sample Contoso**.
1. Luego elija **Crear**.

![Página de creación de una aplicación de Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido se ha creado una aplicación de Azure IoT Central previamente rellenada que contiene una plantilla de dispositivo **Refrigerated Vending Machine** y dispositivos simulados. Consulte el artículo de [definición de una nueva plantilla de dispositivo en la aplicación](tutorial-define-device-type.md) para más información sobre cómo definir sus propias plantillas de dispositivo como generador.
