---
title: Uso de los iconos en el panel de la aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador, aprenda a usar los iconos en los paneles de aplicación, los paneles de conjunto de dispositivos y los paneles de dispositivo.
author: v-krghan
ms.author: v-krghan
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 0803258c362279049a49a7eee00e7a4763621672
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942237"
---
# <a name="how-to-use-tiles"></a>Uso de los iconos
Puede usar los iconos para personalizar los paneles de aplicación, los paneles de dispositivo y los paneles de conjunto de dispositivos. Puede agregar varios iconos a un panel y personalizarlos para mostrar la información relevante para la aplicación. También puede cambiar el tamaño de los iconos y personalizar el diseño en cualquier panel. En la siguiente captura de pantalla se muestra el panel de la aplicación con iconos diferentes.

![Panel de la aplicación](media/howto-use-tiles/image1a.png)


En la tabla siguiente se resume el uso de iconos en Azure IoT Central:

 
| Icono | panel | DESCRIPCIÓN
| ----------- | ------- | ------- |
| Vínculo | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos de vínculo son iconos clicables que muestran un encabezado y un texto de descripción. Use un icono de vínculo para que los usuarios puedan navegar a una dirección URL que está relacionada con la aplicación. |
| Imagen | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos de imagen muestran una imagen personalizada y se puede hacer clic en ellos. Use un icono de imagen para agregar gráficos a un panel y, opcionalmente, permitir a los usuarios navegar a una dirección URL que es relevante para la aplicación.|
| Etiqueta | Paneles de aplicación |Los iconos de etiqueta muestran texto personalizado en un panel. Puede elegir el tamaño del texto. Use un icono de etiqueta para agregar información relevante al panel como, por ejemplo, descripciones, detalles de contacto o ayuda.|
| Map | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos de mapa muestran la ubicación y el estado de un dispositivo en un mapa. Por ejemplo, puede mostrar dónde está un dispositivo y si el ventilador está encendido.|
| Gráfico de líneas | Paneles de aplicación y paneles de dispositivo |Los iconos de gráfico de líneas muestran un gráfico de medidas agregadas para un dispositivo durante un período de tiempo. Por ejemplo, puede mostrar un gráfico de líneas que muestre la temperatura y presión medias en un dispositivo durante la última hora.|
| Gráfico de barras | Paneles de aplicación y paneles de dispositivo |Los iconos de gráfico de barras muestran un gráfico de mediciones agregadas para un dispositivo durante un período de tiempo. Por ejemplo, puede mostrar un gráfico de barras que muestre la temperatura y presión medias en un dispositivo durante la última hora. |
| Historial de eventos | Paneles de aplicación y paneles de dispositivo |El icono de historial de eventos muestra los eventos ocurridos en un dispositivo durante un período de tiempo. Por ejemplo, puede usarlo para mostrar todos los cambios de temperatura que se han producido en un dispositivo durante la última hora. |
| Historial de los estados | Paneles de aplicación y paneles de dispositivo |El icono de historial de estado muestra los valores de las medidas para un período de tiempo. Por ejemplo, puede usarlo para mostrar los valores de temperatura de un dispositivo durante la última hora.|
| KPI | Paneles de aplicación y paneles de dispositivo | El icono de KPI muestra una telemetría o medida de evento agregadas para un período de tiempo. Por ejemplo, puede usarlo para mostrar la temperatura máxima alcanzada para un dispositivo durante la última hora.|
| Último valor conocido | Paneles de aplicación y paneles de dispositivo |El icono de último valor conocido muestra el último valor de una telemetría o medida de estado. Por ejemplo, puede usar este icono para mostrar las medidas más recientes de temperatura, presión y humedad de un dispositivo.|
| Cuadrícula de conjunto de dispositivos | Paneles de conjunto de dispositivos y paneles de aplicación | La cuadrícula de conjunto de dispositivos muestra información sobre el conjunto de dispositivos. Use un icono de cuadrícula de conjunto de dispositivos para mostrar información como, por ejemplo, el nombre, la ubicación y el modelo de todos los dispositivos del conjunto.|


Para obtener más información sobre cómo configurar el panel en la aplicación de Azure IoT Central, consulte [Adición de iconos al panel](./howto-add-tiles-to-your-dashboard.md).
