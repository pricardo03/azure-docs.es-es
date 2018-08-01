---
title: Interpretación del esquema de registro de auditoría de Azure Active Directory en Azure Monitor (versión preliminar) | Microsoft Docs
description: Se describe el esquema de registro de auditoría de Azure AD para su uso en Azure Monitor (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: e1ae8e2a4dc9ef9c21300ebfc4df8c0f1c5819f2
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240595"
---
# <a name="interpret-the-azure-active-directory-audit-logs-schema-in-azure-monitor-preview"></a>Interpretación del esquema de registro de auditoría de Azure Active Directory en Azure Monitor (versión preliminar)

En este artículo se describe el esquema de registro de auditoría de Azure AD en Azure Monitor. Cada entrada de registro individual se almacena como texto, con formato de blob JSON, como se muestra en los dos ejemplos siguientes. 

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-17T00:14:31.2585575Z", 
        "operationName": "Change password (self-service)",
        "operationVersion": "1.0",
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "resultDescription": "None", 
        "durationMs": "-1", 
        "correlationId": "60d5e89a-b890-413f-9e25-a047734afe9f", 
        "identity": "sreens@wingtiptoysonline.com", 
        "Level": "Informational", 
        "location": "WUS", 
        "properties": { 
            "identityType": "UPN", 
            "operationType": "Update", 
            "additionalDetails": "None", 
            "additionalTargets": "", 
            "targetUpdatedProperties": "", 
            "targetResourceType": "UPN__TenantContextID__PUID__ObjectID__ObjectClass", 
            "targetResourceName": "sreens@wingtiptoysonline.com__bf85dc9d-cb43-44a4-80c4-469e8c58249e__1003BFFD9FEB17DB__7a408bdd-7d97-4574-8511-dd747b56465d__User", 
            "auditEventCategory": "UserManagement" 
        } 
    } 
    ] 
} 
```

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-18T19:47:43.0368859Z", 
        "operationName": "Update service principal.", 
        "operationVersion": "1.0", 
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "durationMs": "-1", 
        "callerIpAddress": "<null>", 
        "correlationId": "14916c7a-5a7d-44e8-9b06-74b49efb08ee", 
        "identity": "NA", 
        "Level": "Informational", 
        "properties": { 
            "identityType": "NA", 
            "operationType": "Update", 
            "additionalDetails": {}, 
            "additionalTargets": "", 
            "targetUpdatedProperties": [ 
            { 
                "Name": "Included Updated Properties", 
                "OldValue": null, 
                "NewValue": "" 
            }, 
            { 
                "Name": "TargetId.ServicePrincipalNames", 
                "OldValue": null, 
                "NewValue": "http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29" 
            } 
            ], 
        "targetResourceType": "Other__ObjectID__ObjectClass__Name__AppId__SPN", 
        "targetResourceName": "ServicePrincipal_ea70a262-4da3-440a-b396-9734ddfd9df2__ea70a262-4da3-440a-b396-9734ddfd9df2__ServicePrincipal__Salesforce__cd3ed3de-93ee-400b-8b19-b61ef44a0f29__http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29", 
        "auditEventCategory": "ApplicationManagement" 
      } 
    } 
    ] 
} 
```

| Nombre del campo | DESCRIPCIÓN |
|------------|-------------|
| Twitter en tiempo       | Fecha y hora (UTC) |
| operationName | Nombre de la operación |
| operationVersion | Versión de API de REST solicitada por el cliente. |
| categoría | Actualmente, *auditoría* es el único valor admitido |
| tenantId | GUID de inquilino asociado con los registros. |
| resultType | El resultado de la operación puede ser *Correcto* o *Error*. |
| resultSignature |  Esto no se ha asignado, y puede ignorar este campo de forma segura. | 
| resultDescription | Descripción adicional del resultado, cuando esté disponible. | 
| durationMs |  Esto no se ha asignado, y puede ignorar este campo de forma segura. |
| callerIpAddress | Dirección IP del cliente que realizó la solicitud. | 
| correlationId | GUID opcional que el cliente pasa. Esto puede ayudar a poner en correlación las operaciones de cliente con las operaciones de servidor y es útil al realizar el seguimiento de los registros que se distribuyen entre servicios. |
| identidad | Identidad del token que se ha presentado al realizar la solicitud. Puede ser una cuenta de usuario, una cuenta del sistema o una entidad de servicio. |
| level | Tipo de mensaje. Para los registros de auditoría, siempre es *informativo*. |
| location | Ubicación del centro de datos |
| propiedades | Enumera las propiedades admitidas relacionadas con un registro de auditoría. Para más información, consulte la tabla siguiente. | 


| Nombre de propiedad | DESCRIPCIÓN |
|---------------|-------------|
| AuditEventCategory | Tipo de evento de auditoría. Puede ser *Administración de usuarios*, *Administración de aplicaciones*, etc.|
| Tipo de identidad | *Aplicación* o *usuario* |
| Tipo de operación | Puede ser *Agregar*, *Actualizar*, *Eliminar* u *Otros* |
| Tipo de recurso de destino | Especifica el tipo de recurso de destino en que se realizó la operación. Puede ser *Aplicación*, *Usuario*, *Rol* o *Directiva*. | 
| Nombre de recurso de destino | Nombre del recurso de destino. Por ejemplo, puede ser un nombre de aplicación, un nombre de rol, un nombre principal de usuario o un nombre de entidad de seguridad de servicio. |
| additionalTargets | Se enumeran las propiedades adicionales para operaciones específicas. Por ejemplo, para una operación de actualización, los valores antiguos y los nuevos valores se muestran en *targetUpdatedProperties*. | 

## <a name="next-steps"></a>Pasos siguientes

* [Interpretación de un esquema de registros de inicio de sesión en Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Más información sobre los registros de Diagnósticos de Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Preguntas frecuentes y problemas conocidos](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)