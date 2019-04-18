---
title: Introducción a TmaxSoft OpenFrame en Azure Virtual Machines
description: Rehospedar los IBM z/OS mainframe las cargas de trabajo mediante el entorno de TmaxSoft OpenFrame en Azure Virtual Machines (VM).
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 408e0166e52af9efd3d4c64f1b29bddcfc1cca4c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58896474"
---
# <a name="get-started-with-tmaxsoft-openframe-on-azure"></a>Introducción a TmaxSoft OpenFrame en Azure

Aprovechar los recursos de mainframe existentes y moverlas a Microsoft Azure con TmaxSoft OpenFrame. Esta solución popular de rehospedaje, crea un entorno de emulación en Azure, lo que le permite migrar rápidamente las aplicaciones. No se requiere ningún cambio de formato.

## <a name="openframe-rehosting-environment"></a>Entorno de rehospedaje OpenFrame

Configurar un entorno de OpenFrame en Azure para desarrollo, demostraciones, pruebas o las cargas de trabajo de producción. Como se muestra en la siguiente ilustración, OpenFrame incluye varios componentes que crean el entorno de emulación de gran sistema en Azure. Por ejemplo, servicios en línea OpenFrame reemplazar el middleware de mainframe, como IBM Customer Information Control System (CICS). OpenFrame Batch, con su componente TJES, reemplaza el subsistema de entrada de trabajo (JES de mainframes de IBM). 

![Proceso de rehospedaje OpenFrame](media/openframe-01.png)

> [!NOTE]
> Para ejecutar el entorno OpenFrame en Azure, debe tener una licencia de producto válida o una licencia de prueba de TmaxSoft.

## <a name="openframe-components"></a>Componentes OpenFrame

Los siguientes componentes forman parte del entorno OpenFrame en Azure:

- **Herramientas de migración** incluidos OFMiner, una solución que analiza los activos de mainframes y luego migrarlos a Azure.
- **Los compiladores**, incluidos OFCOBOL, un compilador que interpreta los programas de COBOL del gran sistema; OFPLI, que interpreta PL del gran sistema / programas; y OFASM, un compilador que interpreta los programas de ensamblador de mainframe.
- **Front-end** componentes, incluido el componente de puerta de enlace de OpenFrame que proporciona un agente de escucha 3270, un servidor de aplicaciones web que está certificado para Java Enterprise Edition 6.OFGW y solución de usuario de Java Enterprise (JEUS).
- **Aplicación** entorno. OpenFrame Base es el middleware que administra todo el sistema. C OpenFrame de tipo de servidor (OSC) reemplaza el mainframe middleware y IBM CICS.
- **Base de datos relacional**, por ejemplo, Tibero (se muestra), Microsoft SQL Server, base de datos de Oracle, IBM Db2 o MySQL. Las aplicaciones de OpenFrame utilizan el protocolo de Open Database Connectivity (ODBC) para comunicarse con la base de datos.
- **Seguridad** mediante TACF, un módulo de servicio que controla el acceso de usuario a los sistemas y los recursos. 
- **OFManager** es una solución que proporciona funciones de administración y operación de OpenFrame en el entorno web.

![Arquitectura de OpenFrame](media/openframe-02.png)

## <a name="next-steps"></a>Pasos siguientes

- [Instalar TmaxSoft OpenFrame en Azure](./install-openframe-azure.md)
