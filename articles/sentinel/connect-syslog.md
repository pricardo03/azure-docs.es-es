---
title: Conectar datos de Syslog a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Syslog a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 673b1df6094703bebcbfd9d82c1268c01d46e814
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233588"
---
# <a name="connect-your-external-solution-using-syslog"></a>Conectar su solución externa mediante Syslog

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar a Azure Sentinel cualquier dispositivo local que admita Syslog. Esto se realiza a través de un agente basado en una máquina Linux entre el dispositivo y Azure Sentinel. Si la máquina Linux está en Azure, puede transmitir los registros desde el dispositivo o la aplicación a un área de trabajo dedicada que cree en Azure y conectarla. Si la máquina Linux no está en Azure, puede transmitir los registros desde el dispositivo a una máquina virtual local dedicada o a un equipo donde tenga instalado el Agente para Linux. 

> [!NOTE]
> Si el dispositivo admite CEF de Syslog, la conexión será más completa, así que conviene elegir esta opción y seguir las instrucciones en [Conectar su solución externa mediante Syslog](connect-common-event-format.md).

## <a name="how-it-works"></a>Cómo funciona

La conexión de Syslog se logra mediante un Agente para Linux. El Agente para Linux recibe eventos del demonio de Syslog a través de UDP; pero en los casos en los que se espera que una máquina Linux recopile un gran volumen de eventos Syslog, como cuando un Agente para Linux recibe eventos de otros dispositivos, la configuración se modifica para que use el transporte TCP entre el demonio de Syslog y el Agente.

## <a name="connect-your-syslog-appliance"></a>Conectar el dispositivo de Syslog

1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y elija el icono de **Syslog**.
2. Si la máquina Linux no está en Azure, descargue e instale el **Agente para Linux** de Azure Sentinel en el dispositivo. 
1. Si está trabajando en Azure, seleccione o cree una máquina virtual en el área de trabajo de Azure Sentinel que esté dedicada a recibir mensajes de Syslog. Seleccione la máquina virtual en las áreas de trabajo de Azure Sentinel y haga clic en **Connect** (Conectar) en la parte superior del panel izquierdo.
3. Haga clic en la opción **Configure the logs to be connected** (Configurar los registros que se van a conectar) de la configuración del conector de Syslog. 
4. Haga clic en **Press here to open the configuration blade** (Presione aquí para abrir la hoja de configuración).
1. Seleccione **Data** (Datos) y, después, **Syslog**.
   - Asegúrese de que cada recurso que se va a enviar mediante Syslog figura en la tabla. Establezca un nivel de gravedad en cada recurso que vaya a supervisar. Haga clic en **Aplicar**.
1. En la máquina Syslog, asegúrese de que va a enviar esas instalaciones. 

3. Para usar el esquema correspondiente en Log Analytics para encontrar registros de Syslog, busque **Syslog**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos locales de Syslog a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
