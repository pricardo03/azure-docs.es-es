---
title: Cifrado de datos en reposo de Azure IoT Hub a través de claves administradas por el cliente | Microsoft Docs
description: Cifrado de datos en reposo con claves administradas por el cliente para IoT Hub
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: asrastog
ms.openlocfilehash: 1bb55d593878026bb3e57014a317b4fc0158d734
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2020
ms.locfileid: "75913183"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>Cifrado de datos en reposo con claves administradas por el cliente para IoT Hub

IoT Hub admite el cifrado de datos en reposo con claves administradas por el cliente (CMK), también conocido como Bring your own key (BYOK), para Azure IoT Hub. Azure IoT Hub ofrece cifrado de datos en reposo y en tránsito. De manera predeterminada, IoT Hub usa claves administradas por Microsoft para cifrar los datos. Gracias a la compatibilidad con CMK, ahora los clientes tienen la opción de cifrar los datos en reposo con una clave de cifrado (administrada por los clientes) mediante [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Para usar esta funcionalidad, debe crear una nueva instancia de IoT Hub (nivel básico o estándar) en una de las siguientes regiones: Este de EE. UU., Oeste de EE. UU. 2 o Centro-sur de EE. UU. Para probar esta funcionalidad, póngase en contacto con nosotros a través del [soporte técnico de Microsoft](https://azure.microsoft.com/support/create-ticket/). Mencione el nombre de la empresa y el Id. de suscripción cuando se ponga en contacto con el soporte técnico de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre IoT Hub](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)