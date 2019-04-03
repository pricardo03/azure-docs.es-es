---
title: Guía para instalar e implementar el agente de Linux C de Azure Security Center para la versión preliminar del agente de IoT | Microsoft Docs
description: Obtenga información sobre cómo instalar el agente de IoT de Azure Security Center en Linux de 32 bits y 64 bits.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: mlottner
ms.openlocfilehash: 0089fd1af6576f9bcdebe4b7f270a573205dea82
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861945"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Implementar Azure Security Center para basados en c. IoT security agent para Linux

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica cómo instalar e implementar Azure Security Center (ASC) para el agente de seguridad basada en IoT C en Linux.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalación del agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para otras plataformas y versiones de agente, consulte [elegir el agente de seguridad adecuadas](how-to-deploy-agent.md).

1. Para implementar al agente de seguridad, se necesitan derechos de administrador local en el equipo que desea instalar en (sudo).

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para instalar e implementar al agente de seguridad, realice las siguientes operaciones:


1. Descargue en el equipo la versión más reciente desde [GitHub](https://aka.ms/iot-security-github-c).

1. Extraiga el contenido del paquete y vaya a la carpeta _/Install_.

1. Agregue permisos de ejecución al **script de InstallSecurityAgent**, para lo que debe ejecutar lo siguiente:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Después, ejecute: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Para más información acerca de los parámetros de autenticación, consulte [Procedimiento para configurar la autenticación](concept-security-agent-authentication-methods.md).

El script hace lo siguiente:

1. Instala los requisitos previos.

2. Agrega un usuario de servicio (con el inicio de sesión interactivo deshabilitado).

3. Instala el agente como **daemon** (se da por hecho que el dispositivo usa **systemd** para la administración del servicio).

4. Configura al agente con los parámetros de autenticación proporcionados. 

Para obtener ayuda adicional, ejecute el script con el parámetro – help: 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>Desinstalación del agente

Para desinstalar el agente, ejecute la secuencia de comandos –-desinstalar parámetro:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>solución de problemas
Compruebe el estado de implementación, para lo que debe ejecutar:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Pasos siguientes
- Lea la [información general](overview.md) del servicio ASC for IoT
- Obtenga más información acerca de la [arquitectura](architecture.md) de ASC for IoT
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Obtenga información acerca de las [alertas de seguridad](concept-security-alerts.md)