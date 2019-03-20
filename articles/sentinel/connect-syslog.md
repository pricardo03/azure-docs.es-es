---
title: Recopilar datos de Syslog en versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo recopilar datos de Syslog en Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 55949da97f58f1d8c1670f69d25e92d6bb4e9eef
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447309"
---
# <a name="connect-your-external-solution-using-syslog"></a>Conecte su solución externa mediante Syslog

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar un dispositivo local que admita Syslog a Centinela de Azure. Esto se realiza mediante un agente basado en una máquina Linux entre el dispositivo y Azure Sentinel. Si su equipo Linux está en Azure, puede transmitir los registros del dispositivo o aplicación a un área de trabajo dedicada, cree en Azure y conectarla. Si su equipo Linux no está en Azure, puede transmitir los registros de su dispositivo al dedicada en el entorno local o máquina virtual donde vaya a instalar el agente para Linux. 

> [!NOTE]
> Si el dispositivo es compatible con CEF Syslog, la conexión es más completa y debe elegir esta opción y siga las instrucciones de [recopilar datos de CEF](connect-common-event-format.md).

## <a name="how-it-works"></a>Cómo funciona

Recopilación de Syslog se logra mediante un agente para Linux. De forma predeterminada, el agente para Linux recibe eventos desde el demonio de Syslog a través de UDP, pero en casos donde se espera una máquina Linux para recopilar un gran volumen de eventos de Syslog, como cuando un agente de Linux recibe eventos de otros dispositivos, se modifica la configuración en Use el transporte TCP entre el demonio de Syslog y el agente.

## <a name="connect-your-syslog-appliance"></a>Conecte el dispositivo de Syslog

1. En el portal de Azure Sentinel, seleccione **la recopilación de datos** y elija el **Syslog** icono.
2. Si su equipo Linux no está dentro de Azure, descargue e instale el Azure Sentinel **agente para Linux** en su dispositivo. 
1. Si está trabajando en Azure, seleccione o cree una máquina virtual que en el área de trabajo Centinela de Azure que está dedicado a recibir mensajes de Syslog. Seleccione la máquina virtual en las áreas de trabajo Centinela de Azure y haga clic en **Connect** en la parte superior del panel izquierdo.
3. Haga clic en **configurar los registros que se recopilan** en la instalación del conector de Syslog. 
4. Haga clic en **presione aquí para abrir la hoja de configuración**.
1. Seleccione **datos** y, a continuación, **Syslog**.
   - Asegúrese de que cada recurso que se va a enviar mediante Syslog está en la tabla. Para cada recurso va a supervisar, establezca un nivel de gravedad. Haga clic en **Aplicar**.
1. En el equipo de Syslog, asegúrese de que se va a enviar las instalaciones. 

3. Para usar el esquema correspondiente en Log Analytics para los registros de Syslog, busque **Syslog**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar dispositivos de Syslog en el entorno local a Azure Sentinel. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
