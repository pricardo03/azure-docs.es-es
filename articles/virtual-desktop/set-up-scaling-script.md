---
title: 'Escalado automático de hosts de sesión de la versión preliminar de Windows Virtual Desktop: Azure'
description: Describe cómo configurar el script de escalado automático para los hosts de sesión de la versión preliminar de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
ms.openlocfilehash: 7babfca617ab42da615518726d1b1d4cafe112b5
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70163235"
---
# <a name="automatically-scale-session-hosts"></a>Escalado automático de los host de sesiones

En muchas implementaciones de la versión preliminar de Windows Virtual Desktop, el costo de máquina virtual representa una parte considerable de los costos total de implementación de Windows Virtual Desktop. Para reducir dichos costos, es mejor apagar y desasignar máquinas de virtuales (VM) del host de sesión durante las horas de menos uso y, después, reinícielas durante las horas de máximo uso.

Este artículo usa un script de escalado simple para escalar automáticamente las máquinas virtuales del host de sesión en su entorno de Windows Virtual Desktop. Para más información acerca del funcionamiento del script de escalado, consulte la sección [Funcionamiento del script de escalado](#how-the-scaling-script-works) sección.

## <a name="prerequisites"></a>Requisitos previos

El entorno en el que se ejecuta el script debe tener los siguientes elementos:

- Un inquilino y una cuenta de Windows Virtual Desktop o una entidad de servicio con permisos para realizar consultas en dicho inquilino (como por ejemplo, Colaborador de RDS).
- Máquinas virtuales del grupo de hosts de sesión configuradas y registradas en el servicio Windows Virtual Desktop.
- Una máquina virtual adicional que ejecuta la tarea programada a través del Programador de tareas y tiene acceso de red a los hosts de sesión. Dicha máquina se denominará más adelante, en este mismo documento, como máquina virtual escaladora.
- El [módulo de PowerShell de Microsoft Azure Resource Manager](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) instalado en la máquina virtual en la que se ejecuta la tarea programada.
- El [módulo de PowerShell de Windows Virtual Desktop](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) instalado en la máquina virtual en la que se ejecuta la tarea programada.

## <a name="recommendations-and-limitations"></a>Limitaciones y recomendaciones

Al ejecutar el script de escalado, tenga en cuenta lo siguiente:

- Este script de escalado solo puede controlar un grupo de hosts por cada instancia de la tarea programada que se ejecuta en dicho script.
- Las tareas programadas que ejecutan scripts de escalado deben estar en una máquina virtual que esté siempre activa.
- Cree una carpeta independiente para cada instancia del script de escalado y su configuración.
- Dicho script no admite el inicio de sesión como administrador en Windows Virtual Desktop con cuentas de usuario de Azure AD que requieren la autenticación multifactor. Es aconsejable que use entidades de servicio para acceder al servicio Windows Virtual Desktop y a Azure. Siga [este tutorial](create-service-principal-role-powershell.md) para crear una entidad de servicio y una asignación de roles con PowerShell.
- La garantía que se establece en el contrato de nivel de servicio solo se aplica a las máquinas virtuales de un conjunto de disponibilidad. La versión actual del documento describe un entorno en el que una sola máquina virtual realiza el escalado, que puede cumplir los requisitos de disponibilidad, o no cumplirlos.

## <a name="deploy-the-scaling-script"></a>Implementación del script de escalado

Los procedimientos siguientes le indicará cómo implementar el script de escalado.

### <a name="prepare-your-environment-for-the-scaling-script"></a>Preparación del entorno para el script de escalado

En primer lugar, prepare el entorno para el script de escalado:

1. Inicie sesión en la máquina virtual (máquina virtual de escalado) en la que se ejecutará la tarea programada con una cuenta administrativa de dominio.
2. Cree una carpeta en la máquina virtual de escalado que contenga el script de escalado y su configuración (por ejemplo, **C:\\scaling-HostPool1**).
3. Descargue los archivos **basicScale.ps1**, **Config.xml**, y **Functions-PSStoredCredentials.ps1** archivos y el **PowershellModules** carpeta desde la [escalado repositorio de scripts](https://github.com/Azure/RDS-Templates/tree/master/wvd-sh/WVD%20scaling%20script) y cópielos en la carpeta que creó en el paso 2. Hay dos formas principales de obtener los archivos antes de copiarlos en la máquina virtual de escalado:
    - Clone el repositorio de Git en el equipo local.
    - Vea la versión **Sin formato** de cada archivo, copie y pegue el contenido de los archivos en un editor de texto y, después, guárdelos con el nombre y tipo de archivo correspondientes. 

### <a name="create-securely-stored-credentials"></a>Creación de credenciales almacenadas de forma segura

A continuación, deberá crear las credenciales almacenadas de forma segura:

1. Abra PowerShell ISE como administrador.
2. Importe el módulo de PowerShell de RDS, para lo que debe ejecutar el siguiente cmdlet:

    ```powershell
    Install-Module Microsoft.RdInfra.RdPowershell
    ```
    
3. Abra el panel de edición y cargue el archivo **Function-PSStoredCredentials.ps1**. A continuación, ejecute el script entero (F5).
4. Ejecute el siguiente cmdlet:
    
    ```powershell
    Set-Variable -Name KeyPath -Scope Global -Value <LocalScalingScriptFolder>
    ```
    
    Por ejemplo, **Set-Variable -Name KeyPath -Scope Global -Value "c:\\scaling-HostPool1"**
5. Ejecute el cmdlet **New-StoredCredential -KeyPath \$KeyPath**. Cuando se le solicite, escriba las credenciales de Windows Virtual Desktop con permisos para consultar el grupo de hosts (el grupo de hosts se especifica en **config.xml**).
    - Si utiliza diferentes entidades de servicio o una cuenta estándar, ejecute el cmdlet **New-StoredCredential -KeyPath \$KeyPath** una vez por cada cuenta crear credenciales almacenadas locales.
6. Ejecute **Get-StoredCredential -List** para confirmar que las credenciales se han creado correctamente.

### <a name="configure-the-configxml-file"></a>Configuración del archivo config.xml

Escriba los valores relevantes en los campos siguientes para actualizar la configuración del script de escalado en config.xml:

| Campo                     | DESCRIPCIÓN                    |
|-------------------------------|------------------------------------|
| AADTenantId                   | Identificador de inquilino de Azure AD que asocia la suscripción donde se ejecutan las máquinas virtuales del host de sesión     |
| AADApplicationId              | Id. de aplicación de la entidad de servicio                                                       |
| AADServicePrincipalSecret     | Se puede especificar durante la fase de pruebas, pero se debe mantener vacío una vez que se creen las credenciales con **Functions-PSStoredCredentials.ps1**    |
| currentAzureSubscriptionId    | El identificador de la suscripción de Azure en la que se ejecutan las máquinas virtuales de los hosts de sesión                        |
| tenantName                    | Nombre del inquilino de Windows Virtual Desktop                                                    |
| hostPoolName                  | Nombre de grupo de hosts de Windows Virtual Desktop                                                 |
| RDBroker                      | Dirección URL del servicio WVD, el valor predeterminado es https:\//rdbroker.wvd.microsoft.com             |
| Nombre de usuario                      | El identificador de la aplicación de la entidad de servicio (es posible que tenga la misma entidad de servicio de servicio que en AADApplicationId) o usuario estándar sin autenticación multifactor |
| isServicePrincipal            | Los valores aceptados son **true** o **false**. Indica si el segundo conjunto de credenciales que se usa es una entidad de servicio o una cuenta estándar. |
| BeginPeakTime                 | Cuando comienza el tiempo de uso máximo                                                            |
| EndPeakTime                   | Cuando termina el tiempo de uso máximo                                                              |
| TimeDifferenceInHours         | Diferencia horaria entre la hora local y la hora universal coordinada, en horas                                   |
| SessionThresholdPerCPU        | Número máximo de sesiones por el umbral de CPU que se usa para determinar cuándo se debe iniciar una máquina virtual del host de sesión en las horas pico.  |
| MinimumNumberOfRDSH           | Número mínimo de máquinas virtuales del grupo de hosts que se mantienen en ejecución durante el tiempo de menos uso             |
| LimitSecondsToForceLogOffUser | Número de segundos que se espera antes de obligar a los usuarios a cerrar la sesión. Si se establece en 0, no se obliga a los usuarios a cerrar sesión.  |
| LogOffMessageTitle            | Título del mensaje enviado a los usuarios antes de que se les obligue a cerrar sesión                  |
| LogOffMessageBody             | Cuerpo del mensaje de advertencia que se envía a los usuarios antes de que se cierre la sesión. Por ejemplo, "Este equipo se apagará en X minutos. Guarde el trabajo y cierre la sesión." |

### <a name="configure-the-task-scheduler"></a>Configuración del Programador de tareas

Después de configurar el archivo .xml de configuración, deberá configurar el Programador de tareas para ejecutar el archivo basicScaler.ps1 a intervalos regulares.

1. Inicie el **Programador de tareas**.
2. En la ventana del **Programador de tareas** ventana, seleccione **Crear tarea...**
3. En el cuadro de diálogo **Crear tarea**, seleccione la pestaña **General**, escriba un **nombre** (por ejemplo, "RDSH dinámica"), seleccione **Ejecutar tanto si el usuario inició sesión como si no**  y **Ejecutar con los privilegios más elevados** .
4. Vaya a la pestaña **Desencadenadores** y seleccione **Nuevo...**
5. En el cuadro de diálogo **Nuevo desencadenador**, en **Configuración avanzada**, active **Repetir cada** y seleccione el período y la duración adecuados (por ejemplo, **15 minutos** o **indefinidamente**).
6. Seleccione la pestaña **Acciones** y **Nuevo…**
7. En el cuadro de diálogo **Nueva acción**, escriba **powershell.exe** en el campo **Programa o script** y, después, escriba **C:\\scaling\\basicScale.ps1** en el campo **Agregar argumentos (opcional)** .
8. Vaya a las pestañas **Condiciones** y **Configuración**, y seleccione **Aceptar** para aceptar la configuración predeterminada en cada uno.
9. Escriba la contraseña de la cuenta administrativa en la que planea ejecutar el script de escalado.

## <a name="how-the-scaling-script-works"></a>Funcionamiento del script de escalado

Este script de escalado lee la configuración de un archivo config.xml, lo que incluye el inicio y el final del período de uso máximo durante el día.

Durante el tiempo de uso máximo, el script comprueba el número actual de sesiones y la capacidad del RDSH que se ejecuta en la actualidad para cada grupo de hosts. Calcula si las máquinas virtuales del host de sesión que se ejecuta tienen capacidad suficiente para dar soporte a las sesiones existentes basándose en el parámetro SessionThresholdPerCPU definido en el archivo config.xml. Si no, el script inicia más máquinas virtuales de los hosts de sesión en el grupo de hosts.

Durante el tiempo de uso mínimo, el script determina qué máquinas virtuales de los hosts de sesión deben apagarse en función del parámetro MinimumNumberOfRDSH del archivo config.xml. El script establecerá las máquinas virtuales de los hosts de sesión en modo de purga para evitar que las nuevas sesiones se conecten a los hosts. Si establece el parámetro **LimitSecondsToForceLogOffUser** del archivo config.xml en un valor positivo distinto de cero, el script le enviará una notificación cada vez que los usuarios que tengan la sesión iniciada guarden su trabajo, esperará el tiempo predeterminado y, después, forzará a los usuarios a cerrar la sesión. Una vez que se han cerrado todas las sesiones de usuario en una máquina virtual del host de sesión, el script apagará el servidor.

Si establece el parámetro **LimitSecondsToForceLogOffUser** del archivo config.xml en cero, el script permitirá que el valor de configuración de la sesión de las propiedades del grupo de hosts controle el cierre de las sesiones de los usuarios. Si hay sesiones en una máquina virtual del host de sesión, dejará que se ejecute dicha máquina. Si no hay sesiones, el script apagará la máquina virtual del host de sesión.

El script está diseñado para ejecutarse periódicamente en el servidor de la máquina virtual de escalado mediante el Programador de tareas. Seleccione el intervalo adecuado en función del tamaño del entorno de Servicios de Escritorio remoto y recuerde que iniciar y apagar las máquinas virtuales pueden tardar cierto tiempo. Se recomienda ejecutar el script de escalado cada 15 minutos.

## <a name="log-files"></a>Archivos de registro

El script de escalado crea dos archivos de registro, **WVDTenantScale.log** y **WVDTenantUsage.log**. El primero, **WVDTenantScale.log**, registrará los eventos y errores (si hay) que se produzcan en cada ejecución del script de escalado.

El archivo **WVDTenantUsage.log** registrará el número activo de núcleos y el número activo de máquinas virtuales cada vez que ejecute el script de escalado. Esta información se puede usar para calcular tanto el uso real de las máquinas virtuales de Microsoft Azure como su costo. El archivo tiene el formato de valores separados por comas y cada elemento contiene la información siguiente:

>hora, grupo de hosts, núcleos, máquinas virtuales

El nombre de archivo también puede modificarse para que tenga la extensión .csv y se puede cargar en Microsoft Excel para analizarlo.
