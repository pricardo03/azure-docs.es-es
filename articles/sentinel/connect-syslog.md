---
title: Conectar datos de Syslog a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Syslog a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: dad74410562aa54aeb61675e7dc1c0adccc44797
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679202"
---
# <a name="connect-your-external-solution-using-syslog"></a>Conectar su solución externa mediante Syslog

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar a Azure Sentinel cualquier dispositivo local que admita Syslog. Esto se realiza a través de un agente basado en una máquina Linux entre el dispositivo y Azure Sentinel. Si la máquina Linux está en Azure, puede transmitir los registros desde el dispositivo o la aplicación a un área de trabajo dedicada que cree en Azure y conectarla. Si la máquina Linux no está en Azure, puede transmitir los registros desde el dispositivo a una máquina virtual local dedicada o a un equipo donde tenga instalado el Agente para Linux. 

> [!NOTE]
> Si el dispositivo admite CEF de Syslog, la conexión será más completa, así que conviene elegir esta opción y seguir las instrucciones en [Conectar su solución externa mediante Syslog](connect-common-event-format.md).

## <a name="how-it-works"></a>Cómo funciona

Syslog es un protocolo de registro de eventos que es común a Linux. Las aplicaciones envían mensajes que pueden almacenarse en la máquina local o entregarse a un recopilador de Syslog. Al instalar el agente de Log Analytics para Linux, este configura el demonio Syslog local para que reenvíe mensajes al agente. En ese momento, el agente envía el mensaje a Azure Monitor, donde se crea un registro correspondiente.

Para más información, consulte [Orígenes de datos de Syslog en Azure Monitor](../azure-monitor/platform/data-sources-syslog.md).

> [!NOTE]
> El agente puede recopilar registros de varios orígenes, pero debe estar instalado en el equipo proxy dedicado.

## <a name="connect-your-syslog-appliance"></a>Conectar el dispositivo de Syslog

1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos), seleccione la línea **Syslog** en la tabla y, en el panel de Syslog situado a la derecha, haga clic en **Open connector page** (Abrir la página del conector).
2. Si la máquina Linux se encuentra en Azure, seleccione **Download and install agent on Azure Linux virtual machine** (Descargar e instalar el agente en una máquina virtual Linux de Azure). En la ventana Máquinas virtuales, seleccione las máquinas en las que quiere instalar el agente y haga clic en **Conectar** en la parte superior.
1. Si la máquina Linux no se encuentra en Azure, seleccione **Download and install agent on Linux non-Azure machine** (Descargar e instalar el agente en una máquina Linux que no es de Azure). En la ventana **Agente Directo**, copie el comando situado bajo **Descargar e incorporar Agent para Linux** y ejecútelo en el equipo. 
   > [!NOTE]
   > Asegúrese de configurar la seguridad de la máquina de acuerdo con la directiva de seguridad de su organización. Por ejemplo, puede configurar la red para que se alinee con la directiva de seguridad de la red corporativa y cambiar los puertos y protocolos del demonio para que se adapten a sus requisitos. 

1. En **Configure the logs to be connected** (Configurar los registros que se van a conectar), en la ventana de configuración del conector de Syslog, siga las instrucciones:
    1. Haga clic en el vínculo **Open your workspace advanced settings configuration** (Abrir la configuración de las opciones avanzadas del área de trabajo). 
    1. Seleccione **Datos**, seguido de **Syslog**.
    1. Después, configure en la tabla los recursos que quiere que recopile Syslog. Debe agregar o seleccionar los recursos que su dispositivo de Syslog incluye en sus encabezados de registro. Puede ver esta configuración en el dispositivo de Syslog en Syslog-d en la carpeta /etc/rsyslog.d/security-config-omsagent.conf y en r-Syslog en /etc/syslog-ng/security-config-omsagent.conf. 
       > [!NOTE]
       > Si activa la casilla **Aplicar la configuración siguiente a mis máquinas**, esta configuración se aplicará a todas las máquinas Linux conectadas a esta área de trabajo. Puede ver esta configuración en la máquina de Syslog en 
1. Haga clic en **Press here to open the configuration blade** (Presione aquí para abrir la hoja de configuración).
1. Seleccione **Data** (Datos) y, después, **Syslog**.
   - Asegúrese de que cada recurso que se va a enviar mediante Syslog figura en la tabla. Establezca un nivel de gravedad en cada recurso que vaya a supervisar. Haga clic en **Aplicar**.
1. En la máquina Syslog, asegúrese de que va a enviar esas instalaciones. 

1. Para usar el esquema correspondiente en Log Analytics para encontrar registros de Syslog, busque **Syslog**.
1. Puede usar la función de Kusto que se describe en [Uso de funciones en consultas de registros de Azure Monitor](../azure-monitor/log-query/functions.md) para analizar los mensajes de Syslog y guardarlos como una nueva función de Log Analytics y, luego, usar la función como un nuevo tipo de datos.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos locales de Syslog a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
