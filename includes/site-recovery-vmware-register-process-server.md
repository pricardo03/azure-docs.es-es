---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: e18d0a6a01a86f844edc213fc95003cf4f4b46c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166193"
---
* Conéctese a la máquina virtual del servidor de procesos mediante la Conexión a Escritorio remoto.
* Para iniciar cspsconfigtool.exe, haga clic en el acceso directo disponible en el escritorio. (La herramienta se iniciará automáticamente si es la primera vez que inicia sesión en el servidor de procesos).
  - Nombre completo (FQDN) o dirección IP del servidor de configuración
  - Puerto en el que el servidor de configuración realiza la escucha. El valor debería ser 443
  - Frase de contraseña de la conexión para conectarse al servidor de configuración.
  - Puerto de transferencia de datos que va a configurar para este servidor de procesos. Deje el valor predeterminado tal cual, a menos que lo haya cambiado a un número de puerto diferente en su entorno.

    ![Registrar servidor de procesos](./media/site-recovery-vmware-register-process-server/register-ps.png)
* Haga clic en el botón Guardar para guardar la configuración y registrar el servidor de procesos.
