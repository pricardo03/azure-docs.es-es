---
title: Detección de soluciones de Endpoint Protection y evaluación del mantenimiento en Azure Security Center | Microsoft Docs
description: Cómo se detectan las soluciones de Endpoint Protection y cómo se determina que tienen un estado correcto.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: v-mohabe
ms.openlocfilehash: b17e5f16b988bfa562b00bc6f5b9dfd34be4ca43
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66247970"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Valoración y recomendaciones de Endpoint Protection en Azure Security Center

Valoración y recomendaciones de Endpoint Protection en Azure Security Center es un servicio que detecta y proporciona una valoración del estado de las versiones [compatibles](https://docs.microsoft.com/azure/security-center/security-center-os-coverage#supported-platforms-for-windows-computers-and-vms) de las soluciones de Endpoint Protection. En este tema se explican los escenarios que generan las siguientes dos recomendaciones para las soluciones de protección Endpoint Protection por parte de Azure Security Center.

* **Instalar soluciones de Endpoint Protection en la máquina virtual**
* **Resolver problemas de mantenimiento de Endpoint Protection en las máquinas**

## <a name="windows-defender"></a>Windows Defender

* La recomendación **"Instalar soluciones de Endpoint Protection en la máquina virtual"** se genera cuando se ejecuta [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) y el resultado es **AMServiceEnabled: False**

* La recomendación **"Resolver problemas de mantenimiento de Endpoint Protection en las máquinas"** se genera cuando se ejecuta [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) y ocurre una de las opciones siguientes:

  * Al menos una de las propiedades siguientes es false:

     **AMServiceEnabled**

     **AntispywareEnabled**

     **RealTimeProtectionEnabled**

     **BehaviorMonitorEnabled**

     **IoavProtectionEnabled**

     **OnAccessProtectionEnabled**

  * Si una de las siguientes propiedades, o las dos, es mayor o igual que 7.

     **AntispywareSignatureAge**

     **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center Endpoint Protection

* La recomendación **"Instalar soluciones de Endpoint Protection en la máquina virtual"** se genera cuando se importa **SCEPMpModule  ("$env:ProgramFiles\Microsoft Security Client\MpProvider\MpProvider.psd1" )** y la ejecución de **Get-MProtComputerStatus** da como resultado **AMServiceEnabled = false**.

* La recomendación **"Resolver problemas de mantenimiento de Endpoint Protection en las máquinas"** se genera cuando se ejecuta **Get-MprotComputerStatus** y ocurre una de las opciones siguientes, o ambas:

    * Al menos una de las propiedades siguientes es false:

       **AMServiceEnabled**
    
       **AntispywareEnabled**
    
       **RealTimeProtectionEnabled**
    
       **BehaviorMonitorEnabled**
    
       **IoavProtectionEnabled**
    
       **OnAccessProtectionEnabled**
          
    * Si una de las siguientes actualizaciones de firma, o las dos, es mayor o igual que 7. 

       **AntispywareSignatureAge**
    
       **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* La recomendación **"Instalar soluciones de Endpoint Protection en la máquina virtual"** se genera si no se cumple una de las siguientes comprobaciones, o varias de ellas:
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent** existe.
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent\InstallationFolder** existe.
    * El archivo **dsq_query.cmd** se encuentra en la carpeta de instalación.
    * La ejecución de **dsa_query.cmd** da como resultado **Component.AM.mode: on - Trend Micro Deep Security Agent detected**.

## <a name="symantec-endpoint-protection"></a>Symantec Endpoint Protection
La recomendación **"Instalar soluciones de Endpoint Protection en la máquina virtual"** se genera si no se cumple alguna de las siguientes comprobaciones:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

o

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

La recomendación **"Resolver problemas de estado de protección de puntos de conexión en las máquinas"** se genera si no se cumple alguna de las siguientes comprobaciones:  

* Comprobar que la versión de Symantec es >= 12:  Ubicación del Registro: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion" -Value "PRODUCTVERSION"**

* Comprobar el estado de protección en tiempo real: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* Comprobar el estado de actualización de firma: **HKLM\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LatestVirusDefsDate <= 7 days**

* Comprobar el estado de examen completo: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime <= 7 days**

* Buscar la ruta del número de versión de firma para la versión de firma para Symantec 12: **Rutas de acceso del Registro + "CurrentVersion\SharedDefs" -Value "SRTSP"** 

* Ruta de acceso a la versión de firma para Symantec 14: **Rutas de acceso del Registro + "CurrentVersion\SharedDefs\SDSDefs" -Value "SRTSP"**

Rutas de acceso del Registro:

**"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;** 
 **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>McAfee Endpoint Protection para Windows

La recomendación **"Instalar soluciones de Endpoint Protection en la máquina virtual"** se genera si no se cumplen las siguientes comprobaciones:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** existe.

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

La recomendación **"Resolver problemas de estado de protección de puntos de conexión en las máquinas"** se genera si no se cumplen las siguientes comprobaciones:

* Versión de McAfee: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* Buscar la versión de la firma: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "dwContentMajorVersion"**

* Buscar la fecha de la firma: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "szContentCreationDate" >= 7 days**

* Buscar la fecha del examen: **HKLM:\Software\McAfee\Endpoint\AV\ODS -Value "LastFullScanOdsRunTime" >= 7 days**

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Los registros de extensión de Microsoft Antimalware están disponibles en:  
**%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(Or PaaSAntimalware)\1.5.5.x(version#)\CommandExecution.log**

### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). O bien, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).
