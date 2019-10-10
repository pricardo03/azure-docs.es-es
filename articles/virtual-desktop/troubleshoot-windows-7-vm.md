---
title: 'Solución de problemas de las máquinas virtuales con Windows 7 en Windows Virtual Desktop: Azure'
description: Cómo resolver incidencias en máquinas virtuales con Windows 7 en un entorno de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: helohr
ms.openlocfilehash: 7fdb46ac7cffc4eb6a791304c7a6b58378806296
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679400"
---
# <a name="troubleshoot-windows-7-virtual-machines-in-windows-virtual-desktop"></a>Solución de problemas de máquinas virtuales Windows 7 en Windows Virtual Desktop

Use este artículo para solucionar los problemas que tiene al configurar las máquinas de virtuales (VM) de host de sesión de Windows Virtual Desktop.

## <a name="known-issues"></a>Problemas conocidos

Windows 7 en instancias de Windows Virtual Desktop no admite las siguientes características:

- Aplicaciones virtualizadas (RemoteApps)
- Redirección de zona horaria
- Escalado automático de PPP

Windows Virtual Desktop solo puede virtualizar escritorios completos para Windows 7.

Aunque no se admite el escalado automático de PPP, puede cambiar manualmente la resolución de la máquina virtual; para ello, haga clic con el botón derecho en el icono del cliente de Escritorio remoto y seleccione **Resolución**.

## <a name="error-cant-access-the-remote-desktop-user-group"></a>Error: No se puede acceder al grupo Usuario de Escritorio remoto

Si Windows Virtual Desktop no puede encontrarle o no puede encontrar las credenciales de los usuarios en el grupo Usuario de Escritorio remoto, puede que vea uno de los siguientes mensajes de error:

- "This user is not a member of the Remote Desktop User group" (Este usuario no es miembro del grupo Usuario de Escritorio remoto)
- "You must be granted permissions to sign in through Remote Desktop Services" (Se le deben conceder permisos para iniciar sesión a través de Servicios de Escritorio remoto)

Para corregir este error, agregue el usuario al grupo Usuario de Escritorio remoto:

1. Abra Azure Portal.
2. Seleccione la máquina virtual en la que vio el mensaje de error.
3. Seleccione **Ejecutar un comando**.
4. Ejecute el siguiente comando, pero reemplace `<username>` por el nombre del usuario que desea agregar:
   
   ```cmd
   localgroup "Remote Desktop Users" <username> /add
   ```