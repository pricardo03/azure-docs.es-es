---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: 757d6aeecc6c97726e890076b0e27a82a1a45d27
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556707"
---
1. [Conectarse a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use la `Get-HcsApplianceInfo` para obtener la información del dispositivo.

    El ejemplo siguiente muestra el uso de este cmdlet:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Esta es una tabla que resume la información de dispositivo importantes:
    
    | Parámetro                             | DESCRIPCIÓN                                                                                                                                                  |   |
    |--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    | FriendlyName                   | El nombre descriptivo del dispositivo, configurada mediante la interfaz de usuario web local durante la implementación del dispositivo. El nombre descriptivo predeterminado es el número de serie del dispositivo.  |   |
    | SerialNumber                   | El número de serie del dispositivo es un número único asignado en la fábrica.                                                                             |   |
    | Modelo                          | El modelo para el dispositivo de borde del cuadro de datos o la puerta de enlace de datos de cuadro. El modelo es virtual para la puerta de enlace de datos de cuadro y físicos para el borde del cuadro de datos.                   |   |
    | FriendlySoftwareVersion        | La cadena descriptiva que corresponde a la versión de software del dispositivo. Para un sistema que ejecuta la versión preliminar, la versión de software descriptiva sería 1902 de datos de cuadro de borde. |   |
    | HcsVersion                     | La versión de software HCS que se ejecuta en el dispositivo. Por ejemplo, la versión de software HCS correspondiente a los datos de cuadro de borde 1902 es 1.4.771.324.            |   |
    | LocalCapacityInMb              | La capacidad total local del dispositivo en Megabits.                                                                                                        |   |
    | IsRegistered                   | Este valor indica si el dispositivo se activa con el servicio.                                                                                         |   |
    ```

