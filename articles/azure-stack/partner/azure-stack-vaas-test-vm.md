---
title: Implementación del agente local y prueba de las máquinas virtuales de imagen en la validación como servicio de Azure Stack | Microsoft Docs
description: Implemente el agente local y pruebe las máquinas virtuales de imagen en la validación como servicio de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 78136ab00dcba2f8a99df36ba99d384b49995882
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159221"
---
# <a name="deploy-the-local-agent-and-test-virtual-machines"></a>Implementación del agente local y prueba de las máquinas virtuales

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Obtenga información sobre cómo usar el agente local de validación como servicio (VaaS) para comprobar el hardware. El agente local debe implementarse en la solución de Azure Stack que se va a validar antes de ejecutar las pruebas de validación.

> [!Note]  
> Debe asegurarse de que la máquina en la que se ejecuta el agente local no pierda el acceso a Internet. La máquina solo debe ser accesible para los usuarios que están autorizados a usar la validación como servicio de Azure Stack.

Pasos para probar las máquinas virtuales:

1. Instalación del agente local
2. Inyección de errores en el sistema
3. Ejecución del agente local

## <a name="download-and-start-the-local-agent"></a>Descarga e inicio del agente local

Descargue el agente en un equipo del centro de datos que cumpla los requisitos previos, que no forme parte del sistema de Azure Stack, pero que tenga acceso a todos los puntos de conexión de Azure Stack.

### <a name="machine-prerequisites"></a>Requisitos previos del equipo

Compruebe que el equipo cumple los criterios siguientes:

- Acceso a todos los puntos de conexión de Azure Stack
- .NET 4.6 y PowerShell 5.0 instalados
- Al menos 8 GB de RAM
- Procesadores de 8 núcleos como mínimo
- Espacio en disco mínimo de 200 GB
- Conectividad de red estable a Internet

Azure Stack es el sistema objeto de la prueba. La máquina no debe formar parte de Azure Stack ni estar hospedada en la nube de Azure Stack.

### <a name="download-and-install-the-agent"></a>Descarga e instalación del agente

1. Abra Windows PowerShell en un símbolo del sistema con privilegios elevados en el equipo que se usará para ejecutar las pruebas.
2. Ejecute el comando siguiente para descargar el agente local:

    ```PowerShell  
        Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.3.2.0.nupkg" -outfile "OnPremAgent.zip"
        Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent.3.2.0 -Force
        Set-Location VaaSOnPremAgent.3.2.0\lib\net46
    ````

3. Ejecute el siguiente comando para instalar las dependencias del agente local:

    ```PowerShell  
        $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-VaaSPrerequisites -AadTenantId <AadTenantId> `
        -ServiceAdminCreds <ServiceAdminCreds> `
        -ArmEndpoint https://adminmanagement.<ExternalFqdn> `
        -Region <Region>
    ````

    **Parámetros**

    | Parámetro | DESCRIPCIÓN |
    | --- | --- |
    | aadServiceAdminUser | Usuario administrador global del inquilino de Azure AD. Por ejemplo, puede ser vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | Contraseña del usuario administrador global. |
    | AadTenantId | Identificador del inquilino de Azure AD para la cuenta de Azure registrada en la validación como servicio. |
    | ExternalFqdn | Puede obtener el nombre de dominio completo en el archivo de configuración. Para obtener instrucciones, consulte [Prueba de los parámetros de validación como servicio de Azure Stack](azure-stack-vaas-parameters-test.md). |
    | Region | Región del inquilino de Azure AD. |

El comando descarga una imagen (VHD del sistema operativo) del repositorio de imágenes públicas (PIR) y la copia desde Azure Blob Storage al almacenamiento de Azure Stack. 

![Requisitos previos para la descarga](media/installingprereqs.png)

> [!Note]  
> Si tiene baja velocidad de red al descargar estas imágenes, puede descargarlas por separado en un recurso compartido local y especificar el parámetro **-LocalPackagePath** *recurso_compartido_o_ruta_de_acceso_local*. Encontrará más información sobre la descarga de PIR en la sección [Control de situaciones de conectividad de red lenta](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) de artículo [Solución de problemas de validación como servicio](azure-stack-vaas-troubleshoot.md).

## <a name="fault-injection"></a>Inyección de errores

Microsoft ha diseñado Azure Stack para ser resistente y para tolerar distintos tipos de errores de hardware y software. La inyección de errores aumenta la tasa de errores en el sistema. Este aumento le ayuda a detectar problemas antes, de modo que pueda reducir el número de incidentes que dejen el sistema fuera de servicio.

Ejecute los comandos siguientes para inyectar errores en el sistema.

1. Abra Windows PowerShell en un símbolo del sistema con privilegios elevados.

2. Ejecute el siguiente comando:

    ```PowerShell  
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-ServiceFabricSDK Install-ServiceFabricSDK
    ```

## <a name="run-the-agent"></a>Ejecución del agente

1. Abra Windows PowerShell en un símbolo del sistema con privilegios elevados.

2. Ejecute el siguiente comando:

    ````PowerShell  
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Parámetros**  
    
    | Parámetro | DESCRIPCIÓN |
    | --- | --- |
    | VaaSUserId | Identificador de usuario utilizado para iniciar sesión en el portal de VaaS (por ejemplo, UserName@Contoso.com) |
    | VaaSTenantId | Identificador del inquilino de Azure AD para la cuenta de Azure registrada en la validación como servicio. |

    > [!Note]  
    > Cuando se ejecuta el agente, el directorio de trabajo actual debe ser la ubicación del archivo ejecutable del host del motor de tareas **Microsoft.VaaSOnPrem.TaskEngineHost.exe**.

Si no ve notificado ningún error, la ejecución del agente local se realizó correctamente. El texto de ejemplo siguiente aparece en la ventana de la consola.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Agente iniciado](media/startedagent.png)

Un agente se identifica de forma única por su nombre. De forma predeterminada, usa el nombre de dominio completo (FQDN) de la máquina donde se inició. Se debe minimizar la ventana para evitar cualquier clic accidental en la misma ya que, al cambiar el foco, se detienen todas las demás acciones.

## <a name="next-steps"></a>Pasos siguientes

- [Validación de una nueva solución de Azure Stack](azure-stack-vaas-validate-solution-new.md)  
- Si tiene conectividad a Internet lenta o intermitente, puede descargar la imagen del PIR. Para más información, consulte [Control de situaciones de conectividad de red lenta](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity).
- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
