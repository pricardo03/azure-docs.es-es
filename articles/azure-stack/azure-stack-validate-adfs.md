---
title: Validar la integración de ADFS para Azure Stack
description: Use Azure Stack Readiness Checker para validar la integración de ADFS para Azure Stack.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: 0aa13f2fced9122163d5278b5bb50cc1e11a0379
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946887"
---
# <a name="validate-adfs-integration-for-azure-stack"></a>Validar la integración de ADFS para Azure Stack

Use la herramienta Azure Stack Readiness Checker (AzsReadinessChecker) para confirmar que su entorno esté listo para usarse con Azure Stack. Debe confirmar la integración de ADFS antes de comenzar con la integración del centro de datos o antes de realizar una implementación de Azure Stack.

La herramienta Readiness Checker valida:

* Los *metadatos de federación* contienen los elementos XML válidos para la federación.
* El *certificado SSL de ADFS* se puede recuperar y se puede compilar una cadena de confianza. En la marca de ADFS, debe confiar en la cadena de certificados SSL. Es necesario que el certificado lo firme la misma *autoridad de certificación* que firmó los certificados de implementación de Azure Stack o un asociado de confianza de la autoridad raíz. Para ver la lista completa de asociados de la autoridad raíz de confianza, consulte: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca
* *El certificado de firma de ADFS* es de confianza y no expirará en breve.

Para obtener más información acerca de los requisitos de la integración del centro de datos de Azure Stack, consulte [Integración del centro de datos de Azure Stack: identidad](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>Obtención de la herramienta Readiness Checker

Descargue la versión más reciente de Azure Stack Readiness Checker (AzsReadinessChecker) desde [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Requisitos previos

Deben cumplirse los siguientes requisitos previos.

**Equipo donde se ejecuta la herramienta:**

* Windows 10 o Windows Server 2016, con conectividad de dominio.
* Azure PowerShell 5.1 o posterior. Para comprobar la versión, ejecute el siguiente cmd de PowerShell y luego revise la versión *principal* y las versiones *secundarias*:  
   > `$PSVersionTable.PSVersion`
* La versión más reciente de la herramienta [Microsoft Azure Stack Readiness Checker](https://aka.ms/AzsReadinessChecker).

**Entorno de los Servicios de federación de Active Directory:**

Necesita al menos uno de los siguientes formatos de metadatos:

* La dirección URL de los metadatos de la federación de ADFS. Por ejemplo: `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
* Archivo XML de metadatos de federación. Por ejemplo, FederationMetadata.xml

## <a name="validate-adfs-integration"></a>Validación de la integración de ADFS

1. En un equipo que cumpla los requisitos previos, abra un símbolo del sistema administrativo de PowerShell y ejecute el siguiente comando para instalar AzsReadinessChecker.

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Desde el símbolo del sistema de PowerShell, ejecute lo siguiente para iniciar la validación. Especifique el valor de **-CustomADFSFederationMetadataEndpointUri** como el URI para los metadatos de federación:

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Después de ejecutar la herramienta, revise el resultado. Confirme que el estado es correcto para los requisitos de integración de ADFS. Una validación correcta tiene un aspecto similar al de la siguiente imagen.

    ```
    Invoke-AzsADFSValidation v1.1809.1001.1 started.

    Testing ADFS Endpoint https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

            Read Metadata:                         OK
            Test Metadata Elements:                OK
            Test SSL ADFS Certificate:             OK
            Test Certificate Chain:                OK
            Test Certificate Expiry:               OK

    Details:
    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.
    Additional help URL: https://aka.ms/AzsADFSIntegration

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsADFSValidation Completed
    ```

En entornos de producción, el proceso de prueba de cadenas de certificados de confianza que se realiza desde una estación de trabajo de operadores no puede considerarse totalmente indicativo de la postura de confianza de PKI en la infraestructura de Azure Stack. La red VIP pública de la marca de Azure Stack necesita tener conectividad a la CRL para la infraestructura PKI.

## <a name="report-and-log-file"></a>Informe y archivo de registro

Cada vez que se ejecuta la validación, los resultados se registran en **AzsReadinessChecker.log** y **AzsReadinessCheckerReport.json**. La ubicación de estos archivos se muestra con los resultados de validación de PowerShell.

Estos archivos de validación pueden ayudarle a compartir el estado antes de implementar Azure Stack o investigar problemas de validación. Ambos archivos conservan los resultados de cada comprobación de validación posterior. El informe ofrece la confirmación del equipo de implementación de la configuración de identidad. El archivo de registro puede ayudar al equipo de implementación o de soporte técnico a investigar los problemas de validación.

De forma predeterminada, ambos archivos se escriben en `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Uso:

* El parámetro **-OutputPath** *ruta de acceso* al final de la línea de comandos de ejecución para especificar otra ubicación para el informe.
* El parámetro **-CleanReport** al final del comando de ejecución para borrar *AzsReadinessCheckerReport.json* de la información del informe previo. Para obtener más información, consulte el artículo [Informe de validación de Azure Stack](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Errores de validación

Si se producen errores en una comprobación de validación, los detalles sobre los errores se muestran en la ventana de PowerShell. La herramienta también registra información en *AzsReadinessChecker.log*.

Los ejemplos siguientes ofrecen instrucciones sobre errores de validación comunes.

### <a name="command-not-found"></a>No se encontró el comando

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Causa**: PowerShell Autoload no se pudo cargar correctamente en el módulo de Readiness Checker.

**Resolución**: importe el módulo de Readiness Checker de forma explícita. Copie y pegue el código siguiente en PowerShell y actualice la \<versión\> con el número de versión correspondiente a la versión instalada actualmente.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Pasos siguientes

[Vista del informe de preparación](azure-stack-validation-report.md)  
[Consideraciones generales sobre la integración de Azure Stack](azure-stack-datacenter-integration.md)  
