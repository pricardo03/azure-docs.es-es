---
title: Directivas de autenticación de Azure API Management | Microsoft Docs
description: Aprenda sobre las directivas de autenticación disponibles para su uso en Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 9ee4a9fb5c63061eed32389b5672652aad01208a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994959"
---
# <a name="api-management-authentication-policies"></a>Directivas de autenticación de Azure API Management
En este tema se proporciona una referencia para las siguientes directivas de API Management. Para obtener más información sobre cómo agregar y configurar directivas, consulte [Directivas en Administración de API](https://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Directivas de autenticación  
  
-   [Autenticar con opción básica](api-management-authentication-policies.md#Basic) : autenticar con un servicio de back-end mediante la autenticación básica.  
  
-   [Autenticar con certificado de cliente](api-management-authentication-policies.md#ClientCertificate) : autenticar con un servicio de back-end mediante certificados de cliente.  

-   [Autenticar con identidad administrada](api-management-authentication-policies.md#ManagedIdentity) -autenticarse con el [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para el servicio API Management.  
  
##  <a name="Basic"></a> Autenticación con Basic  
 Use la directiva `authentication-basic` para realizar la autenticación con un servicio de back-end mediante autenticación Básica. Esta directiva establece eficazmente el encabezado de autorización HTTP en el valor correspondiente a las credenciales proporcionadas en la directiva.  
  
### <a name="policy-statement"></a>Instrucción de la directiva  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Ejemplo  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementos  
  
|Name|DESCRIPCIÓN|Obligatorio|  
|----------|-----------------|--------------|  
|authentication-basic|Elemento raíz.|Sí|  
  
### <a name="attributes"></a>Atributos  
  
|Name|DESCRIPCIÓN|Obligatorio|Valor predeterminado|  
|----------|-----------------|--------------|-------------|  
|username|Especifica el nombre de usuario de la credencial básica.|Sí|N/D|  
|password|Especifica la contraseña de usuario de la credencial básica.|Sí|N/D|  
  
### <a name="usage"></a>Uso  
 Esta directiva puede usarse en las siguientes [secciones](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) y [ámbitos](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) de directiva.  
  
-   **Secciones de la directiva:** inbound (entrada)  
  
-   **Ámbitos de la directiva:** API  
  
##  <a name="ClientCertificate"></a> Autenticación Básica  
 Use la directiva `authentication-certificate` para realizar la autenticación con un servicio de back-end mediante un certificado de cliente. El certificado se debe [instalar primero en API Management](https://go.microsoft.com/fwlink/?LinkID=511599) y se identifica mediante su huella digital.  
  
### <a name="policy-statement"></a>Instrucción de la directiva  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Ejemplo  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementos  
  
|Name|DESCRIPCIÓN|Obligatorio|  
|----------|-----------------|--------------|  
|authentication-certificate|Elemento raíz.|Sí|  
  
### <a name="attributes"></a>Atributos  
  
|Name|DESCRIPCIÓN|Obligatorio|Valor predeterminado|  
|----------|-----------------|--------------|-------------|  
|thumbprint|La huella digital del certificado de cliente.|Sí|N/D|  
  
### <a name="usage"></a>Uso  
 Esta directiva puede usarse en las siguientes [secciones](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) y [ámbitos](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) de directiva.  
  
-   **Secciones de la directiva:** inbound (entrada)  
  
-   **Ámbitos de la directiva:** API  

##  <a name="ManagedIdentity"></a> Autenticar con identidad administrada  
 Use el `authentication-managed-identity` directiva para autenticarse con un servicio de back-end mediante la identidad administrada del servicio API Management. Esta directiva usa eficazmente la identidad administrada para obtener un token de acceso de Azure Active Directory para acceder al recurso especificado. 
  
### <a name="policy-statement"></a>Instrucción de la directiva  
  
```xml  
<authentication-managed-identity resource="resource" output-token-variable-name="token-variable" ignore-error="true|false"/>  
```  
  
### <a name="example"></a>Ejemplo  
  
```xml  
<authentication-managed-identity resource="https://graph.windows.net" output-token-variable-name="test-access-token" ignore-error="true" /> 
```  
  
### <a name="elements"></a>Elementos  
  
|Name|DESCRIPCIÓN|Obligatorio|  
|----------|-----------------|--------------|  
|authentication-managed-identity |Elemento raíz.|Sí|  
  
### <a name="attributes"></a>Atributos  
  
|Name|DESCRIPCIÓN|Obligatorio|Valor predeterminado|  
|----------|-----------------|--------------|-------------|  
|resource|String. El URI de Id. de aplicación de la API (recurso seguro) de la web de destino en Azure Active Directory.|Sí|N/D|  
|output-token-variable-name|String. Nombre de la variable de contexto que recibirá el valor del token como un tipo de objeto `string`.|Sin |N/D|  
|ignore-error|Booleano. Si establece en `true`, la canalización de pólizas seguirá ejecutándose incluso si no se obtiene un token de acceso.|Sin |false|  
  
### <a name="usage"></a>Uso  
 Esta directiva puede usarse en las siguientes [secciones](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) y [ámbitos](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) de directiva.  
  
-   **Secciones de la directiva:** entrante  
  
-   **Ámbitos de la directiva:** global, producto, API, operación  

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo trabajar con directivas, consulte:

+ [Directivas de Azure API Management](api-management-howto-policies.md)
+ [API de transformación](transform-api.md)
+ En la [Referencia de directivas](api-management-policy-reference.md) se muestra una lista completa de declaraciones de directivas y su configuración
+ [Ejemplos de directivas](policy-samples.md)   
