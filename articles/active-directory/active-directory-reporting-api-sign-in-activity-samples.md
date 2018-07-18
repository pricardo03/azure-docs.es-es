---
title: Ejemplos de la API de informes de actividad de inicio de sesión de Azure Active Directory | Microsoft Docs
description: Introducción a la API de informes de Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 52d49770014a5fb6a5eec644868e702c8a8d9ef3
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36224905"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Ejemplos de la API de informe de actividad de inicio de sesión de Azure Active Directory
Este artículo forma parte de una serie de artículos sobre la API de generación de informes de Azure Active Directory.  
La característica de generación de informes de Azure AD proporciona una API que permite acceder a los datos de actividades de inicio de sesión mediante el uso de código o herramientas relacionadas.  
Este artículo se centra en proporcionar el código de ejemplo para la **API de actividad de inicio de sesión**.

Consulte:

* [Registros de auditoría](active-directory-reporting-azure-portal.md#activity-reports) para más información conceptual
* [Introducción a la API de generación de informes de Azure Active Directory](active-directory-reporting-api-getting-started.md) para obtener más información sobre esta API


## <a name="prerequisites"></a>requisitos previos
Para poder usar los ejemplos de este artículo, debe completar los [requisitos previos para acceder a la API de generación de informes de Azure AD](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>Script de PowerShell

```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfSignInDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/signIns"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into SignIns$i.json"
        if($docCount -le $countOfSignInDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath SignIns$i.json -append  -Force       
        }
        else
        {           
            $docCount=0
            $i = $i+1
        }
            
        #Get url from next link
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'         
    }while($url -ne $null)
    Write-Output "Total Fetched record count is : $totalFetchedRecordCount"
                
} else {
    Write-Host "ERROR: No Access Token"
}

```




## <a name="executing-the-script"></a>Ejecución del script
Una vez que termine de editar el script, ejecútelo y compruebe que el informe de registros de inicio de sesión devuelva los datos esperados.

El script devuelve la salida del informe de inicio de sesión en formato JSON. También se crea un archivo `SignIns.json` con la misma salida. Puede experimentar modificando el script para que devuelva datos de otros informes y convertir en comentario los formatos de salida que no necesite.

## <a name="next-steps"></a>Pasos siguientes
* ¿Quiere personalizar los ejemplos de este artículo? Consulte [Azure Active Directory sign-in activity report API samples](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)(Referencia de la API de la actividad de inicio de sesión de Azure Active Directory). 
* Si quiere obtener una descripción completa del uso de la API de generación de informes de Azure Active Directory, consulte el artículo de [introducción a la API de generación de informes de Azure Active Directory](active-directory-reporting-api-getting-started.md).
* Si quiere obtener más información sobre informes de Azure Active Directory, consulte la [guía de generación de informes de Azure Active Directory](active-directory-reporting-guide.md).  

