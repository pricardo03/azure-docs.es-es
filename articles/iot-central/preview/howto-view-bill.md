---
title: Administrar la factura y convertir la versión de prueba a una de pago por uso en la aplicación Azure IoT Central | Microsoft Docs
description: Como administrador, puede aprender a administrar su factura y a convertir la versión de prueba a una de pago por uso en la aplicación Azure IoT Central
author: v-krghan
ms.author: v-krghan
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 11bcdaab34cf4d82b688a578ab2d9ee686ffe07e
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73894164"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>Administrar la factura en una aplicación de IoT Central

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En este artículo se describe cómo puede administrar la factura como administrador en la aplicación Azure IoT Central en la sección de administración y también cómo puede convertir su versión de prueba a una de pago por uso.

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

    ![Evaluación gratuita](media/howto-view-bill/freetrialbilling.png)

1. Seleccione **Convert to Pay-As-You-Go** (Convertir a pago por uso).

    ![Conversión de la evaluación gratuita](media/howto-view-bill/convert.png)

1. Seleccione la instancia de Azure Active Directory correspondiente y la suscripción de Azure que se usará con la aplicación de pago por uso.

1. Después de seleccionar **Convert** (Convertir), la aplicación pasará a ser de pago por uso y se comenzará la facturación.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar su factura en la aplicación Azure IoT Central, el siguiente paso sugerido es aprender sobre la [personalización de la interfaz de usuario de la aplicación](howto-customize-ui.md) en Azure IoT Central.