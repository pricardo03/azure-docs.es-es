---
title: Guía para instalar e implementar el agente de Linux C de ASC para vista previa del agente de IoT | Microsoft Docs
description: Obtenga información sobre cómo instalar el ASC para el agente de IoT en Linux de 32 bits y 64 bits.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: mlottner
ms.openlocfilehash: 2b18a09e5b79e7b3d3ea837e937397ac92491f9f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619856"
---
# <a name="deploy-asc-for-iot-c-based-security-agent-for-linux"></a>Implementar ASC para basados en c. IoT security agent para Linux

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica cómo instalar e implementar el ASC para el agente de seguridad basada en IoT C en Linux.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalar el agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para otras plataformas y versiones de agente, consulte [elegir el agente de seguridad adecuadas](how-to-deploy-agent.md).

1. Para implementar al agente de seguridad, se necesitan derechos de administrador local en el equipo que desea instalar en (sudo).

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para instalar e implementar al agente de seguridad, haga lo siguiente:


1. Descargue la versión más reciente en el equipo de [Github](https://aka.ms/iot-security-github-c).

1. Extraiga el contenido del paquete y navegue hasta la _/instalar_ carpeta.

1. Agregue permisos de ejecución a la **InstallSecurityAgent script** ejecutando lo siguiente:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. A continuación, ejecute: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Consulte [cómo configurar la autenticación](concept-security-agent-authentication-methods.md) para obtener más información acerca de los parámetros de autenticación.

El script hace lo siguiente:

1. Instala los requisitos previos.

2. Agrega un usuario de servicio (con deshabilitado el inicio de sesión interactivo).

3. Instala el agente como un **Daemon** -supone que el dispositivo usa **systemd** para administración de servicios.

4. Configura al agente con los parámetros de autenticación proporcionados. 

Para obtener ayuda adicional, ejecute el script con el parámetro – help: 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>Desinstalar el agente

Para desinstalar el agente, ejecute la secuencia de comandos –-desinstalar parámetro:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>solución de problemas
Compruebe el estado de implementación mediante la ejecución:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Pasos siguientes
- Leer los procesos de ASC para el servicio IoT [información general](overview.md)
- Más información sobre los procesos de ASC para IoT [arquitectura](architecture.md)
- Habilitar la [servicio](quickstart-onboard-iot-hub.md)
- Leer el [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Comprender [las alertas de seguridad](concept-security-alerts.md)