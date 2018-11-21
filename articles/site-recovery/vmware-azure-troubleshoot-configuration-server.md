---
title: Solución de problemas con el servidor de configuración durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas de la implementación del servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/11/2018
ms.author: ramamill
ms.openlocfilehash: b819783d127c51c0d5f33b2273a37a4180cb13a6
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569760"
---
# <a name="troubleshoot-configuration-server-issues"></a>Solución de problemas del servidor de configuración

En este artículo se le ayudará a solucionar problemas al implementar y administrar el servidor de configuración de [Azure Site Recovery](site-recovery-overview.md). El servidor de configuración se usa para configurar la recuperación ante desastres para VM de VMware y servidores físicos en Azure con Site Recovery. 

## <a name="installation-failures"></a>Errores de instalación

| **Mensaje de error** | **Acción recomendada** |
|--------------------------|------------------------|
|ERROR   no se pudieron cargar las cuentas. Error: System.IO.IOException: no se pueden leer los datos de la conexión de transporte al instalar y registrar el servidor CS.| Asegúrese de que TLS 1.0 está habilitado en el equipo. |

## <a name="registration-failures"></a>Errores de registro

Se pueden depurar los errores de registro con los registros de la carpeta %ProgramData%\ASRLogs.

Se pueden depurar los errores de registro revisando los registros de la carpeta **%ProgramData%\ASRLogs**.

| **Mensaje de error** | **Acción recomendada** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Mensaje: ACS50008: SAML token is invalid (el token SAML no es válido).<br>Id. de seguimiento: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Id. de correlación: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Marca de tiempo: **2016-12-12 14:50:08Z<br>** | Asegúrese de que la hora de su reloj del sistema no sea más de 15 minutos anterior o posterior a la hora local. Vuelva a ejecutar el instalador para completar el registro.|
|**09:35:27** : DRRegistrationException while trying to get all disaster recovery vault for the selected certificate (DRRegistrationException al intentar obtener todos los almacenes de recuperación ante desastres del certificado seleccionado): : Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException produjo, Exception.Message: ACS50008: SAML token is invalid (el token SAML no es válido).<br>Id. de seguimiento: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Id. de correlación: abe9deb8-3e64-464d-8375-36db9816427a<br>Marca de tiempo: **2016-05-19 01:35:39Z**<br> | Asegúrese de que la hora de su reloj del sistema no sea más de 15 minutos anterior o posterior a la hora local. Vuelva a ejecutar el instalador para completar el registro.|
|06:28:45:Error al crear certificado<br>06:28:45:Setup cannot proceed (el programa de instalación no puede proseguir). No se puede crear el certificado necesario para autenticarse en Site Recovery. Vuelva a ejecutar el programa de instalación | Asegúrese de que ejecuta el programa de instalación como un administrador local. |
