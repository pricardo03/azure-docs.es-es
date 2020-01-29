---
title: Conectividad de dispositivos en Azure IoT Central | Microsoft Docs
description: En este artículo se proporciona un resumen de cómo Azure IoT Central le ayuda a mantener los dispositivos conectados y en buen estado.
author: aaronbjork
ms.author: abjork
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: ''
ms.openlocfilehash: 7c102e7ffde7faab5d864c1d9acaafe141664ebf
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76119557"
---
# <a name="stay-connected-with-azure-iot-central-preview-features"></a>Manténgase conectado con Azure IoT Central (características en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En este artículo se proporciona información general sobre cómo Azure IoT Central ayuda a mantener los dispositivos conectados y en buen estado.

Con cualquier solución de IoT diseñada para funcionar a escala, es importante tener un enfoque bien estructurado en relación con la administración de dispositivos. No basta con que los dispositivos se conecten a la nube. Se necesita un método para mantenerlos conectados y en buen estado a medida que la solución evoluciona, crece y envejece. Azure IoT Central proporciona las funcionalidades necesarias para asegurarse de que los dispositivos de la solución de IoT estén bien atendidos durante todo el ciclo de vida de la aplicación.

## <a name="dashboards"></a>Paneles 
Los [paneles](howto-manage-devices.md#import-devices) integrados proporcionan un área expuesta personalizable para supervisar el estado de los dispositivos y la telemetría. Comience con un panel prediseñado a partir de una [plantilla de aplicación](howto-use-app-templates.md) o cree sus propios paneles adaptados a las necesidades de la aplicación. Los paneles se pueden compartir con todos los usuarios de la aplicación o mantenerse privados.

## <a name="rules-and-actions"></a>Reglas y acciones 
Identifique rápidamente los dispositivos que necesitan atención mediante la creación de [reglas personalizadas](tutorial-create-telemetry-rules.md) basadas en el estado del dispositivo y la telemetría. Configure acciones para notificar a las personas adecuadas que garantizan que las medidas correctivas se producen de manera oportuna.

## <a name="jobs"></a>Trabajos 
Los [trabajos](../core/howto-run-a-job.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) permiten realizar actualizaciones individuales o masivas en las propiedades, la configuración y los comandos de un dispositivo. 

## <a name="user-roles-and-permissions"></a>Roles y permisos de usuario
Los [roles y permisos](howto-manage-users-roles.md) permiten adaptar la experiencia de cada usuario. Cree roles directamente a través de la interfaz de usuario y asigne fácilmente los permisos adecuados. 




