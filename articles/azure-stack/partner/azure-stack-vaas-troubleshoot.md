---
title: Solución de problemas de validación de Azure Stack como servicio | Microsoft Docs
description: Solucione problemas de validación como servicio para Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 4372da2f1057c06761dd6abaf18c26df6e33640e
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234572"
---
# <a name="troubleshoot-validation-as-a-service"></a>Solución de problemas de la validación como un servicio

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Los siguientes son problemas comunes no relacionados con las versiones de software, y sus soluciones.

## <a name="the-portal-shows-local-agent-in-debug-mode"></a>El portal muestra el agente local en modo de depuración

Esto probablemente se deba a que el agente no puede enviar latidos al servicio debido a una conexión de red inestable. Se envía un latido cada cinco minutos. Si el servicio no recibe un latido durante 15 minutos, considera que el agente está inactivo y ya no se programarán pruebas en él. Compruebe el mensaje de error en el archivo *Agenthost.log* ubicado en el directorio donde se inició el agente.

> [!Note] 
> Las pruebas que ya estén en ejecución en el agente seguirán ejecutándose, pero si el latido no se restaura antes de que finalice la prueba, el agente no podrá actualizar el estado de prueba ni cargar registros. La prueba siempre se mostrará como **en ejecución** y debe cancelarse.

## <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>El proceso del agente en el equipo se cerró mientras se ejecutaba la prueba. Qué cabe esperar

Si el proceso del agente se cierre de manera brusca, por ejemplo, se reinicia el equipo, se finaliza el proceso (CTRL+C en la ventana del agente se considera un apagado estable), la prueba que se ejecutaba en él se seguirá mostrando como **en ejecución**. Si se reinicia el agente, este actualizará el estado de la prueba a **cancelado**. Si no se reinicia el agente, la prueba aparece como **en ejecución** y debe cancelarla manualmente.

> [!Note] 
> Las pruebas dentro de un flujo de trabajo están programadas para ejecutarse de forma secuencial. Las pruebas **pendientes** no se ejecutarán hasta que no se completen las pruebas en estado **en ejecución** en el mismo flujo de trabajo.

## <a name="handle-slow-network-connectivity"></a>Control de la conectividad de red lenta

Puede descargar la imagen del PIR en un recurso compartido en el centro de datos local. Luego puede comprobar la imagen.

<!-- This is from the appendix to the Deploy local agent topic. -->

### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>Descarga de la imagen PIR en el recurso compartido local en el caso de tráfico de red lento

1. Descargue AzCopy desde: [vaasexternaldependencies(AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip).

2. Extraiga AzCopy.zip y cambie al directorio que contiene AzCopy.exe.

3. Abra Windows PowerShell con un símbolo del sistema con privilegios elevados. Ejecute los comandos siguientes:

```PowerShell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare es la ruta de acceso del recurso compartido o la ruta de acceso local.

### <a name="verifying-pir-image-file-hash-value"></a>Comprobación del valor hash del archivo de imagen PIR

Puede usar el cmdlet **Get-HashFile** para obtener el valor hash para los archivos de imagen del repositorio de imágenes públicas descargadas para comprobar la integridad de las imágenes.

| Nombre de archivo | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL.vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL.vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS.vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604-20170619.1.vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información, consulte [Validación como un servicio para Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
