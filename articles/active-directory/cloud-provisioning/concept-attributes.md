---
title: Descripción del esquema y las expresiones personalizadas de Azure AD
description: En este tema se describe el esquema de Azure AD, los atributos del agente de aprovisionamiento y las expresiones personalizadas.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: eae594bcc20e3c4ed1c6fbd0333699de8c9f4452
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795531"
---
# <a name="understanding-the-azure-ad-schema"></a>Descripción del esquema de Azure AD
Un objeto de Azure AD, como cualquier directorio, es una construcción de datos de alto nivel mediante programación que representa elementos como usuarios, grupos y contactos.  Al crear un nuevo usuario o contacto en Azure AD, está creando una nueva instancia de ese objeto.  Estas instancias se pueden diferenciar en función de sus propiedades.

En Azure AD, las propiedades son los elementos responsables de almacenar información sobre una instancia de un objeto en Azure AD.  

El esquema de Azure AD define las reglas para las que las propiedades se pueden usar en una entrada, los tipos de valores que esas propiedades pueden tener y el modo en que los usuarios pueden interactuar con esos valores. 

Azure AD tiene dos tipos de propiedades.  Las propiedades son:
- **Propiedades integradas**: las predefinidas por el esquema de Azure AD.  Proporcionan diferentes usos y pueden o no ser accesibles.
- **Extensiones de directorio**: las que se proporcionan para que pueda personalizar Azure AD para su propio uso.  Por ejemplo, si ha extendido Active Directory en el entorno local con un determinado atributo y desea pasar ese atributo, puede usar una de las propiedades personalizadas que se proporcionan. 

## <a name="attributes-and-expressions"></a>Atributos y expresiones
Cuando un objeto, como un usuario, se aprovisiona en Azure AD, se crea una nueva instancia del objeto de usuario.  Esta creación incluye las propiedades de ese objeto, que también se conocen como atributos.  Inicialmente, el objeto recién creado tendrá sus atributos establecidos en los valores que las reglas de sincronización determinen.  Estos atributos se mantienen entonces actualizados a través del agente de aprovisionamiento en la nube.

![](media/concept-attributes/attribute1.png)

Por ejemplo, si un usuario forma parte del Departamento de marketing, su atributo de departamento de Azure AD se creará inicialmente cuando se aprovisione y, a continuación, el valor se establecerá en Marketing.  Pero entonces, seis meses más tarde, cambia a Ventas.  El atributo de su departamento de AD local se cambia a Ventas.  Este cambio se sincronizará en Azure AD y se reflejará en el objeto de usuario de Azure AD.

La sincronización de atributos puede ser directa, en la que el valor de Azure AD se establece directamente en el valor del atributo en el entorno local.  O bien, puede haber una expresión de programación que controle esta sincronización.  Una expresión de programación sería necesaria en los casos en que sea necesario realizar alguna lógica o una determinación para rellenar el valor.

Por ejemplo, si tuviera mi atributo de correo electrónico ("john.smith@contoso.com") y necesitara quitar la parte "@contoso.com" y pasar solo el valor "john.smith", usaría algo parecido a esto:

`Replace([mail], "@contoso.com", , ,"", ,)`  

**Ejemplo de entrada y salida:** <br>

* **ENTRADA** (mail): "john.smith@contoso.com"
* **SALIDA**: "john.smith"

Para más información sobre la escritura de expresiones personalizadas y la sintaxis, consulte [Escritura de expresiones para la asignación de atributos en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data).

En la tabla siguiente se enumeran algunos atributos comunes y se describe cómo se sincronizan con Azure AD.


|Active Directory local|Tipo de asignación|Azure AD|
|-----|-----|-----|
|cn|Directo|commonName
|countryCode|Directo|countryCode|
|DisplayName|Directo|DisplayName|
|givenName|Expression|givenName|
|objectGUID|Directo|sourceAnchorBinary|  
|userprincipalName|Directo|userPrincipalName|
|ProxyAdress|Directo|ProxyAddress|

## <a name="viewing-the-schema"></a>Visualización del esquema
Para ver el esquema y comprobarlo, siga estos pasos:

1.  Vaya a [Probador de Graph](https://developer.microsoft.com/graph/graph-explorer).
2.  Inicie sesión con su cuenta de administrador global.
3.  A la izquierda, haga clic en **modificar permisos** y asegúrese de que **Directory.ReadWrite.All** tiene consentimiento.
4.  Ejecute la consulta siguiente: https://graph.microsoft.com/beta/serviceprincipals/.  Esta consulta devolverá una lista de entidades de servicio.
5.  Busque "appDisplayName": "Active Directory to Azure Active Directory Provisioning" (Active Directory para el aprovisionamiento de Azure Active Directory) y anote el valor de "id:" (identificador).
    ```
    "value": [
            {
                "id": "00d41b14-7958-45ad-9d75-d52fa29e02a1",
                "deletedDateTime": null,
                "accountEnabled": true,
                "appDisplayName": "Active Directory to Azure Active Directory Provisioning",
                "appId": "1a4721b3-e57f-4451-ae87-ef078703ec94",
                "applicationTemplateId": null,
                "appOwnerOrganizationId": "47df5bb7-e6bc-4256-afb0-dd8c8e3c1ce8",
                "appRoleAssignmentRequired": false,
                "displayName": "Active Directory to Azure Active Directory Provisioning",
                "errorUrl": null,
                "homepage": "https://account.activedirectory.windowsazure.com:444/applications/default.aspx?metadata=AD2AADProvisioning|ISV9.1|primary|z",
                "loginUrl": null,
                "logoutUrl": null,
                "notificationEmailAddresses": [],
                "preferredSingleSignOnMode": null,
                "preferredTokenSigningKeyEndDateTime": null,
                "preferredTokenSigningKeyThumbprint": null,
                "publisherName": "Active Directory Application Registry",
                "replyUrls": [],
                "samlMetadataUrl": null,
                "samlSingleSignOnSettings": null,
                "servicePrincipalNames": [
                    "http://adapplicationregistry.onmicrosoft.com/adprovisioningtoaad/primary",
                    "1a4721b3-e57f-4451-ae87-ef078703ec94"
                ],
                "signInAudience": "AzureADMultipleOrgs",
                "tags": [
                    "WindowsAzureActiveDirectoryIntegratedApp"
                ],
                "addIns": [],
                "api": {
                    "resourceSpecificApplicationPermissions": []
                },
                "appRoles": [
                    {
                        "allowedMemberTypes": [
                            "User"
                        ],
                        "description": "msiam_access",
                        "displayName": "msiam_access",
                        "id": "a0326856-1f51-4311-8ae7-a034d168eedf",
                        "isEnabled": true,
                        "origin": "Application",
                        "value": null
                    }
                ],
                "info": {
                    "termsOfServiceUrl": null,
                    "supportUrl": null,
                    "privacyStatementUrl": null,
                    "marketingUrl": null,
                    "logoUrl": null
                },
                "keyCredentials": [],
                "publishedPermissionScopes": [
                    {
                        "adminConsentDescription": "Allow the application to access Active Directory to Azure Active Directory Provisioning on behalf of the signed-in user.",
                        "adminConsentDisplayName": "Access Active Directory to Azure Active Directory Provisioning",
                        "id": "d40ed463-646c-4efe-bb3e-3fa7d0006688",
                        "isEnabled": true,
                        "type": "User",
                        "userConsentDescription": "Allow the application to access Active Directory to Azure Active Directory Provisioning on your behalf.",
                        "userConsentDisplayName": "Access Active Directory to Azure Active Directory Provisioning",
                        "value": "user_impersonation"
                    }
                ],
                "passwordCredentials": []
            },
    ```
6. Reemplace {Service principal Id} por su valor y ejecute la siguiente consulta: `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal id}/synchronization/jobs/`
7. Busque la sección "id": "AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976" y anote el "id:".
    ```
    {
                "id": "AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976",
                "templateId": "AD2AADProvisioning",
                "schedule": {
                    "expiration": null,
                    "interval": "PT2M",
                    "state": "Active"
                },
                "status": {
                    "countSuccessiveCompleteFailures": 0,
                    "escrowsPruned": false,
                    "code": "Active",
                    "lastSuccessfulExecutionWithExports": null,
                    "quarantine": null,
                    "steadyStateFirstAchievedTime": "2019-11-08T15:48:05.7360238Z",
                    "steadyStateLastAchievedTime": "2019-11-20T16:17:24.7957721Z",
                    "troubleshootingUrl": "",
                    "lastExecution": {
                        "activityIdentifier": "2dea06a7-2960-420d-931e-f6c807ebda24",
                        "countEntitled": 0,
                        "countEntitledForProvisioning": 0,
                        "countEscrowed": 15,
                        "countEscrowedRaw": 15,
                        "countExported": 0,
                        "countExports": 0,
                        "countImported": 0,
                        "countImportedDeltas": 0,
                        "countImportedReferenceDeltas": 0,
                        "state": "Succeeded",
                        "error": null,
                        "timeBegan": "2019-11-20T16:15:21.116098Z",
                        "timeEnded": "2019-11-20T16:17:24.7488681Z"
                    },
                    "lastSuccessfulExecution": {
                        "activityIdentifier": null,
                        "countEntitled": 0,
                        "countEntitledForProvisioning": 0,
                        "countEscrowed": 0,
                        "countEscrowedRaw": 0,
                        "countExported": 5,
                        "countExports": 0,
                        "countImported": 0,
                        "countImportedDeltas": 0,
                        "countImportedReferenceDeltas": 0,
                        "state": "Succeeded",
                        "error": null,
                        "timeBegan": "0001-01-01T00:00:00Z",
                        "timeEnded": "2019-11-20T14:09:46.8855027Z"
                    },
                    "progress": [],
                    "synchronizedEntryCountByType": [
                        {
                            "key": "group to Group",
                            "value": 33
                        },
                        {
                            "key": "user to User",
                            "value": 3
                        }
                    ]
                },
                "synchronizationJobSettings": [
                    {
                        "name": "Domain",
                        "value": "{\"DomainFQDN\":\"contoso.com\",\"DomainNetBios\":\"CONTOSO\",\"ForestFQDN\":\"contoso.com\",\"ForestNetBios\":\"CONTOSO\"}"
                    },
                    {
                        "name": "DomainFQDN",
                        "value": "contoso.com"
                    },
                    {
                        "name": "DomainNetBios",
                        "value": "CONTOSO"
                    },
                    {
                        "name": "ForestFQDN",
                        "value": "contoso.com"
                    },
                    {
                        "name": "ForestNetBios",
                        "value": "CONTOSO"
                    },
                    {
                        "name": "QuarantineTooManyDeletesThreshold",
                        "value": "500"
                    }
                ]
            }
    ```
8. Ahora, ejecute la consulta siguiente: `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal Id}/synchronization/jobs/{AD2AAD Provisioning id}/schema`
 
    Ejemplo: https://graph.microsoft.com/beta/serviceprincipals/653c0018-51f4-4736-a3a3-94da5dcb6862/synchronization/jobs/AD2AADProvisioning.e9287a7367e444c88dc67a531c36d8ec/schema

 Reemplace los valores de {Service Principal Id} y de {AD2ADD Provisioning Id} por los suyos.

9. Esta consulta devolverá el esquema.
  ![](media/concept-attributes/schema1.png)
 
## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
