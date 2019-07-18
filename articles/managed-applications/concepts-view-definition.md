---
title: Introducción a la definición de vistas en Azure Managed Applications | Microsoft Docs
description: Se describe el concepto de creación de una definición de vistas para Azure Managed Applications.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: 6735787f9b43f98ab611584f3c7191c9f927dbc2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479057"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Artefacto de definición de vistas en Azure Managed Applications

La definición de vistas es un artefacto opcional en Azure Managed Applications. Permite personalizar la página de introducción y agregar más vistas como métricas y recursos personalizados.

En este artículo se proporciona una introducción al artefacto de definición de vistas y sus capacidades.

## <a name="view-definition-artifact"></a>Ver definición de artefacto

El artefacto de definición de vistas debe denominarse **viewDefinition.json** y debe colocarse en el mismo nivel que **createUiDefinition.json** y **mainTemplate.json** en el paquete .zip que crea la definición de una aplicación administrada. Para obtener información sobre cómo crear el paquete .zip y publicar la definición de una aplicación administrada, consulte [Publicación de la definición de una aplicación administrada de Azure](publish-managed-app-definition-quickstart.md).

## <a name="view-definition-schema"></a>Esquema de la definición de vistas

El archivo **viewDefinition.json** solo tiene una propiedad `views` de nivel superior, que es una matriz de vistas. Cada vista se muestra en la interfaz de usuario de la aplicación administrada como un elemento de menú independiente en la tabla de contenidos. Cada vista tiene una propiedad `kind` que establece el tipo de vista. Debe establecerse en uno de los siguientes valores: [Introducción](#overview), [Métricas](#metrics), [Recursos personalizados](#custom-resources). Para obtener más información, consulte el [esquema JSON actual para viewDefinition.json](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

Ejemplo de JSON para la definición de vistas:

```json
{
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Test Action",
                        "path": "testAction"
                    },
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { },
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        }
    ]
}

```

## <a name="overview"></a>Información general

`"kind": "Overview"`

Cuando proporciona esta vista en **viewDefinition.json**, esta reemplaza la página de introducción predeterminada en la aplicación administrada.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Propiedad|Obligatorio|DESCRIPCIÓN|
|---------|---------|---------|
|encabezado|Sin|El encabezado de la página de introducción.|
|description|Sin|La descripción de la aplicación administrada.|
|comandos|Sin|La matriz de botones adicionales de la barra de herramientas de la página de introducción; consulte [comandos](#commands).|

## <a name="metrics"></a>Métricas

`"kind": "Metrics"`

La vista de métricas le permite recopilar y agregar datos de los recursos de la aplicación administrada a las [métricas de Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Propiedad|Obligatorio|DESCRIPCIÓN|
|---------|---------|---------|
|DisplayName|Sin|El título de visualización de la vista.|
|version|Sin|La versión de la plataforma utilizada para representar la vista.|
|gráficos|Sí|La matriz de los gráficos de la página de métricas.|

### <a name="chart"></a>Gráfico

|Propiedad|Obligatorio|DESCRIPCIÓN|
|---------|---------|---------|
|DisplayName|Sí|El título de visualización del gráfico.|
|chartType|Sin|La visualización que se utilizará para este gráfico. De forma predeterminada, se utiliza un gráfico de líneas. Tipos de gráficos compatibles: `Bar, Line, Area, Scatter`.|
|Métricas|Sí|La matriz de métricas que se trazarán en el gráfico. Para obtener más información acerca de las métricas compatibles con Azure Portal, consulte [Métricas compatibles con Azure Monitor](../azure-monitor/platform/metrics-supported.md).|

### <a name="metric"></a>Métrica

|Propiedad|Obligatorio|DESCRIPCIÓN|
|---------|---------|---------|
|Nombre|Sí|El nombre de la métrica.|
|AggregationType|Sí|El tipo de agregación que se usará para esta métrica. Tipos de agregación compatibles: `none, sum, min, max, avg, unique, percentile, count`.|
|espacio de nombres|Sin|Información adicional que se usará al determinar el proveedor de métricas adecuado.|
|resourceTagFilter|Sin|La matriz de etiquetas del recurso (se separarán con la palabra `or`) para las que se mostrarán métricas. Se aplica encima del filtro de tipo de recurso.|
|resourceType|Sí|El tipo de recurso para el que se mostrarán métricas.|

## <a name="custom-resources"></a>Recursos personalizados

`"kind": "CustomResources"`

Puede definir varias vistas de este tipo. Cada vista representa un **único** tipo de recurso personalizado del proveedor personalizado que ha definido en **mainTemplate.json**. Para consultar una introducción a los proveedores de recursos, vea la [introducción a los proveedores personalizados de Azure en versión preliminar](custom-providers-overview.md).

En esta vista, puede realizar operaciones GET, PUT, DELETE y POST para el tipo de recurso personalizado. Las operaciones POST podrían ser acciones personalizadas globales o acciones personalizadas en un contexto del tipo de recurso personalizado.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Test Action",
                "path": "testAction"
            },
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Propiedad|Obligatorio|DESCRIPCIÓN|
|---------|---------|---------|
|DisplayName|Sí|El título de visualización de la vista. El título debe ser **único** para cada vista CustomResources en su **viewDefinition.json**.|
|version|Sin|La versión de la plataforma utilizada para representar la vista.|
|resourceType|Sí|El tipo de recurso personalizado. Debe ser un tipo de recurso personalizado **único** del proveedor personalizado.|
|createUIDefinition|Sin|Esquema de creación de definiciones de interfaz de usuario para crear el comando de recurso personalizado. Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).|
|comandos|Sin|La matriz de botones adicionales de la barra de herramientas de la vista CustomResources; consulte [comandos](#commands).|
|columnas|Sin|La matriz de columnas del recurso personalizado. Si no se ha definido, se mostrará la columna `name` de forma predeterminada. La columna debe tener `"key"` y `"displayName"`. Para la clave, proporcione la clave de la propiedad que se va a mostrar en una vista. Si es anidada, utilice el punto como delimitador, por ejemplo, `"key": "name"` o `"key": "properties.property1"`. Como nombre para mostrar, proporcione el nombre de la propiedad que se mostrará en una vista. También puede proporcionar una propiedad `"optional"`. Si se establece en true, la columna se oculta en una vista de forma predeterminada.|

## <a name="commands"></a>Comandos:

Los comandos son una matriz de botones adicionales de la barra de herramientas que se muestran en la página. Cada comando representa una acción POST del proveedor personalizado de Azure definida en **mainTemplate.json**. Para ver una introducción a los proveedores personalizados, consulte la [información general sobre proveedores personalizados de Azure](custom-providers-overview.md).

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Propiedad|Obligatorio|DESCRIPCIÓN|
|---------|---------|---------|
|DisplayName|Sí|El nombre para mostrar del botón de comando.|
|path|Sí|El nombre de la acción del proveedor personalizado. La acción debe estar definida en **mainTemplate.json**.|
|icon|Sin|El icono del botón de comando. La lista de iconos compatibles se define en el [esquema JSON](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).|
|createUIDefinition|Sin|Esquema de creación de definiciones de interfaz de usuario del comando. Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).|

## <a name="next-steps"></a>Pasos siguientes

- Para una introducción a las aplicaciones administradas, consulte la [introducción a las aplicaciones administradas de Azure](overview.md).
- Para ver una introducción a los proveedores personalizados, consulte la [información general sobre proveedores personalizados de Azure](custom-providers-overview.md).
