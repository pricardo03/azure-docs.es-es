---
title: Consultar la factura y convertir la versión de prueba a una de pago por uso en la aplicación Azure IoT Central | Microsoft Docs
description: Como administrador, puede aprender a ver su factura y a convertir la versión de prueba a una de pago por uso en la aplicación Azure IoT Central
author: v-krghan
ms.author: v-krghan
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: d27288718b7f224c78c9cf1f15d05198f628fd15
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878952"
---
# <a name="view-your-bill-in-an-iot-central-application"></a>Consulta de la factura en la aplicación de IoT Central

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este artículo se describe cómo ver la factura como administrador en la aplicación Azure IoT Central en la sección de administración y también cómo puede convertir su versión de prueba a una de pago por uso.

Para tener acceso a la sección **Administración** y poder usarla, debe disponer del rol **Administrador** para la aplicación Azure IoT Central. Si crea una aplicación Azure IoT Central, se le asigna automáticamente el rol **Administrator** para esa aplicación.

## <a name="view-your-bill"></a>Ver la factura

Para ver la factura, vaya a la página **Facturación** en la sección **Administración**. La página de facturación de Azure se abre en una nueva pestaña, en la que puede ver la factura de cada una de las aplicaciones de Azure IoT Central.

## <a name="convert-your-trial-to-pay-as-you-go"></a>Conversión de la cuenta de evaluación gratuita en una de pago por uso

- Las aplicaciones de **versión de prueba** son gratuitas durante siete días y luego expiran. Se pueden convertir a pago por uso en cualquier momento antes.
- Las aplicaciones de **pago por uso** se cobran por dispositivo; los primeros cinco dispositivos son gratuitos, por suscripción.

Obtenga más información sobre los precios en la [página de precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

En la sección de facturación, puede convertir sus aplicaciones en versión de prueba a versiones de pago por uso.

Para completar el proceso de autoservicio, siga estos pasos:

1. Vaya a la página **Facturación** de la sección **Administración**.

    ![Evaluación gratuita](media/howto-view-bill-pnp/freetrialbilling.png)

1. Seleccione **Convert to Pay-As-You-Go** (Convertir a pago por uso).

    ![Conversión de la evaluación gratuita](media/howto-view-bill-pnp/convert.png)

1. Seleccione la instancia de Azure Active Directory correspondiente y la suscripción de Azure que se usará con la aplicación de pago por uso.

1. Después de seleccionar **Convert** (Convertir), la aplicación pasará a ser de pago por uso y se comenzará la facturación.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a ver su factura en la aplicación Azure IoT Central, el siguiente paso sugerido es aprender sobre cómo [personalizar la interfaz de usuario de la aplicación](howto-customize-ui-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) en Azure IoT Central.