---
title: 'Tutorial: Obtención de datos mediante Reporting API de Azure AD con certificados | Microsoft Docs'
description: En este tutorial se explica cómo usar Reporting API de Azure AD con credenciales de certificado para obtener datos de directorios sin intervención del usuario.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 5c54af76fc1e145ea062c6bcb37f354a7de94781
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364183"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Tutorial: Obtención de datos mediante Reporting API de Azure AD con certificados

Las [API de generación de informes de Azure Active Directory (Azure AD)](concept-reporting-api.md) proporcionan acceso mediante programación a los datos a través de un conjunto de API de REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas. Si quiere acceder a Reporting API de Azure AD sin intervención del usuario, debe configurar el acceso para usar certificados.

En este tutorial obtendrá información sobre cómo crear un certificado de prueba y usarlo para obtener acceso a Microsoft Graph API para la creación de informes. No se recomienda usar el certificado de prueba en un entorno de producción. 

## <a name="prerequisites"></a>Requisitos previos

1. En primer lugar, complete los [requisitos previos para acceder a la API de generación de informes de Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md). 

2. Descargue e instale [Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

3. Instale [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Este módulo proporciona varios cmdlets de utilidades incluidos los siguientes:
    - Las bibliotecas ADAL necesarias para la autenticación
    - Los tokens de acceso de usuario, las claves de aplicación y los certificados mediante ADAL
    - Graph API con control de resultados paginados

4. Si es la primera vez que usa el módulo, ejecute **Install-MSCloudIdUtilsModule**; en caso contrario, puede importarlo mediante el comando de PowerShell **Import-Module**.

La sesión debe tener un aspecto similar a esta pantalla:

  ![Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)
  
## <a name="create-a-test-certificate"></a>Creación de un certificado de prueba

1. Use el commandlet **New-SelfSignedCertificate** de PowerShell para crear un certificado de prueba.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

2. Use el commandlet **Export-Certificate** para exportarlo en un archivo de certificado.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="register-the-certificate-in-your-app"></a>Registrar el certificado en la aplicación

1. Vaya a [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**, luego, seleccione **Registros de aplicaciones** y elija la aplicación de la lista. 

2. Seleccione **Configuración** > **Claves** y, después, seleccione **Cargar clave pública**.

3. Seleccione el archivo de certificado del paso anterior y seleccione **Guardar**. 

4. Anote el identificador de la aplicación y la huella digital del certificado que acaba de registrar con la aplicación. Para buscar la huella digital, desde la página de aplicación en el portal, vaya a **Configuración** y haga clic en **Claves**. La huella digital se encontrará en la lista **Claves públicas**.

5. Abra el manifiesto de aplicación en el editor de manifiesto insertado y reemplace la propiedad *keyCredentials* por la información del nuevo certificado con el siguiente esquema. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Guarde el manifiesto. 
  
## <a name="get-an-access-token-for-ms-graph-api"></a>Obtener un token de acceso para MS Graph API

1. Use el cmdlet **Get-MSCloudIdMSGraphAccessTokenFromCert** del módulo MSCloudIdUtils de PowerShell pasando el identificador de la aplicación y la huella digital que obtuvo en el paso anterior. 

 ![Azure Portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Uso del token de acceso para llamar a Graph API

1. Ahora, puede usar el token de acceso en el script de Powershell para consultar Graph API. Use el cmdlet **Invoke-MSCloudIdMSGraphQuery** desde MSCloudIDUtils para enumerar el punto de conexión de SignIns y DirectoryAudits. Este cmdlet controla los resultados de paginado múltiple y los envía a la canalización de PowerShell.

2. Consulte el punto de conexión DirectoryAudits para recuperar los registros de auditoría. 
 ![Azure Portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

3. Consulte el punto de conexión de SignIns para recuperar los registros de inicio de sesión.
 ![Azure Portal](./media/tutorial-access-api-with-certificates/query-signins.png)

4. Ahora puede exportar estos datos en un archivo CSV y guardarlos en un sistema SIEM. También puede encapsular el script en una tarea programada para obtener datos de Azure AD de su inquilino periódicamente sin tener que almacenar las claves de aplicación en el código fuente. 

## <a name="next-steps"></a>Pasos siguientes

* [Obtención de una primera impresión de Reporting API](concept-reporting-api.md)
* [Referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referencia de la API de generación de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



