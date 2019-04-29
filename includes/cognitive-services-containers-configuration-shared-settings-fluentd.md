---
ms.openlocfilehash: ffd17f7a641e1481aa4c88f8b2eb12ec11fa7d8b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60599376"
---
Fluentd es un recopilador de datos de código abierto para el registro unificado. La opción de configuración `Fluentd` administra la conexión del contenedor a un servidor de [Fluentd](https://www.fluentd.org). En el contenedor, se incluye un proveedor de registros de Fluentd que permite que el contenedor escriba los registros y, opcionalmente, los datos de métricas en un servidor de Fluentd.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `Fluentd`.

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|------|-----------|-------------|
| `Host` | String | Dirección IP o nombre de host DNS del servidor de Fluentd. |
| `Port` | Entero | Puerto del servidor de Fluentd.<br/> El valor predeterminado es 24 224. |
| `HeartbeatMs` | Entero | Intervalo de latidos (en milisegundos). Si no se envía ningún tráfico de evento antes de que este intervalo expire, se envía un latido al servidor de Fluentd. El valor predeterminado es 60 000 milisegundos (1 minuto). |
| `SendBufferSize` | Entero | Espacio en búfer de red (en bytes) asignado para las operaciones de envío. El valor predeterminado es 32 768 bytes (32 kilobytes). |
| `TlsConnectionEstablishmentTimeoutMs` | Entero | Tiempo de expiración (en milisegundos) para establecer una conexión SSL/TLS con el servidor de Fluentd. El valor predeterminado es 10 000 milisegundos (10 segundos).<br/> Si `UseTLS` se establece en false, este valor se ignora. |
| `UseTLS` | Boolean | Indica si el contenedor debe utilizar SSL/TLS para comunicarse con el servidor de Fluentd. El valor predeterminado es false. |