---
title: Importación o exportación de datos con Azure App Configuration | Microsoft Docs
description: Obtenga información sobre cómo importar o exportar datos a o desde Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 64fcc8396fc1b771d0095ee595fd177d7fe99b58
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899513"
---
# <a name="import-or-export-configuration-data"></a>Importación o exportación de datos de configuración

Azure App Configuration admite las operaciones de importación y exportación. Use estas operaciones para trabajar con datos de configuración de forma masiva e intercambiar datos entre el almacén de App Configuration y el proyecto de código. Por ejemplo, puede configurar un almacén de App Configuration para pruebas y otro para producción. A continuación, puede copiar la configuración de la aplicación de uno a otro a través de un archivo para que no tenga que escribir los datos dos veces.

En este artículo se proporciona una guía para importar y exportar los datos con App Configuration.

## <a name="import-data"></a>Importar datos

La importación trae los datos de configuración de un almacén de App Configuration desde un origen existente, en lugar de tener que escribirlos manualmente. Use la función de importación para migrar los datos a un almacén de App Configuration o para agregar datos de varios orígenes. App Configuration admite la importación desde un archivo JSON, YAML o de propiedades.

Importe los datos con [Azure Portal](https://portal.azure.com) o con la [CLI de Azure](./scripts/cli-import.md). En Azure Portal, haga lo siguiente:

1. Vaya a su almacén de App Configuration y seleccione **Importar/Exportar**.

2. En la pestaña **Importar**, seleccione **Servicio de origen** > **Archivo de configuración**.

3. Seleccione **Para el idioma** > **Tipo de archivo**.

4. Seleccione el icono **Carpeta** y busque el archivo para importar.

    ![Importación del archivo](./media/import-file.png)

5. Seleccione un **Separador** y, si lo desea, escriba un **Prefijo** para usarlo con los nombres de clave importados.

6. Si lo desea, seleccione una **Etiqueta**.

7. Seleccione **Aplicar** para finalizar la importación.

    ![Importación de archivo finalizada](./media/import-file-complete.png)

## <a name="export-data"></a>Exportar datos

La exportación escribe los datos de configuración almacenados en App Configuration a otro destino. Utilice la función de exportación, por ejemplo, para guardar los datos de un almacén de App Configuration en un archivo que está insertado en el código de aplicación durante la implementación.

Para exportar los datos, use [Azure Portal](https://portal.azure.com) o la [CLI de Azure](./scripts/cli-export.md). En Azure Portal, haga lo siguiente:

1. Vaya a su almacén de App Configuration y seleccione **Importar/Exportar**.

2. En la pestaña **Exportar**, seleccione **Servicio de destino** > **Archivo de configuración**.

3. Si lo desea, escriba un **Prefijo** y seleccione una **Etiqueta** y un momento dado para las claves que se exportarán.

4. Seleccione un **Tipo de archivo** > **Separador**.

5. Seleccione **Aplicar** para finalizar la exportación.

    ![Exportación de archivo finalizada](./media/export-file-complete.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cree una aplicación web ASP.NET Core](./quickstart-aspnet-core-app.md)  
