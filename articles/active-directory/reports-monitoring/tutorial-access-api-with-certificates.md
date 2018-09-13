---
title: Obtención de datos mediante Reporting API de Azure AD con certificados | Microsoft Docs
description: Explica cómo usar Reporting API de Azure AD con credenciales de certificado para obtener datos de directorios sin intervención del usuario.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7e2dd4c50a1d6995302c5a2a6f9b4877253d0a41
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382287"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Obtención de datos mediante Reporting API de Azure Active Directory con certificados

Las [API de generación de informes de Azure Active Directory (Azure AD)](concept-reporting-api.md) proporcionan acceso mediante programación a los datos a través de un conjunto de API de REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas. Si quiere acceder a Reporting API de Azure AD sin intervención del usuario, puede configurar el acceso para usar certificados.

Esto implica los pasos siguientes:

1. [Instalación de los requisitos previos](#install-prerequisites)
2. [Registrar el certificado en la aplicación](#register-the-certificate-in-your-app)
3. [Obtener un token de acceso para MS Graph API](#get-an-access-token-for-ms-graph-api)
4. [Consultar los puntos de conexión de MS Graph API](#query-the-ms-graph-api-endpoints)


## <a name="install-prerequisites"></a>Requisitos previos de instalación

1. En primer lugar, asegúrese de que ha completado los [requisitos previos para acceder a la API de generación de informes de Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md). 

2. Descargue e instale Azure AD Powershell V2, según las instrucciones de [Azure Active Directory PowerShell(https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

3. Instale MSCloudIDUtils desde [PowerShellGallery - MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Este módulo proporciona varios cmdlets de utilidades incluidos los siguientes:
    - Las bibliotecas ADAL necesarias para la autenticación
    - Los tokens de acceso de usuario, las claves de aplicación y los certificados mediante ADAL
    - Graph API con control de resultados paginados

4. Si es la primera vez que usa el módulo, ejecute **Install-MSCloudIdUtilsModule** para completar la instalación; en caso contrario, puede simplemente importarlo mediante el comando de Powershell **Import-Module**.

La sesión debe tener un aspecto similar a esta pantalla:

  ![Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)

## <a name="register-the-certificate-in-your-app"></a>Registrar el certificado en la aplicación

1. En primer lugar, vaya a la página de registro de aplicaciones. Para ello, vaya a [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory** y después en **Registros de aplicaciones** y elija la aplicación en la lista. 

2. Luego, siga los pasos necesarios para [registrar el certificado con Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-certificate-credentials#register-your-certificate-with-azure-ad) para la aplicación. 

3. Anote el identificador de la aplicación y la huella digital del certificado que acaba de registrar con la aplicación. Para buscar la huella digital, desde la página de aplicación en el portal, vaya a **Configuración** y haga clic en **Claves**. La huella digital se encontrará en la lista **Claves públicas**.

  
## <a name="get-an-access-token-for-ms-graph-api"></a>Obtener un token de acceso para MS Graph API

Para obtener un token de acceso para MS Graph API, use el cmdlet **Get-MSCloudIdMSGraphAccessTokenFromCert** del módulo MSCloudIdUtils de PowerShell. 

>[!NOTE]
>Tendrá que usar el identificador de aplicación (también conocido como ClientID) y la huella digital del certificado con la clave privada instalada en el almacén de certificados del equipo (el almacén de certificados de CurrentUser o LocalMachine).
>

 ![Azure Portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Uso del token de acceso para llamar a Graph API

Ahora, puede usar el token de acceso en el script de Powershell para consultar Graph API. A continuación se muestra un ejemplo de uso del cmdlet **Invoke-MSCloudIdMSGraphQuery** desde MSCloudIDUtils para enumerar el punto de conexión de SignIns o DirectoryAudits. Este cmdlet controla los resultados de paginado múltiple y los envía luego a la canalización de PowerShell.

### <a name="query-the-directoryaudits-endpoint"></a>Consulta del punto de conexión de DirectoryAudits
 ![Azure Portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

 ### <a name="query-the-signins-endpoint"></a>Consulta del punto de conexión de SignIns
 ![Azure Portal](./media/tutorial-access-api-with-certificates/query-signins.png)

Ahora puede exportar estos datos en un archivo CSV y guardarlos en un sistema SIEM. También puede encapsular el script en una tarea programada para obtener datos de Azure AD de su inquilino periódicamente sin tener que almacenar las claves de aplicación en el código fuente. 


## <a name="next-steps"></a>Pasos siguientes

* [Obtención de una primera impresión de Reporting API](concept-reporting-api.md)
* [Referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referencia de la API de generación de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



