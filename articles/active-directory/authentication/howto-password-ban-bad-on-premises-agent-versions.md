---
title: Historial de versiones del agente de protección con contraseña de Azure AD local
description: Historial de cambios de comportamiento y versiones de los documentos
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: bcf5176728b520cae5d31750384f316efe244b7e
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663628"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Vista previa:  Historial de versiones del agente de protección con contraseña de Azure AD

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12650"></a>1.2.65.0

Fecha de lanzamiento: 01/02/2019

Cambios:

* Ahora se admiten el servicio de proxy y el agente de controlador de dominio en Server Core. Los requisitos mínimos del sistema operativo son los mismos que antes: Windows Server 2012 para agentes de controlador de dominio, y Windows Server 2012 R2 para los servidores proxy.
* Los cmdlets Register-AzureADPasswordProtectionProxy y Register-AzureADPasswordProtectionForest ahora admiten los modos de autenticación de Azure basada en código de dispositivo.
* El cmdlet Get-AzureADPasswordProtectionDCAgent omitirá los puntos de conexión de servicio alterados o no válidos. Esto corrige el error por el que los controladores de dominio a veces aparecen varias veces en la salida.
* El cmdlet Get-AzureADPasswordProtectionSummaryReport omitirá los puntos de conexión de servicio alterados o no válidos. Esto corrige el error por el que los controladores de dominio a veces aparecen varias veces en la salida.
* El módulo de PowerShell del servidor proxy ahora se registra desde %ProgramFiles%\WindowsPowerShell\Modules. Ya no se modifica la variable de entorno PSModulePath del equipo.
* Se agregó un nuevo cmdlet Get-AzureADPasswordProtectionProxy para ayudar a detectar servidores proxy registrados en un bosque o dominio.
* El agente de controlador de dominio usa una nueva carpeta en el recurso compartido sysvol para replicar las directivas de contraseñas y otros archivos.

   Ubicación de la carpeta antigua:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Ubicación de la carpeta nueva:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   (Este cambio se realizó para evitar advertencias de "GPO huérfano" falso positivas).

   > [!NOTE]
   > No se realizará ninguna migración ni intercambio de datos entre la antigua carpeta y la nueva carpeta. Las versiones anteriores del agente de controlador de dominio seguirán usando la ubicación anterior hasta que actualice a esta versión o una versión posterior. Una vez que todos los agentes de controlador de dominio ejecuten la versión 1.2.65.0 o posterior, se podrá eliminar manualmente la carpeta sysvol anterior.

* El agente de controlador de dominio y servicio de proxy ahora detectarán y eliminarán las copias alteradas de sus puntos de conexión de servicio correspondientes.
* Cada agente de controlador de dominio eliminará periódicamente los puntos de conexión de servicio alterados y obsoletos en su dominio, para puntos de conexión tanto del agente de controlador de dominio como del servicio de proxy. Los puntos de conexión del agente de controlador de dominio y del servicio de proxy se consideran obsoletos si su marca de tiempo de latido es anterior a siete días.
* El agente de controlador de dominio ahora renovará el certificado del bosque según sea necesario.
* El servicio de proxy ahora renovará el certificado del servidor proxy según sea necesario.
* Actualizaciones al algoritmo de validación de contraseña: la lista global de contraseñas prohibidas y la lista de contraseñas prohibidas específica del cliente (si está configurada) se combinan antes de las validaciones de contraseñas. Ahora se puede rechazar una contraseña determinada (error o solo auditoría) si contiene los tokens tanto de la lista global como de la específica del cliente. La documentación de registro de eventos se ha actualizado para reflejar esto; consulte [Monitor Azure AD Password Protection](howto-password-ban-bad-on-premises-monitor.md) (Supervisión de la protección de contraseñas de Azure AD).
* Correcciones de rendimiento y estabilidad
* Registro mejorado

> [!WARNING]
> Funcionalidad de tiempo limitado: el servicio del agente de controlador de dominio en esta versión (1.2.65.0) detendrá el procesamiento de solicitudes de validación de contraseñas a partir del 1 de septiembre de 2019.  Los servicios del agente de controlador de dominio en versiones anteriores (consulte la lista siguiente) detendrán el procesamiento a partir del 1 de julio de 2019. El servicio del agente de controlador de dominio en todas las versiones registrará eventos 10021 en el registro de eventos de administración en los dos meses anteriores a estas fechas límite. Se quitarán todas las restricciones de límite de tiempo en la próxima versión de disponibilidad general. El servicio del agente de proxy no está limitado por el tiempo en ninguna versión, pero aun así se debe actualizar a la versión más reciente para sacar provecho de todas las de errores posteriores y otras mejoras correcciones.

## <a name="12250"></a>1.2.25.0

Fecha de lanzamiento: 01/11/2018

Correcciones:

* El agente de controlador de dominio y el servicio de proxy ya no deberían funcionar mal debido a errores de confianza de certificados.
* El agente de controlador de dominio y el servicio de proxy presentan correcciones adicionales para las máquinas compatibles con FIPS.
* El servicio de proxy funcionará ahora correctamente en un entorno de red solo TLS 1.2.
* Correcciones menores de rendimiento y estabilidad
* Registro mejorado

Cambios:

* El nivel de sistema operativo mínimo necesario para el servicio de proxy es ahora Windows Server 2012 R2. El nivel de sistema operativo mínimo necesario para el servicio de agente de controlador de dominio sigue siendo Windows Server 2012.
* El servicio de proxy ahora requiere .NET versión 4.6.2.
* El algoritmo de validación de contraseña usa una tabla de normalización de caracteres ampliada. Como consecuencia, contraseñas que se aceptaron en versiones anteriores podrían rechazarse.

## <a name="12100"></a>1.2.10.0

Fecha de lanzamiento: 17/8/2018

Correcciones:

* Register-AzureADPasswordProtectionProxy y Register-AzureADPasswordProtectionForest ya admiten la autenticación multifactor.
* Register-AzureADPasswordProtectionProxy requiere un controlador de dominio WS2012 o posterior en el dominio para evitar los errores de cifrado.
* El servicio del agente del controlador de dominio es más confiable en cuanto a la solicitud de una directiva de contraseñas nueva de Azure en el inicio.
* El servicio del agente del controlador de dominio solicitará una directiva de contraseñas nueva de Azure cada hora si es necesario, pero ahora lo hará en una hora de inicio seleccionada aleatoriamente.
* El servicio del agente del controlador de dominio ya no producirá un retraso indefinido en el nuevo anuncio del controlador de dominio cuando se instale en un servidor antes de su promoción como una réplica.
* El servicio del agente del controlador de dominio ahora admite la opción de configuración "Habilitar protección con contraseña en Windows Server Active Directory".
* Ahora, los instaladores de proxy y agente del controlador de dominio son compatibles con la actualización local al realizar la actualización a versiones futuras.

> [!WARNING]
> La actualización local de la versión 1.1.10.3 no se admite y producirá un error de instalación. Para actualizar a la versión 1.2.10 o posterior, primero deberá desinstalar por completo el software del servicio de proxy y el agente de controlador de dominio y, luego, deberá instalar la nueva versión desde cero. Es necesario eliminar el registro del servicio de proxy de protección con contraseña de Azure AD.  No es necesario volver a registrar el bosque.

> [!NOTE]
> Las actualizaciones locales del software del agente del controlador de dominio requerirán un reinicio.

* El servicio de proxy y de agente del controlador de dominio ahora se pueden ejecutar en un servidor configurado para usar solo algoritmos conformes a FIPS.
* Correcciones menores de rendimiento y estabilidad
* Registro mejorado

## <a name="11103"></a>1.1.10.3

Fecha de lanzamiento: 15/6/2018

Versión preliminar pública inicial

## <a name="next-steps"></a>Pasos siguientes

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
