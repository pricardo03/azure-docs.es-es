---
title: Guía para instalar e implementar el agente basado en C de Linux de Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Aprenda a instalar el agente de Azure Security Center for IoT en máquinas Linux de 32 bits y 64 bits.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: mlottner
ms.openlocfilehash: 49ebb5932e1e918330625fd0df98811873dd5cd5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200655"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Implementar el agente de seguridad basado en C de Azure Security Center for IoT para Linux

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En esta guía se explica cómo instalar e implementar el agente de seguridad basado en C de Azure Security Center (ASC) for IoT en Linux.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalación del agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para obtener información sobre otras plataformas y tipos de agente, vea [Elegir el agente de seguridad correcto](how-to-deploy-agent.md).

1. Para implementar el agente de seguridad, se necesitan derechos de administrador local en el equipo de instalación (sudo).

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

Para desinstalar al agente, ejecute el script con el parámetro –-uninstall:

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