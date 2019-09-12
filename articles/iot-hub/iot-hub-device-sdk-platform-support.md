---
title: Compatibilidad de plataformas de SDK de dispositivo IoT de Azure | Microsoft Docs
description: 'Conceptos: lista de plataformas compatibles con los SDK de dispositivo IoT de Azure'
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: robinsh
ms.openlocfilehash: 9a64c6e476910ab4fe983fa949680f05fdded3ae
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70161858"
---
# <a name="azure-iot-sdks-platform-support"></a>Compatibilidad de plataformas de SDK de IoT de Azure

Los [SDK de IoT de Azure](iot-hub-devguide-sdks.md) son un conjunto de bibliotecas para interactuar con IoT Hub y el servicio Device Provisioning con amplia compatibilidad de lenguajes y plataformas. Los SDK se ejecutan en la mayoría de las plataformas, y los desarrolladores pueden trasladar el SDK de C una plataforma específica siguiendo la [guía de migración](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md). 

Microsoft admite una variedad de sistemas operativos, plataformas y marcos de trabajo y se puede extender mediante el SDK de C de IoT de Azure. Algunos reciben soporte oficial por el equipo, agrupados en niveles que representan el nivel de compatibilidad de los usuarios pueden esperar. Las *plataformas completamente compatibles* implican que Microsoft:

- Compila y ejecuta pruebas de un extremo a otro continuamente sobre la versión maestra y otras versiones admitidas de LTS.  Para proporcionar cobertura de pruebas en las distintas versiones, se suele realizar una prueba en comparación con la última versión de LTS y la versión más popular.  Otras versiones de la misma plataforma pueden admitirse mediante la compatibilidad de versiones de plataforma.
- Proporciona instrucciones de instalación o paquetes si corresponde.
- Es totalmente compatible con las plataformas en GitHub.

Además, diversos socios han trasladado nuestro SDK de C a más plataformas y se ocupan del mantenimiento de la capa de abstracción de plataforma (PAL). El [catálogo de dispositivos Azure Certified for IoT](https://catalog.azureiotsolutions.com/) también presenta una lista de las plataformas de sistemas operativos con las que se han probado los distintos SDK. Los SDK también se compilan periódicamente en estas plataformas, con pruebas y soporte limitados:

* MBED2
* Arduino
* Windows CE 2013 (en desuso en octubre de 2018)
* .NET Standard 1.3 con .NET Core 2.1 y .NET Framework 4.7
* Xamarin iOS, Android, UWP

## <a name="supported-platforms"></a>Plataformas compatibles

Hay varias plataformas compatibles.

### <a name="c-sdk"></a>SDK DE C

| OS                  | Arco | Compilador             | Biblioteca TLS       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | gcc-5.4.0            | openssl  - 1.0.2g |
| Ubuntu 18.04 LTS    | X64  | gcc-7.3              | WolfSSL – 1.13    |
| Ubuntu 18.04 LTS    | X64  | Clang 6.0.X          | Openssl – 1.1.0g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | OSX nativo        |
| Windows Server 2016 | x64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Stretch    | x64  | gcc-7.3              | Openssl – 1.1.0f  |

### <a name="python-sdk"></a>SDK de Python

| OS                  | Arco | Compilador   | Biblioteca TLS |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2.7 | openssl     |
| Windows Server 2016 | x64  | Python 2.7 | openssl     |
| Windows Server 2016 | x86  | Python 3.5 | openssl     |
| Windows Server 2016 | x64  | Python 3.5 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 2.7 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 3.4 | openssl     |
| MacOS High Sierra   | x64  | Python 2.7 | openssl     |

### <a name="net-sdk"></a>.NET SDK

| OS                  | Arco | marco            | Estándar          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET Core 2.1        | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Core 2.1        | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.7   | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.5.1 | N/D               |

### <a name="nodejs-sdk"></a>SDK de Node.js

| OS                                           | Arco | Versión de nodo    |
|----------------------------------------------|------|-----------------|
| Ubuntu 16.04 LTS (mediante la imagen de docker de nodo 6) | X64  | LTS y Current |
| Windows Server 2016                          | X64  | LTS y Current |

### <a name="java-sdk"></a>SDK de Java

| OS                  | Arco | Versión de Java |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |
| Android API 28 | X64  | Java 8       |
| Android Things | X64  | Java 8      |

## <a name="partner-supported-platforms"></a>Plataformas de asociados compatibles

Los clientes pueden extender el soporte de plataforma. Para ello, deben portar el SDK de C de Azure IoT, en concreto, mediante la creación de la capa de abstracción de plataforma (PAL) del SDK. Microsoft trabaja con sus asociados para proporcionar soporte técnico ampliado. Una lista de asociados ha portado el SDK de C a más plataformas y mantiene la PAL.

| Asociado             | Dispositivos                            | Vínculo                     | Soporte técnico |
|---------------------|------------------------------------|--------------------------|---------|
| Espressif           | ESP32 <br/> ESP8266                              | [Esp-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  
| Qualcomm            | Qualcomm MDM9206 LTE IoT Modem     | [Qualcomm LTE for IoT SDK](https://developer.qualcomm.com/software/lte-iot-sdk) | [Foro](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4 Series <br/> STM32F4 Series <br/>  STM32F7 Series <br/>  STM32L4 Discovery Kit for IoT node    | [X-CUBE-CLOUD](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html) <br/> [X-CUBE-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html) <br/> [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html) <br/> [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            | [Soporte técnico](https://www.st.com/content/st_com/en/support/support-home.html)
| Texas Instruments   | CC3220SF LaunchPad </br> CC3220S LaunchPad </br> CC3235SF LaunchPad </br> CC3235S LaunchPad </br> MSP432E4 LaunchPad | [Complemento de Azure IoT para SimpleLink](https://github.com/TexasInstruments/azure-iot-pal-simplelink) | [TI E2E Forum](https://e2e.ti.com) <br/> [TI E2E Forum for CC3220](https://e2e.ti.com/support/wireless_connectivity/simplelink_wifi_cc31xx_cc32xx/) <br/> [TI E2E Forum for MSP432E4](https://e2e.ti.com/support/microcontrollers/msp430/) |

## <a name="next-steps"></a>Pasos siguientes

* [SDK de dispositivos y servicios](iot-hub-devguide-sdks.md)
* [Porting Guidance](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md) (Guía de migración)
