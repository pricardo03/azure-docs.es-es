---
title: Comprender ASC para recomendaciones de seguridad de IoT Preview | Microsoft Docs
description: Obtenga información sobre el concepto de recomendaciones de seguridad y cómo se usan en ASC para IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 1e4582d93d1e3380ecdabdb241f27839d4da4565
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541863"
---
# <a name="security-recommendations"></a>Recomendaciones de seguridad

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En función de análisis de la solución en curso, ASC para IoT proporciona las siguientes recomendaciones cuando sea necesario para ayudar a mejorar y proteger los dispositivos, el estado operativo y el entorno general de IoT Hub. 


## <a name="device-recommendations"></a>Recomendaciones de dispositivo

Recomendaciones de dispositivos proporcionan información y sugerencias para mejorar la seguridad de los dispositivos y de comportamiento. 

| Gravedad | NOMBRE                                                      | Origen de datos | DESCRIPCIÓN                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mediano   | Abrir puertos en el dispositivo                                      | Agente       | Se ha encontrado un punto de conexión de escucha en el dispositivo.                                                                                                                                                          |
| Mediano   | Se encuentra en una de las cadenas de la directiva de firewall permisiva. | Agente       | Permite la directiva de firewall se encuentra (entrada/salida). Directiva de Firewall debe denegar todo el tráfico de forma predeterminada y definir reglas para permitir la comunicación necesaria desde el dispositivo.                               |
| Mediano   | Se encontró la regla de firewall permisiva en la cadena de entrada     | Agente       | Se ha encontrado una regla en el servidor de seguridad que contiene un patrón permisivo para una amplia gama de direcciones IP o puertos.                                                                                    |
| Mediano   | Se encontró la regla de firewall permisiva en la cadena de salida    | Agente       | Se ha encontrado una regla en el servidor de seguridad que contiene un patrón permisivo para una amplia gama de direcciones IP o puertos.                                                                                   |
| Mediano   | Error de validación de línea de base de sistema de operación           | Agente       | Dispositivo no es compatible con [pruebas comparativas CIS Linux](https://www.cisecurity.org/cis-benchmarks/)                                                                                                         |

### <a name="operational-recommendation"></a>Recomendación operativa

Recomendaciones operativas proporcionan información y sugerencias para mejorar la configuración del agente.

| Gravedad | NOMBRE                                    | Origen de datos | DESCRIPCIÓN                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Bajo      | El agente envía los mensajes no utilizados          | Agente       | 10% o más de los mensajes de seguridad eran menores de 4kb durante las últimas 24 horas.  |
| Bajo      | Configuración de seguridad gemelo no óptima | Agente       | Configuración de seguridad gemelo no es óptima.                                        |
| Bajo      | Conflicto de configuración de seguridad gemelo    | Agente       | Se identificaron los conflictos en la configuración de seguridad gemelo.                           |


## <a name="iot-hub-recommendations"></a>Recomendaciones de IoT Hub

Las alertas de recomendación proporcionan información y sugerencias para las acciones mejorar la posición de seguridad de su entorno.  

| Gravedad | NOMBRE                                                     | Origen de datos | DESCRIPCIÓN                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alto     | Credenciales de autenticación idénticas utilizadas por varios dispositivos | IoT Hub     | Las credenciales de autenticación de IoT Hub utilizan varios dispositivos. Esto puede indicar un dispositivo ilegítimo suplantando a un dispositivo legítimo. Uso de credenciales duplicadas aumenta el riesgo de suplantación del dispositivo por un individuo malintencionado. |
| Mediano   | Se debe denegar la directiva de filtro IP predeterminada                  | IoT Hub     | Configuración del filtro IP debe tener reglas definidas para el tráfico permitido y debe de forma predeterminada, se deniegan el tráfico de todos los demás de forma predeterminada.                                                                                                     |
| Mediano   | Regla de filtro IP incluye gran variedad IP                   | IoT Hub     | Un intervalo de IP de origen de regla de permitir IP filtro es demasiado grande. Reglas excesivamente permisivas pueden exponer su centro de IoT a actores malintencionados.                                                                                       |
| Bajo      | Habilitar registros de diagnóstico en IoT Hub                       | IoT Hub     | Habilite los registros y consérvelos por hasta un año. Retención de registros le permite volver a crear seguimientos de actividad para fines de investigación cuando se produce un incidente de seguridad o se pone en peligro su red.                                       |
|