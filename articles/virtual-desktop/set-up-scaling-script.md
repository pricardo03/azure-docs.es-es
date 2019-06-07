---
title: 'Escalado automático de hosts de sesión de Windows Vista previa del escritorio Virtual: Azure'
description: Describe cómo configurar el script de escalado automático para los hosts de sesión de Windows Vista previa del escritorio Virtual.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: e9f500e3ab965b9dbfc5e395a6572497c85f6f8f
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755135"
---
# <a name="automatically-scale-session-hosts"></a>Escalado automático de los host de sesiones

Para muchas implementaciones de vista previa de Escritorio Virtual Windows en Azure, el costo de máquina virtual representa una parte significativa del costo total de implementación de Escritorio Virtual de Windows. Para reducir los costos, es mejor apagar y desasignar máquinas de virtuales del host de sesión (VM) durante las horas de poca actividad de uso y, a continuación, reinícielos durante las horas de uso.

Este artículo usa un script simple de escalado para escalar automáticamente las máquinas virtuales de host de sesión en su entorno de Escritorio Virtual de Windows. Para obtener más información acerca de cómo funciona el script de escalado, consulte el [cómo funciona la secuencia de comandos escalado](#how-the-scaling-script-works) sección.

## <a name="prerequisites"></a>Requisitos previos

El entorno donde se ejecuta el script debe tener lo siguiente:

- Un inquilino de Escritorio Virtual de Windows y la cuenta o una entidad de servicio con permisos para consultar ese inquilino (por ejemplo, el colaborador de RDS).
- Máquinas virtuales del grupo de host de sesión configurado y registren con el servicio de Escritorio Virtual de Windows.
- Una máquina virtual adicional que se ejecuta la tarea programada a través del programador de tareas y tiene acceso a la red para hosts de sesión. Esto se hará referencia a más adelante en el documento como máquina virtual de scaler.
- El [módulo Microsoft Azure Resource Manager PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) instalado en la máquina virtual ejecutando la tarea programada.
- El [módulo Windows PowerShell de Escritorio Virtual](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) instalado en la máquina virtual ejecutando la tarea programada.

## <a name="recommendations-and-limitations"></a>Limitaciones y recomendaciones

Al ejecutar el script de escalado, tenga en cuenta lo siguiente:

- Este script de escalado solo puede controlar un grupo host por cada instancia de la tarea programada que se está ejecutando el script de escalado.
- Las tareas programadas que se ejecutan scripts de escala deben estar en una máquina virtual que siempre está activada.
- Cree una carpeta independiente para cada instancia de la secuencia de comandos de escalado y su configuración.
- Este script no admite iniciar sesión como administrador al escritorio Virtual de Windows con cuentas de usuario de Azure AD que requieren la autenticación multifactor. Se recomienda que usar a entidades de servicio para acceder al servicio de Escritorio Virtual de Windows y Azure. Siga [este tutorial](create-service-principal-role-powershell.md) para crear una entidad de servicio y una asignación de roles con PowerShell.
- Garantía de SLA de Azure solo se aplica a las máquinas virtuales en un conjunto de disponibilidad. La versión actual del documento describe un entorno con una sola máquina virtual realiza el escalado, que puede no cumplir los requisitos de disponibilidad.

## <a name="deploy-the-scaling-script"></a>Implementar el script de escalado

Los procedimientos siguientes le indicará cómo implementar la secuencia de comandos de escalado.

### <a name="prepare-your-environment-for-the-scaling-script"></a>Preparar el entorno para el script de escalado

En primer lugar, prepare el entorno para el script de escalado:

1. Inicie sesión en la máquina virtual (VM de scaler) que se ejecutará la tarea programada con una cuenta administrativa de dominio.
2. Cree una carpeta en la máquina virtual para almacenar el script de escalado y su configuración de scaler (por ejemplo, **C:\\HostPool1 escalado**).
3. Descargue el **basicScale.ps1**, **Config.xml**, y **funciones PSStoredCredentials.ps1** archivos y el **PowershellModules** carpeta desde la [escalado repositorio de scripts](https://github.com/Azure/RDS-Templates/tree/master/wvd-sh/WVD%20scaling%20script) y copiarlos en la carpeta que creó en el paso 2. Hay dos métodos principales para obtener los archivos antes de copiarlos en la máquina virtual de scaler:
    - Clone el repositorio de git en el equipo local.
    - Ver el **Raw** versión de cada archivo, copie y pegue el contenido de cada archivo en un editor de texto, y después guarde los archivos con el nombre de archivo correspondiente y el tipo de archivo. 

### <a name="create-securely-stored-credentials"></a>Crear las credenciales almacenadas de forma segura

A continuación, deberá crear las credenciales almacenadas de forma segura:

1. Abra PowerShell ISE como administrador.
2. Importe el módulo de PowerShell de RDS, ejecute el siguiente cmdlet:

    ```powershell
    Install-Module Microsoft.RdInfra.RdPowershell
    ```
    
3. Abra el panel de edición y carga el **función PSStoredCredentials.ps1** archivo.
4. Ejecute el siguiente cmdlet:
    
    ```powershell
    Set-Variable -Name KeyPath -Scope Global -Value <LocalScalingScriptFolder>
    ```
    
    Por ejemplo, **Set-Variable - nombre KeyPath-ámbito Global-valor "c:\\HostPool1 escalado"**
5. Ejecute el **StoredCredential de New - KeyPath \$KeyPath** cmdlet. Cuando se le solicite, escriba sus credenciales de Escritorio Virtual de Windows con permisos para consultar el grupo de host (el grupo host se especifica en el **config.xml**).
    - Si utiliza diferentes entidades de servicio o cuenta estándar, ejecute el **StoredCredential de New - KeyPath \$KeyPath** cmdlet una vez para que cada cuenta crear las credenciales almacenadas.
6. Ejecute **StoredCredentials de Get-lista** para confirmar las credenciales se han creado correctamente.

### <a name="configure-the-configxml-file"></a>Configurar el archivo config.xml

Escriba los valores correspondientes en los campos siguientes para actualizar los valores de secuencia de comandos de escalado en el archivo config.xml:

| Campo                     | DESCRIPCIÓN                    |
|-------------------------------|------------------------------------|
| AADTenantId                   | Identificador de inquilino de AD de Azure que asocia la suscripción donde se ejecuta el host de sesión de las máquinas virtuales     |
| AADApplicationId              | Id. de aplicación de la entidad de servicio                                                       |
| AADServicePrincipalSecret     | Esto se puede especificar durante la fase de pruebas, pero es mantenerse vacío después de crear las credenciales con **PSStoredCredentials.ps1 de funciones**    |
| currentAzureSubscriptionId    | El identificador de la suscripción de Azure donde se ejecuta el host de sesión de las máquinas virtuales                        |
| tenantName                    | Nombre del inquilino de Escritorio Virtual de Windows                                                    |
| hostPoolName                  | Nombre del grupo host de Escritorio Virtual de Windows                                                 |
| RDBroker                      | Dirección URL del servicio WVD predeterminado valor https:\//rdbroker.wvd.microsoft.com             |
| Nombre de usuario                      | El Id. de aplicación de la entidad de servicio (es posible tener la misma entidad de servicio como en AADApplicationId) o un usuario estándar sin Multi-factor authentication |
| isServicePrincipal            | Valores aceptados son **true** o **false**. Indica si el segundo conjunto de credenciales que utiliza una entidad de servicio o una cuenta estándar. |
| BeginPeakTime                 | Cuando comienza el tiempo de uso máximo                                                            |
| EndPeakTime                   | Cuando finaliza el tiempo de uso máximo                                                              |
| TimeDifferenceInHours         | Diferencia horaria entre la hora local y UTC, en horas                                   |
| SessionThresholdPerCPU        | Número máximo de sesiones por el umbral de CPU que se usa para determinar cuándo debe iniciar durante las horas de una máquina virtual del host de sesión nuevo.  |
| MinimumNumberOfRDSH           | Número mínimo de máquinas virtuales siga ejecutándose durante el tiempo de poca actividad de uso de grupos de host             |
| LimitSecondsToForceLogOffUser | Número de segundos que deben transcurrir antes de obligar a los usuarios para cerrar la sesión. Si se establece en 0, los usuarios no está obligado a cerrar la sesión.  |
| LogOffMessageTitle            | Título del mensaje enviado a un usuario antes de que obliga a cerrar sesión                  |
| LogOffMessageBody             | Cuerpo del mensaje de advertencia que se envía a los usuarios antes de que está cerrados. Por ejemplo, "este equipo se apagará avanzar en la X minutos. Guarde su trabajo y cerrar la sesión." |

### <a name="configure-the-task-scheduler"></a>Configurar al programador de tareas

Después de configurar el archivo .xml de configuración, deberá configurar el programador de tareas para ejecutar el archivo basicScaler.ps1 a intervalos regulares.

1. Iniciar **del programador de tareas**.
2. En el **programador de tareas** ventana, seleccione **crear tarea...**
3. En **crear tarea** cuadro de diálogo, seleccione el **General** , escriba un **nombre** (por ejemplo, "RDSH dinámica"), seleccione **ejecutar si el usuario ha iniciado sesión o no** y **ejecutar con privilegios más altos**.
4. Vaya a la **desencadenadores** y, después, seleccione **nuevo...**
5. En el **nuevo desencadenador** cuadro de diálogo, en **configuración avanzada**, comprobar **repetir la tarea cada** y seleccione el período adecuado y la duración (por ejemplo, **15 minutos** o **indefinidamente**).
6. Seleccione el **acciones** pestaña y **nuevo...**
7. En el **nueva acción** cuadro de diálogo, escriba **powershell.exe** en el **programa o script** campo y, después, escriba **C:\\escalado\\ RDSScaler.ps1** en el **agregar argumentos (opcional)** campo.
8. Vaya a la **condiciones** y **configuración** pestañas y seleccione **Aceptar** para aceptar la configuración predeterminada para cada uno.
9. Escriba la contraseña para la cuenta administrativa que va a ejecutar el script de escalado.

## <a name="how-the-scaling-script-works"></a>Cómo funciona el script de escalado

Este script de escalado lee la configuración de un archivo config.xml, incluido el inicio y final del período de uso máximo durante el día.

Durante el tiempo de uso máximo, el script comprueba el número actual de sesiones y la capacidad RDSH de ejecución actual para cada grupo host. Calcula si el host de sesión está ejecutando las máquinas virtuales tiene capacidad suficiente para admitir las sesiones existentes basándose en el parámetro SessionThresholdPerCPU definido en el archivo config.xml. De lo contrario, el script inicia las máquinas virtuales del host de sesión adicional en el grupo host.

Durante el tiempo de uso de poca actividad, la secuencia de comandos determina qué host de sesión de las máquinas virtuales deben apagar basándose en el parámetro MinimumNumberOfRDSH en el archivo config.xml. El script establecerá la sesión agote modo para evitar que las nuevas sesiones conectarse a los hosts de máquinas virtuales de host. Si establece la **LimitSecondsToForceLogOffUser** parámetro en el archivo config.xml para un valor positivo distinto de cero, el script le notificará cualquier actualmente ha iniciado sesión a los usuarios guardar trabajo, espere el tiempo predeterminado y, a continuación, forzar el usuarios a cerrar la sesión. Una vez que todas las sesiones de usuario se han firmado en una máquina virtual del host de sesión, se apagará el servidor en la secuencia de comandos.

Si establece la **LimitSecondsToForceLogOffUser** parámetro en el archivo config.xml en cero, la secuencia de comandos permitirá que la opción de configuración de sesión en el host de propiedades de grupo controlar las sesiones de usuario la firma. Si no hay ninguna sesión en una máquina virtual del host de sesión, dejará la máquina virtual del host de sesión ejecutando. Si no existe ninguna sesión, se apagará la máquina virtual del host de sesión en la secuencia de comandos.

El script está diseñado para ejecutarse periódicamente en el servidor de la máquina virtual de scaler mediante el programador de tareas. Seleccione el intervalo de tiempo adecuado en función del tamaño de su entorno de servicios de escritorio remoto y recuerde que iniciar y apagar las máquinas virtuales pueden tardar algún tiempo. Se recomienda ejecutar el script de escalado cada 15 minutos.

## <a name="log-files"></a>Archivos de registro

El script de escalado crea dos archivos de registro, **WVDTenantScale.log** y **WVDTenantUsage.log**. El **WVDTenantScale.log** archivo registrará los eventos y errores (si existe) durante cada ejecución del script de escalado.

El **WVDTenantUsage.log** archivo registrará el número de núcleos de activos y el número de máquinas virtuales activos cada vez que ejecute el script de escalado. Puede usar esta información para calcular el uso real de las máquinas virtuales de Microsoft Azure y el costo. El archivo de formato es como valores separados por comas, con cada elemento que contiene la información siguiente:

>tiempo, el grupo host, núcleos, las máquinas virtuales

El nombre de archivo también puede modificarse para que tenga una extensión .csv, cargado en Microsoft Excel y analizan.
