---
title: Creación de una aplicación de Azure IoT Central | Microsoft Docs
description: Cree una nueva aplicación de Azure IoT Central. Cree una aplicación de evaluación o de pago por uso mediante una plantilla de aplicación.
author: viv-liu
ms.author: viviali
ms.date: 06/07/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 9d242c48068e96498a811f52dbc599abd32bc936
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382999"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Creación de una aplicación de Azure IoT Central (características en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este inicio rápido se muestra cómo crear una aplicación de Azure IoT Central que use características en versión preliminar como IoT Plug and Play.

> [!WARNING]
> Las funcionalidades de IoT Plug and Play en Azure IoT Central se encuentran actualmente en versión preliminar pública. No use ninguna aplicación de IoT Central preparada para IoT Plug and Play para cargas de trabajo de producción. Para entornos de producción, use una aplicación de IoT Central creada con una plantilla de aplicación actual y disponible con carácter general.

## <a name="create-an-application"></a>Creación de una aplicación

Vaya al sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral). Luego, inicie sesión con una cuenta Microsoft personal, profesional o educativa.

Para empezar a crear una nueva aplicación de Azure IoT Central, seleccione **New Application** (Nueva aplicación). Este vínculo le lleva a la página **Create an application** (Crear aplicación).

![Página de creación de una aplicación de Azure IoT Central](media/quick-deploy-iot-central-pnp/iotcentralcreate.png)

Para crear una aplicación de Azure IoT Central que incluya características en versión preliminar, como IoT Plug and Play, siga estos pasos:

1. Elija un plan de pago:
   - Las aplicaciones de **versión de prueba** son gratuitas durante siete días y luego expiran. Se pueden convertir a pago por uso en cualquier momento antes. Si crea una aplicación de **evaluación gratuita**, deberá escribir la información de contacto y elegir si desea recibir información y sugerencias de Microsoft.
   - Las aplicaciones de **Pago por uso** se cobran por dispositivo y los cinco primeros dispositivos son gratuitos. Si crear una aplicación de **pago por uso**, deberá seleccionar su *directorio*, *suscripción de Azure* y *región*:
      - El *Directorio* es la instancia de Azure Active Directory (AD) para crear la aplicación. Contiene las identidades de usuario, las credenciales y otra información de la organización. Si no tiene un inquilino de Azure AD, se crea uno automáticamente al crearse una suscripción de Azure.
      - Una *suscripción de Azure* permite crear instancias de los servicios de Azure. IoT Central aprovisionará los recursos de su suscripción. Si no tiene una suscripción de Azure, puede crear una en [la página de suscripción a Azure](https://aka.ms/createazuresubscription). Después de crear la suscripción de Azure, vuelva a la página **Crear aplicación**. La nueva suscripción aparecerá en el cuadro de lista desplegable **Suscripción de Azure**.
      - *Región* es la ubicación física donde desea crear la aplicación. Durante la versión preliminar pública, las únicas regiones disponibles para una **Aplicación de versión preliminar** son **Norte de Europa** y **Centro de EE. UU.** .

      Obtenga más información sobre los precios en la [página de precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

1. Elija un nombre de aplicación descriptivo, como **Contoso IoT**. Azure IoT Central genera un prefijo de dirección URL único. Puede cambiar este prefijo de dirección URL por algo más fácil de recordar.

1. Elija la plantilla **Preview application** (Aplicación de versión preliminar). Una plantilla de aplicación puede contener elementos predefinidos, como plantillas de dispositivos y paneles que le ayudarán a empezar a trabajar.

1. En la parte inferior de la página, seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una aplicación de IoT Central que usa las características en versión preliminar. El siguiente paso que le sugerimos es:

> [!div class="nextstepaction"]
> [Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central](./tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
