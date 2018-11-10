---
title: Preparación de un host de extensiones de Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo preparar un host de extensiones que se habilite automáticamente mediante un paquete de actualización futuro de Azure Stack.
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 11/02/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.openlocfilehash: 4376b9e89aeef32987f7a3bb29ca6815e941ba00
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960249"
---
# <a name="prepare-for-extension-host-for-azure-stack"></a>Preparación de un host de extensiones de Azure Stack

El host de extensiones protege a Azure Stack reduciendo el número de puertos TCP/IP necesarios. En este artículo, se explica cómo preparar Azure Stack para el host de extensiones, que se habilita automáticamente mediante un paquete de actualización de Azure Stack posterior a la actualización 1808.

## <a name="certificate-requirements"></a>Requisitos de certificados

El host de extensiones implementa dos nuevos espacios de nombres de dominio que permiten garantizar entradas de host únicas para cada extensión del portal. Los nuevos espacios de nombres de dominio requieren dos certificados comodín adicionales para garantizar una comunicación segura.

En la tabla, se muestran los nuevos espacios de nombres y los certificados asociados:

| Carpeta de implementación | Nombres alternativos del firmante (SAN) y firmante del certificado requeridos | Ámbito (por región) | Nombre del subdominio |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| Administración del host de extensiones | *.adminhosting.\<region>.\<fqdn> (certificados SSL comodín) | Administración del host de extensiones | adminhosting.\<region>.\<fqdn> |
| Host de extensiones público | *.hosting.\<region>.\<fqdn> (certificados SSL comodín) | Host de extensiones público | hosting.\<region>.\<fqdn> |

Los requisitos detallados de los certificados pueden consultarse en el artículo [Requisitos de certificados de infraestructura de clave pública de Azure Stack](azure-stack-pki-certs.md).

## <a name="create-certificate-signing-request"></a>Creación de una solicitud de firma de certificados

La herramienta Readiness Checker de Azure Stack permite crear una solicitud de firma de certificados para los dos nuevos certificados SSL obligatorios. Siga los pasos del artículo [Generación de solicitudes de firma de certificados de Azure Stack](azure-stack-get-pki-certs.md).

> [!Note]  
> En función de cómo se hayan solicitado los certificados SSL, es posible que pueda omitir este paso.

## <a name="validate-new-certificates"></a>Validación de nuevos certificados

1. Abra PowerShell con permisos elevados en el host de ciclo de vida de hardware o en la estación de trabajo de administración de Azure Stack.
2. Ejecute el siguiente cmdlet para instalar la herramienta Readiness Checker de Azure Stack.

    ```PowerShell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

3. Ejecute el script siguiente para crear la estructura de carpetas necesaria:

    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > Si realiza la implementación con Servicios de federación de Azure Active Directory (AD FS), deben agregarse los directorios siguientes a **$directories** en el script: `ADFS`, `Graph`.

4. Ejecute los cmdlets siguientes para iniciar la comprobación de certificados:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ```

5. Sitúe los certificados en los directorios apropiados.

6. Compruebe que la salida y todos los certificados pasen todas las pruebas.


## <a name="import-extension-host-certificates"></a>Importación de certificados del host de extensiones

Para seguir los pasos que se indican a continuación, utilice un equipo que pueda conectarse al punto de conexión con privilegios de Azure Stack. Asegúrese de que tiene acceso a los nuevos archivos de certificado de ese equipo.

1. Para seguir los pasos que se indican a continuación, utilice un equipo que pueda conectarse al punto de conexión con privilegios de Azure Stack. Asegúrese de que tiene acceso a los nuevos archivos de certificado de ese equipo.
2. Abra PowerShell ISE para ejecutar los siguientes bloques de script.
3. Importe el certificado del punto de conexión de hospedaje. Ajuste el script para que coincida con el entorno.
4. Importe el certificado del punto de conexión de hospedaje de administración.

    ```PowerShell  

    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```
5. Importe el certificado del punto de conexión de hospedaje.
    ```PowerShell  
    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$HostingCertContent = [Byte[]](Get-Content c:\certificate\myhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```



### <a name="update-dns-configuration"></a>Actualización de la configuración de DNS

> [!Note]  
> Este paso no es necesario si usa la delegación de zonas DNS para la integración de DNS.
Si se han configurado registros A de un determinado host para publicar puntos de conexión de Azure Stack, será necesario crear dos registros A más:

| IP | Nombre de host. | Escriba |
|----|------------------------------|------|
| \<IP> | Adminhosting.<Region>.<FQDN> | Una  |
| \<IP> | Hosting.<Region>.<FQDN> | Una  |

Las direcciones IP asignadas pueden recuperarse utilizando un punto de conexión con privilegios y ejecutando el cmdlet **Get-AzureStackStampInformation**.

### <a name="ports-and-protocols"></a>Puertos y protocolos

En el artículo [Integración del centro de datos de Azure Stack: publicar puntos de conexión](azure-stack-integrate-endpoints.md), se describen los puertos y protocolos que requieren comunicaciones entrantes para publicar puntos de conexión de Azure Stack antes del lanzamiento del host de extensiones.

### <a name="publish-new-endpoints"></a>Publicación de nuevos puntos de conexión

Hay dos nuevos puntos de conexión que deben publicarse a través del firewall. Las direcciones IP asignadas del grupo de VIP públicas pueden recuperarse utilizando el cmdlet **Get-AzureStackStampInformation**.

> [!Note]  
> Realice este cambio antes de habilitar el host de extensiones. De este modo, modo los portales de Azure Stack estarán disponibles en todo momento.

| Punto de conexión (VIP) | Protocolo | Puertos |
|----------------|----------|-------|
| AdminHosting | HTTPS | 443 |
| Hospedaje | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>Actualización de las reglas de publicación existentes (tras las habilitación de del host de extensiones)

> [!Note]  
> El paquete de actualización de Azure Stack 1808 **no** habilita todavía el host de extensiones. No obstante, permite prepararse para el host de extensiones importando los certificados necesarios. No cierre ningún puerto antes de que el host de extensiones se habilite automáticamente con un paquete de actualización de Azure Stack Update posterior al 1808.

Los siguientes puertos de punto de conexión existentes deben cerrarse en las reglas del firewall existentes.

> [!Note]  
> Es recomendable que estos puertos se cierren una vez que la validación se ha realizado correctamente.

| Punto de conexión (VIP) | Protocolo | Puertos |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| Portal (administrador) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015 |
| Portal (usuario) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003 |
| Azure Resource Manager (administrador) | HTTPS | 30024 |
| Azure Resource Manager (usuario) | HTTPS | 30024 |

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de la [integración del firewall](azure-stack-firewall.md).
- Más información acerca de la [generación de solicitudes de firma de certificados de Azure Stack](azure-stack-get-pki-certs.md)
