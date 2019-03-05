---
title: Opciones de IoT de Microsoft Azure | Microsoft Docs
description: Elija cómo implementar la solución de IoT mediante los aceleradores de soluciones de IoT de Azure, Azure IoT Central o Azure IoT Hub.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: de08894a8493ba64a59f4e012bc3f6b4f6e95b83
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56880757"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Comparación de las opciones de Azure IoT y Azure IoT Central

Microsoft Azure IoT Central y Azure IoT ofrecen varias opciones para la creación de una solución de IoT. Estas opciones son adecuadas para distintos conjuntos de requisitos del cliente:

* [Azure IoT Central](overview-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) es una solución de software como servicio (SaaS) que usa un enfoque basado en un modelo que le ayuda a crear soluciones de IoT de nivel empresarial sin necesidad de contar con experiencia en el desarrollo de soluciones en la nube.

* Los [aceleradores de soluciones de IoT de Azure](https://docs.microsoft.com/azure/iot-accelerators/) son una colección de nivel empresarial de [aceleradores de soluciones](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) integrados en la plataforma como servicio de Azure (PaaS) que le ayudan a acelerar el desarrollo de soluciones de IoT personalizadas.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub es la PaaS de Azure central que utilizan tanto Azure IoT Central como los aceleradores de soluciones de Azure IoT. IoT Hub admite una comunicación bidireccional confiable y segura entre millones de dispositivos de IoT y una solución en la nube. IoT Hub le ayuda a superar los desafíos de la implementación de IoT como son:

* La administración y la conectividad de los dispositivos de gran volumen.
* La ingesta de telemetría de gran volumen.
* El comando y el control de dispositivos.
* El cumplimiento de la seguridad de los dispositivos.

## <a name="compare-azure-iot-central-and-azure-iot-solution-accelerators"></a>Comparación de los aceleradores de soluciones IoT de Azure y Azure IoT Central

La elección de un producto de IoT de Azure es una parte fundamental del planeamiento de la solución de IoT. IoT Hub es un servicio individual de Azure que, por sí mismo, no proporciona una solución de IoT completa. Puede usar IoT Hub como un punto de partida para cualquier solución de IoT. Tampoco es necesario usar aceleradores de soluciones de Azure IoT o Azure IoT Central para poder usar IoT Hub. Tanto los aceleradores de soluciones de IoT como Azure IoT Central usan IoT Hub, junto con otros servicios de Azure.

En la tabla siguiente se resumen las principales diferencias entre los aceleradores de soluciones de IoT y Azure IoT Central para ayudarle a elegir la opción correcta en función de sus necesidades:

|     | Azure IoT Central | Aceleradores de soluciones de IoT de Azure |
| --- | ----------- | --------- |
| Uso principal                      | Acelerar el tiempo de comercialización para las soluciones de IoT sencillas que no requieren una personalización profunda del servicio.                                                    | Acelerar el desarrollo de una solución de IoT personalizada que necesita la máxima flexibilidad.                                                                                                                             |
| Acceso a los servicios PaaS subyacentes | SaaS. Dado que se trata de una solución totalmente administrada, los servicios subyacentes no se exponen.                                                                                            | Tiene acceso a los servicios de Azure subyacentes para administrarlos o reemplazarlos según sea necesario.                                                                                                                    |
| Flexibilidad                        | Mediana. Puede usar la experiencia de usuario integrada y basada en el explorador para personalizar el modelo de solución y los aspectos de la interfaz de usuario. La infraestructura no es personalizable porque los distintos componentes no se exponen. | Alta. El código de los microservicios es abierto y puede modificarlo como considere oportuno. Además, puede personalizar la infraestructura de implementación.                                               |
| Nivel de dificultad                        | Baja. Necesita conocimientos de modelado para personalizar la solución. No se requieren conocimientos de codificación.                                                                          | Medio alto. Necesita conocimientos de .NET o Java para personalizar el back-end de la solución. Necesita conocimientos de JavaScript para personalizar la visualización.                                                                       |
| Experiencia introductoria             | Tanto las plantillas de aplicaciones como las plantillas de dispositivos proporcionan modelos precompilados. Se pueden implementar en minutos.                                                                                                  | Las soluciones preconfiguradas implementan escenarios comunes de IoT. Se pueden implementar en minutos.                                                                                                                            |
| Precios                            | Estructura de precios sencilla y predecible.                                                                                                                           | Puede ajustar los servicios para controlar el costo.                                                                                                                                                            |

Los productos que puede usar para compilar la solución de IoT dependen de:

* Los requisitos empresariales.
* El tipo de solución que desee compilar.
* La variedad de destrezas de la organización para crear y mantener la solución a largo plazo.

## <a name="next-steps"></a>Pasos siguientes

En función del producto y el enfoque elegidos, los pasos siguientes sugeridos son:

* **Azure IoT Central**: [¿Qué es Azure IoT Central?](overview-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* **Aceleradores de soluciones IoT**: [¿Qué son los aceleradores de soluciones de Azure IoT?](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md)
* **IoT Hub**: [¿Qué es Azure IoT Hub?](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)