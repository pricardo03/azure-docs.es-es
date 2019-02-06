---
title: Esquema de registros de inicio de sesión de Azure Active Directory en Azure Monitor (versión preliminar) | Microsoft Docs
description: Se describe el esquema de registros de inicio de sesión de Azure AD para su uso en Azure Monitor (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 3c67446f1e828ae528f2e4eb4a0292da2c993ded
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156299"
---
# <a name="interpret-the-azure-ad-sign-in-logs-schema-in-azure-monitor-preview"></a>Interpretación del esquema de registros de inicio de sesión de Azure AD en Azure Monitor (versión preliminar)

En este artículo se describe el esquema de registro de inicio de sesión de Azure Active Directory (Azure AD) en Azure Monitor. La mayoría de la información relacionada con los inicios de sesión se proporciona en el atributo *Properties* del objeto `records`.

```json
{ 
    "records": [ 
    { 
        "time": "2018-05-16T16:09:58.4634578Z", 
        "resourceId": null, 
        "operationName": "Sign-in activity", 
        "operationVersion": "1.0", 
        "category": "SignIn", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "50140", 
        "resultSignature": "None", 
        "resultDescription": "Other", 
        "durationMs": 0, 
        "callerIpAddress": "167.220.0.158", 
        "correlationId": "13e19598-e040-487f-bd32-d38a2cd75d9a", 
        "identity": "arvind harinder", 
        "Level": 4, 
        "location": "US", 
        "properties": { 
            "id": "0782c515-08b6-4029-a65c-29d9a3d20800", 
            "createdDateTime": "2018-05-16T16:09:58.4634578+00:00", 
            "userDisplayName": "Arvind Harinder", 
            "userPrincipalName": "ah@wingtiptoysonline.onmicrosoft.com", 
            "userId": "5b9f356d-9592-42fd-9ec4-d70963909534", 
            "appId": "c44b4083-3bb0-49c1-b47d-974e53cbdf3c", 
            "appDisplayName": "Azure Portal", 
            "ipAddress": "167.220.0.158", 
            "status": { 
                "errorCode": 50140, 
                "failureReason": "Other" 
            }, 
            "clientAppUsed": "Browser", 
            "deviceDetail": { 
                "operatingSystem": "Windows 10", 
                "browser": "Chrome 66.0.3359" 
            }, 
            "location": { 
                "city": "Sammamish", 
                "state": "Washington", 
                "countryOrRegion": "US", 
                "geoCoordinates": { 
                    "latitude": 47.66630935668945, 
                    "longitude": -122.09821319580078 
                } 
            }, 
            "correlationId": "13e19598-e040-487f-bd32-d38a2cd75d9a", 
            "conditionalAccessStatus": 2, 
            "conditionalAccessPolicies": [ 
            { 
                "id": "de7e60eb-ed89-4d73-8205-2227def6b7c9", 
                "displayName": "[billg] SharePoint limited access policy", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "7412a2d8-cbb1-4f1c-96cf-8410b4b8b37b", 
                "displayName": "[BillG] AIP MFA Policy", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "727ed8ea-059d-4d8f-aba5-c1dc500e8b06", 
                "displayName": "[billg] mfa for mail", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "6701123a-b4c6-48af-8565-565c8bf7cabc", 
                "displayName": "Medium signin risk block", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "fbafa2da-cf7f-4ec3-83cf-281188e53f76", 
                "displayName": "Require MFA for admins [Ignite talk] ", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "15339054-709d-4e06-a9ec-342bf043ea56", 
                "displayName": "Enhanced proofing for Azure portal [Ignite talk]", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "2ff9436f-bc72-4ce6-b17e-e7e51153146e", 
                "displayName": "[calebb] AIP policy", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "46ab586b-9447-4847-a889-e60705d96e56", 
                "displayName": "Test policy, OR", 
                "enforcedGrantControls": [], 
                "enforcedSessionControls": [], 
                "result": 3 
            }, 
            { 
                "id": "ceb6e17e-a5d0-4b3a-a150-6c2be2d5b0e9", 
                "displayName": "mm policy with Duo", 
                "enforcedGrantControls": [ 
                    "Require Duo Mfa" 
                ], 
            "enforcedSessionControls": [], 
            "result": 2 
            }, 
            ], 
            "isRisky": false 
            } 
        } 
    } 
```

## <a name="field-descriptions"></a>Descripciones de los campos

| Nombre del campo | DESCRIPCIÓN |
|------------|-------------|
| Hora | Fecha y hora en formato UTC. |
| ResourceId | Este valor no se ha asignado, y puede ignorar este campo de forma segura.  |
| nombreOperación | Para inicios de sesión, este valor es siempre *Actividad de inicio de sesión*. |
| OperationVersion | Versión de la API REST solicitada por el cliente. |
| Categoría | Para inicios de sesión, este valor es siempre *SignIn*. | 
| TenantId | GUID del inquilino asociado con los registros. |
| ResultType | El resultado de la operación de inicio de sesión puede ser *Correcto* o *Error*. | 
| ResultSignature | Contiene el código de error, si existe, de la operación de inicio de sesión. |
| ResultDescription | Proporciona la descripción del error para la operación de inicio de sesión. |
| DurationMs |  Este valor no se ha asignado, y puede ignorar este campo de forma segura.|
| CallerIpAddress | Dirección IP del cliente que realizó la solicitud. | 
| CorrelationId | GUID opcional que el cliente pasa. Este valor puede ayudar a poner en correlación las operaciones de cliente con las operaciones de servidor y es útil al realizar el seguimiento de los registros que se distribuyen entre servicios. |
| Identidad | Identidad del token que se ha presentado al realizar la solicitud. Puede ser una cuenta de usuario, una cuenta del sistema o una entidad de servicio. |
| Nivel | Proporciona el tipo de mensaje. Para la auditoría, siempre es *informativo*. |
| Ubicación | Indica la ubicación de la actividad de inicio de sesión. |
| Properties (Propiedades) | Enumera todas las propiedades asociadas con los inicios de sesión. Para más información, consulte [Referencia de Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). Este esquema usa los mismos nombres de atributo que el recurso de inicio de sesión, para fines de legibilidad.

## <a name="next-steps"></a>Pasos siguientes

* [Interpretación del esquema de registros de auditoría en Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Más información sobre los registros de diagnóstico de Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)
