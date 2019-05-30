---
title: EndPoint protection soluciones detección y evaluación de mantenimiento en Azure Security Center | Microsoft Docs
description: Cómo las soluciones de protección de punto de conexión se detectan y se identifican con un estado correcto.
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
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247970"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Evaluación de EndPoint protection y las recomendaciones de Azure Security Center

Evaluación de EndPoint protection y las recomendaciones de Azure Security Center detecta y proporciona la evaluación del mantenimiento [admite](https://docs.microsoft.com/azure/security-center/security-center-os-coverage#supported-platforms-for-windows-computers-and-vms) versiones de las soluciones de protección de punto de conexión. En este tema se explica los escenarios que generan las siguientes dos recomendaciones para soluciones de protección de punto de conexión por Azure Security Center.

* **Instalar las soluciones de protección de punto de conexión en la máquina virtual**
* **Resolver problemas de estado de endpoint protection en los equipos**

## <a name="windows-defender"></a>Windows Defender

* El **"Instalar soluciones de protección de punto de conexión en la máquina virtual"** recomendación se genera cuando [Get MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) se ejecuta y el resultado es **AMServiceEnabled: False**

* El **"Solucionar problemas de estado de endpoint protection en los equipos"** recomendación se genera cuando [Get MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) ejecuciones y una o ambas de las siguientes acciones se produce:

  * Al menos una de las siguientes propiedades es false:

     **AMServiceEnabled**

     **AntispywareEnabled**

     **RealTimeProtectionEnabled**

     **BehaviorMonitorEnabled**

     **IoavProtectionEnabled**

     **OnAccessProtectionEnabled**

  * Si una o ambas de las siguientes propiedades es mayor o igual a 7.

     **AntispywareSignatureAge**

     **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Protección de punto de conexión de Microsoft System Center

* El **"Instalar soluciones de protección de punto de conexión en la máquina virtual"** recomendación se genera cuando se importan **SCEPMpModule ("$env: archivos seguridad Client\MpProvider\MpProvider.psd1")** y ejecutando **Get MProtComputerStatus** de resultados con **AMServiceEnabled = false**

* El **"Solucionar problemas de estado de endpoint protection en los equipos"** recomendación se genera cuando **Get MprotComputerStatus** ejecuciones y una o ambas de las siguientes acciones se produce:

    * Al menos una de las siguientes propiedades es false:

       **AMServiceEnabled**
    
       **AntispywareEnabled**
    
       **RealTimeProtectionEnabled**
    
       **BehaviorMonitorEnabled**
    
       **IoavProtectionEnabled**
    
       **OnAccessProtectionEnabled**
          
    * Si una o ambas de las siguientes actualizaciones de firma es mayor o igual a 7. 

       **AntispywareSignatureAge**
    
       **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* El **"Instalar soluciones de protección de punto de conexión en la máquina virtual"** recomendación se genera si una o varias de las siguientes comprobaciones no se cumplen:
    * **Agente de seguridad HKLM:\SOFTWARE\TrendMicro\Deep** existe
    * **HKLM:\SOFTWARE\TrendMicro\Deep seguridad Agent\InstallationFolder** existe
    * El **dsq_query.cmd** archivo se encuentra en la carpeta de instalación
    * Ejecutando **dsa_query.cmd** de resultados con **Component.AM.mode: en - Trend Micro Deep Security Agent ha detectado**

## <a name="symantec-endpoint-protection"></a>Protección de extremos de Symantec
El **"Instalar soluciones de protección de punto de conexión en la máquina virtual"** recomendación se genera si no se cumple alguna de las siguientes comprobaciones:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

o

* **HKLM:\Software\Wow6432Node\Symantec\Symantec extremo Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

El **"Solucionar problemas de estado de endpoint protection en los equipos"** recomendación se genera si no se cumple alguna de las siguientes comprobaciones:  

* Comprobar la versión de Symantec > = 12:  Ubicación del registro: **HKLM:\Software\Symantec\Symantec Protection\CurrentVersion del punto de conexión"-"PRODUCTVERSION"de valor**

* Comprobar el estado de protección en tiempo Real: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* Comprobar el estado de actualización de firma: **Punto de conexión HKLM\Software\Symantec\Symantec Protection\CurrentVersion\public-opstate\LatestVirusDefsDate < = 7 días**

* Comprobar el estado de examen completo: **Punto de conexión HKLM:\Software\Symantec\Symantec Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime < = 7 días**

* Encontrar el número de versión de firma ruta de acceso a la versión de la firma de Symantec 12: **Las rutas de acceso del registro + "CurrentVersion\SharedDefs"-"SRTSP" de valor** 

* Ruta de acceso a la versión de la firma de Symantec 14: **Las rutas de acceso del registro + "CurrentVersion\SharedDefs\SDSDefs"-"SRTSP" de valor**

Rutas de acceso del registro:

**"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path; ** 
 **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>McAfee endpoint protection para Windows

El **"Instalar soluciones de protección de punto de conexión en la máquina virtual"** recomendación se genera si no se cumplen las siguientes comprobaciones:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** existe

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

El **"Solucionar problemas de estado de endpoint protection en los equipos"** recomendación se genera si no se cumplen las siguientes comprobaciones:

* McAfee, versión: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* Busque la versión de la firma: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "dwContentMajorVersion"**

* Buscar fecha de firma: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "szContentCreationDate" >= 7 days**

* Buscar fecha de detección: **HKLM:\Software\McAfee\Endpoint\AV\ODS-valor "LastFullScanOdsRunTime" > = 7 días**

## <a name="troubleshoot-and-support"></a>Solución de problemas y soporte técnico

### <a name="troubleshoot"></a>Solución de problemas

Están disponibles en los registros de extensión de Microsoft Antimalware:  
**%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(Or PaaSAntimalware)\1.5.5.x(version#)\CommandExecution.log**

### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). O bien, puede presentar una incidencia de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).
