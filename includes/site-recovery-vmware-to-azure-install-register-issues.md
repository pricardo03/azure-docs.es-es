---
author: rockboyfor
ms.service: site-recovery
ms.topic: include
origin.date: 10/26/2018
ms.date: 12/10/2018
ms.author: v-yeche
ms.openlocfilehash: 9919521c8cb77f23f50a8097c4e630b4467dc725
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119738"
---
### <a name="installation-failures"></a>Errores de instalación
| **Ejemplo de mensaje de error** | **Acción recomendada** |
|--------------------------|------------------------|
|ERROR   no se pudieron cargar las cuentas. Error: System.IO.IOException: no se pueden leer los datos de la conexión de transporte al instalar y registrar el servidor CS.| Asegúrese de que TLS 1.0 está habilitado en el equipo. |

### <a name="registration-failures"></a>Errores de registro
Se pueden depurar los errores de registro revisando los registros de la carpeta **%ProgramData%\ASRLogs**.

| **Ejemplo de mensaje de error** | **Acción recomendada** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Mensaje: ACS50008: El token SAML no es válido.<br>Id. de seguimiento: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Id. de correlación: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Marca de tiempo: **2016-12-12 14:50:08Z<br>** | Asegúrese de que la hora de su reloj del sistema no sea más de 15 minutos anterior o posterior a la hora local. Vuelva a ejecutar el instalador para completar el registro.|
|**09:35:27**: DRRegistrationException al intentar obtener todos los almacenes de recuperación ante desastres para el certificado seleccionado: Threw Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message: ACS50008: El token SAML no es válido.<br>Id. de seguimiento: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Id. de correlación: abe9deb8-3e64-464d-8375-36db9816427a<br>Marca de tiempo: **2016-05-19 01:35:39Z**<br> | Asegúrese de que la hora de su reloj del sistema no sea más de 15 minutos anterior o posterior a la hora local. Vuelva a ejecutar el instalador para completar el registro.|
|06:28:45:Error al crear certificado<br>06:28:45:Setup cannot proceed (el programa de instalación no puede proseguir). No se puede crear el certificado necesario para autenticarse en Site Recovery. Vuelva a ejecutar el programa de instalación | Asegúrese de que ejecuta el programa de instalación como un administrador local. |