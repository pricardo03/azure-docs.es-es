---
title: Referencia para ver un artefacto de definición de una aplicación administrada de Azure
description: Proporciona un ejemplo de artefacto de definición de vista para Azure Managed Applications. El nombre de archivo es viewDefinition.json.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: 1f56f6c9c519bd29423d92a8dc8b8ce5904b523c
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332631"
---
# <a name="reference-view-definition-artifact"></a>Referencia: Ver definición de artefacto

Este artículo es una referencia para un artefacto *viewDefinition.json* en Azure Managed Applications. Para más información sobre cómo crear configuraciones de vistas, consulte [Ver artefacto de definición](concepts-view-definition.md).

## <a name="view-definition"></a>Ver definición

El JSON siguiente muestra un ejemplo de un archivo *viewDefinition.json* para Azure Managed Applications:

```json
{
  "views": [{
    "kind": "Overview",
    "properties": {
      "header": "Welcome to your Demo Azure Managed Application",
      "description": "This Managed application with Custom Provider is for demo purposes only.",
      "commands": [{
          "displayName": "Ping Action",
          "path": "/customping",
          "icon": "LaunchCurrent"
      }]
    }
  },
  {
    "kind": "CustomResources",
    "properties": {
      "displayName": "Users",
      "version": "1.0.0.0",
      "resourceType": "users",
      "createUIDefinition": {
        "parameters": {
          "steps": [{
            "name": "add",
            "label": "Add user",
            "elements": [{
              "name": "name",
              "label": "User's Full Name",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a full user name.",
              "constraints": { "required": true }
            },
            {
              "name": "location",
              "label": "User's Location",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a Location.",
              "constraints": { "required": true }
            }]
          }],
          "outputs": {
            "name": "[steps('add').name]",
            "properties": {
              "FullName": "[steps('add').name]",
              "Location": "[steps('add').location]"
            }
          }
        }
      },
      "commands": [{
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
      }],
      "columns": [
        { "key": "properties.FullName", "displayName": "Full Name" },
        { "key": "properties.Location", "displayName": "Location", "optional": true }
      ]
    }
  }]
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación de una aplicación administrada con acciones y recursos personalizados](tutorial-create-managed-app-with-custom-provider.md).
- [Referencia: Artefactos de elementos de la interfaz de usuario](reference-createuidefinition-artifact.md).
- [Referencia: Artefacto de plantilla de implementación](reference-main-template-artifact.md).