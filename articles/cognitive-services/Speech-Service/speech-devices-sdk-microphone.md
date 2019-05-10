---
title: Recomendaciones de matriz micrófono Speech SDK de dispositivos - servicios de voz
titleSuffix: Azure Cognitive Services
description: Recomendaciones de matriz de micrófono de Speech SDK de dispositivos. Se recomiendan las geometrías de matriz siguientes para su uso con la pila de Audio de Microsoft. Ubicación de orígenes de sonido y rechazo de ruido ambiental se ha mejorado con un número mayor de micrófonos con dependencias en aplicaciones específicas, escenarios de usuario y el factor de forma de dispositivo.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 63dd64e900cf68e708032569ca75ac2e8b221491
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237005"
---
# <a name="speech-devices-sdk-microphone-array-recommendations"></a>Recomendaciones de matriz de micrófono de SDK de dispositivos de voz

En este artículo, obtendrá información sobre cómo diseñar una matriz de micrófono para el SDK de dispositivos de voz.

El SDK de dispositivos de voz funciona mejor con una matriz de micrófono se ha diseñado según las directrices siguientes, incluida la selección de geometría y componente de micrófono. También tiene instrucciones en la integración y consideraciones eléctricas.

## <a name="microphone-geometry"></a>Geometría de micrófono

Se recomiendan las geometrías de matriz siguientes para su uso con la pila de Audio de Microsoft. Ubicación de orígenes de sonido y rechazo de ruido ambiental se ha mejorado con un número mayor de micrófonos con dependencias en aplicaciones específicas, escenarios de usuario y el factor de forma de dispositivo.

|          | Matriz circular    |       |  Matriz lineal              |                |
|----------|-------------------|-------------------|----------------|----------------|
|          |<img src="media/speech-devices-sdk/7-mic-c.png" alt="7 mic circular array" width="150"/>|<img src="media/speech-devices-sdk/4-mic-c.png" alt="4 mic circular array" width="150"/>|<img src="media/speech-devices-sdk/4-mic-l.png" alt="4 mic linear array" width="150"/>|<img src="media/speech-devices-sdk/2-mic-l.png" alt="2 mic linear array" width="150"/>|
| \# MIC  | 7                 | 4                 | 4              | 2              |
| Geometría | 1 externo, 6 centro, el radio = mm 42,5, espaciado de manera uniforme| 3 1 exterior, centro, el radio = mm 42,5, espaciado de manera uniforme | Longitud = 120 mm, espaciado = 40 mm | Espaciado = 40 mm |

Canales de micrófono deben ordenarse según la numeración se muestra para cada uno por encima de la matriz, aumentando de 0.  La pila de Audio de Microsoft requiere un flujo de referencia adicional de reproducción de audio para realizar la eliminación del eco.

## <a name="component-selection"></a>Selección de componentes

Componentes de micrófono deben seleccionarse para reproducir con precisión una señal libre de ruido y la distorsión.

Las propiedades recomendadas cuando se selecciona micrófonos son:

| Parámetro                         | Recomendada                       |
|-----------------------------------|-----------------------------------|
| SNR                               | \> 65 dB (1 kHz señal 94 dBSPL, ruido ponderado)   |
| Coincidencia de Amplitude                | ± 1 dB @ 1 kHz                     |
| Fase de coincidencia                    | ± 2° @ 1 kHz                       |
| Punto de sobrecarga acústicos (AOP)     | \> dBSPL 120 (THD = 10%)          |
| Velocidad de bits                          | Mínimo de 24 bits                    |
| Frecuencia de muestreo                     | Mínimo kHz 16\*                   |
| Direccionalidad                       | Multidireccionales                   |
| Respuesta de frecuencia                | + 3 dB, 8000-200 Hz flotante máscara\*|
| Confiabilidad                       | Intervalo de temperatura de almacenamiento-40 ° C a 70 ° C<br />Intervalo de temperatura de funcionamiento-20 ° C a 55 ° C  |

*\*Pueden ser necesarios para aplicaciones de comunicaciones de alta calidad (VoIP) mayores tasas de muestreo o de intervalos de frecuencia "mayor"*

Selección de componentes buena debe estar emparejado con buena integración electroacústicos con el fin de evitar perjudicando el rendimiento de los componentes usados. Casos de uso único también es posible que necesite requisitos adicionales (por ejemplo: funcionamiento de temperatura).

## <a name="microphone-array-integration"></a>Integración de la matriz de micrófono

El rendimiento de las matrices cuando se integra en un dispositivo y después de cualquier ganancia fijo o EQ debe cumplir las siguientes recomendaciones:

|  Parámetro        |    Recomendada |
|--------------------|----------------------------------------------------|
|  SNR                 | \> 65 dB (1 kHz señal 94 dBSPL, ruido ponderado) |
|  Sensibilidad de salida  | -dBFS 26/Pa @ 1 kHz (recomendado) |
|  Coincidencia de Amplitude  | ± 2 dB, 200-8000 Hz |
|  Fase de coincidencia      | ± 5°, 200-8000 Hz |
| THD %                 | 1 ≤ %, 200-8000 Hz, 94 dBSPL, 5 º orden |
|  Respuesta de frecuencia  | + 6 dB, 8000-200 Hz flotante máscara\* |

*\*Intervalos de frecuencia "Mayor" pueden que sea necesarios para las aplicaciones de comunicaciones de alta calidad (VoIP)*

## <a name="speaker-integration-recommendations"></a>Recomendaciones de integración de orador

Como eco acústico es necesario para los dispositivos de reconocimiento de voz que contienen los altavoces, se proporcionan recomendaciones adicionales para la integración y la selección del orador.

| Parámetro                         | Recomendada                       |
|-----------------------------------|-----------------------------------|
| Consideraciones de linealidad          | Ningún procesamiento no lineal después de la referencia de speaker, en caso contrario, se requiere una secuencia de referencia de bucle invertido basada en hardware  |
| Bucle invertido de orador                  | Proporciona a través de WASAPI, las API privadas, complemento ALSA personalizada (Linux), o proporciona a través de canal de firmware      |
| THD %                              | 3ª bandas octava 5ª pedido mínimo, 70 dBA reproducción @ 0,8 m ≤ 6.3%, de 315-500 Hz ≤ 5%, 630 5000 Hz                 |
| Acoplamiento de eco micrófonos      | \> dB-10 TCLw mediante el método de ITU-T G.122 anexo B.4, normalizado a nivel de mic<br />TCLw = TCLwmeasured \+ (medido nivel - sensibilidad de la salida de destino)<br />TCLw = TCLwmeasured \+ (medido nivel - (-26)) |

## <a name="integration-design-architecture"></a>Diseño de arquitectura de integración

Las siguientes directrices sobre arquitectura son necesarias al integrar micrófonos en un dispositivo:

| Parámetro                         | Recomendación                    |
|-----------------------------------|-----------------------------------|
| Similitud de puerto de MIC               | Todos los puertos de micrófono tienen la misma longitud de matriz    |
| Dimensiones del puerto de MIC               | Puerto tamaño Ø0.8 1.0 mm. Longitud de puerto del puerto diámetro \< 2              |
| Sellado de MIC                       | Sellar juntas aplican de manera uniforme en la pila de copia de seguridad. Recomienda \> razón de compresión de un 70% de juntas de espuma     |
| Confiabilidad de MIC                   | Malla debe usarse para evitar el polvo y la entrada (entre PCB para inferior portar micrófonos y sellar portada juntas o superior)  |
| Aislamiento de MIC                     | Las juntas de goma y el desacoplamiento de vibración a través de la estructura, especialmente para aislar las rutas de acceso de vibración debido a los altavoces integrados      |
| Reloj de muestreo                    | Audio de dispositivo debe estar libre de vibración y abandonos con desfase baja    |
| Capacidad de registro                 | El dispositivo debe poder grabar secuencias de canal individual sin procesar al mismo tiempo |
| USB                               | Todos los dispositivos entrados audio de USB deben establecer los descriptores de acuerdo con la [especificación de Rev3 de dispositivos de Audio USB](https://www.usb.org/document-library/usb-audio-devices-rev-30-and-adopters-agreement) |
| Geometría de micrófono               | Deben implementar controladores [descriptores de geometría de matriz de micrófono](https://docs.microsoft.com/windows-hardware/drivers/audio/ksproperty-audio-mic-array-geometry) correctamente  |
| Detectabilidad                   | Dispositivos no deben tener ningún imprevisibles o incontrolable hardware, firmware o 3rd algoritmos de procesamiento de audio no lineal basado en software de terceros desde el dispositivo|
| Formato de captura                    | Formatos de capture deben usar una frecuencia de muestreo mínimo de 16 kHz y profundidad de 24 bits recomendada      |

## <a name="electrical-architecture-considerations"></a>Consideraciones sobre la arquitectura eléctrica

Si procede, las matrices pueden estar conectadas a un host USB (por ejemplo, un SoC que se ejecuta la pila de Audio de Microsoft) y las interfaces de servicios de voz u otras aplicaciones.

Componentes de hardware como la conversión de PDM a TDM deben asegurarse de que se conserve el intervalo dinámico y SNR de los micrófonos dentro de muestras de nuevo.

Alta velocidad USB 2.0 Audio de clase debe ser compatible dentro de cualquier MCU de audio con el fin de proporcionar el ancho de banda necesario para hasta siete canales a mayores velocidades de muestreo y la profundidad de bits.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el SDK de dispositivos de voz](speech-devices-sdk.md)
