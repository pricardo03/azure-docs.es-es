---
title: Solución de problemas de los dispositivos de Speech SDK - servicios de voz
titleSuffix: Azure Cognitive Services
description: En este artículo se proporciona información para ayudarle a resolver los problemas que puede surgir al usar el SDK de dispositivos de voz.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: wellsi
ms.openlocfilehash: 87fb35f06dcb1d1e3fb8c3ae3be64c7448162f14
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026161"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Solución de problemas de los dispositivos de Speech SDK

En este artículo se proporciona información para ayudarle a resolver los problemas que puede surgir al usar el SDK de dispositivos de voz.

## <a name="certificate-failures"></a>Errores de certificado

Si recibe errores de certificado al usar los servicios de voz, asegúrese de que el dispositivo tiene la fecha y hora correctas:

1. Vaya a **Configuración**. En **System** (Sistema), seleccione **Date & time** (Fecha y hora).

    ![En Settings (Configuración), seleccione Date & time (Fecha y hora).](media/speech-devices-sdk/qsg-12.png)

1. Mantenga la opción **Automatic date & time** (Fecha y hora automáticas) seleccionada. En **Select time zone** (Seleccionar zona horaria), seleccione la zona horaria actual.

    ![Seleccione las opciones de fecha y zona horaria](media/speech-devices-sdk/qsg-13.png)

    Cuando vea que la hora del kit desarrollo coincide con la hora del equipo, sabrá que el kit de desarrollo está conectado a Internet.

## <a name="next-steps"></a>Pasos siguientes

* [Revise las notas de la versión.](devices-sdk-release-notes.md)
