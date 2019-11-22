---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: c18e40b8950d5e75398382a41cf0acc3505ea5a6
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74092991"
---
## <a name="configure-the-sensor-fingerprint-provider"></a>Configuración del proveedor de huellas digitales del sensor

Para empezar, es preciso crear y configurar un proveedor de huellas digitales del sensor. Este se encargará de leer los sensores específicos de la plataforma en el dispositivo y de omitirlos para lograr una representación común, que es la que usa la sesión del delimitador en la nube.