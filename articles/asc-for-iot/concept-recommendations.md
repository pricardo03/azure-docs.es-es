---
title: Descripción de las recomendaciones de seguridad de Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Sepa en qué consiste el concepto de recomendaciones de seguridad y cómo se usan en Azure Security Center for IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 122a1585abf43d1aedc7f329cd7e41a6228f8c3e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200672"
---
# <a name="security-recommendations"></a>Recomendaciones de seguridad

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Security Center (ASC) for IoT analiza los dispositivos IoT y los recursos de Azure y hace recomendaciones de seguridad para reducir la superficie expuesta a ataques. Las recomendaciones de seguridad requieren una acción y tienen como objetivo ayudar a los clientes a cumplir los procedimientos recomendados de seguridad existentes.

En este artículo, encontrará una lista de recomendaciones que se pueden activar en IoT Hub o en sus dispositivos IoT.

## <a name="recommendations-for-iot-devices"></a>Recomendaciones para dispositivos IoT

Las recomendaciones de dispositivo proporcionan información y sugerencias para mejorar la posición de seguridad de los dispositivos. 

| Gravedad | NOMBRE                                                      | Origen de datos | DESCRIPCIÓN                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mediano   | Puertos abiertos en el dispositivo                                      | Agente       | Se encontró un punto de conexión de escucha en el dispositivo.                                                                                                                                                          |
| Mediano   | Se encontró una directiva de firewall permisiva en una de las cadenas. | Agente       | Se encontró una directiva de firewall permitida en las cadenas de firewall principales (ENTRADA/SALIDA). La directiva debe denegar todo el tráfico de manera predeterminada y definir reglas para permitir la comunicación necesaria hacia y desde el dispositivo.                               |
| Mediano   | Se encontró una regla de firewall permisiva en la cadena de entrada     | Agente       | Se encontró una regla en el firewall que contiene un patrón permisivo para una amplia gama de direcciones IP o puertos.                                                                                    |
| Mediano   | Se encontró una regla de firewall permisiva en la cadena de salida    | Agente       | Se encontró una regla en el firewall que contiene un patrón permisivo para una amplia gama de direcciones IP o puertos.                                                                                   |
| Mediano   | Error de validación de línea base de sistema operativo           | Agente       | El dispositivo no cumple las [pruebas comparativas de CIS Linux](https://www.cisecurity.org/cis-benchmarks/)                                                                                                         |

### <a name="operational-recommendations-for-iot-devices"></a>Recomendaciones operativas para dispositivos IoT

Las recomendaciones operativas proporcionan información y sugerencias para mejorar la configuración de los agentes de seguridad.

| Gravedad | NOMBRE                                    | Origen de datos | DESCRIPCIÓN                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Bajo      | El agente envía mensajes sin utilizar          | Agente       | 10 % o más de los mensajes de seguridad tenían un tamaño inferior a 4 KB durante las últimas 24 horas.  |
| Bajo      | Configuración de gemelo de seguridad no óptima | Agente       | La configuración del gemelo de seguridad no es óptima.                                        |
| Bajo      | Conflicto de configuración de gemelo de seguridad    | Agente       | Se identificaron conflictos en la configuración del gemelo de seguridad.                           |


## <a name="recommendations-for-iot-hub"></a>Recomendaciones para IoT Hub

Las alertas de recomendación proporcionan información y sugerencias relativas a las acciones para mejorar la posición de seguridad del entorno.  

| Gravedad | NOMBRE                                                     | Origen de datos | DESCRIPCIÓN                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alto     | Credenciales de autenticación idénticas utilizadas por varios dispositivos | IoT Hub     | Hay varios dispositivos que usan las mismas credenciales de autenticación de IoT Hub. Esto puede indicar que hay un dispositivo ilegítimo que suplanta un dispositivo legítimo. El uso de credenciales duplicadas aumenta el riesgo de suplantación del dispositivo a manos de un individuo malintencionado. |
| Mediano   | La directiva del filtro de IP predeterminada debe ser Denegar.                  | IoT Hub     | La configuración del filtro IP debe tener reglas definidas para el tráfico permitido y debe denegar el resto del tráfico de forma predeterminada.                                                                                                     |
| Mediano   | La regla del filtro IP incluye un amplio intervalo de IP                   | IoT Hub     | Un intervalo IP de origen de la regla de filtro IP permitido es demasiado grande. Las reglas excesivamente permisivas podrían exponer su instancia de IoT Hub a agentes malintencionados.                                                                                       |
| Bajo      | Habilitar registros de diagnóstico en IoT Hub                       | IoT Hub     | Habilite los registros y consérvelos por hasta un año. Conservar los registros le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.                                       |
|