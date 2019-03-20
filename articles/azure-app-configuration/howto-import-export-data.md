---
title: Importar o exportar datos con la configuración de la aplicación de Azure | Microsoft Docs
description: Obtenga información sobre cómo importar o exportar datos a o desde la configuración de la aplicación de Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 846472e00bc048de906ee8e14f6de38e366f3571
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225236"
---
# <a name="import-or-export-configuration-data"></a>Importación o exportación de datos de configuración

Azure admite datos de configuración de la aplicación importación y exportación las operaciones. Use estas operaciones para trabajar con datos de configuración en los datos de forma masiva y exchange entre el almacén de configuración de aplicación y el código de proyecto. Por ejemplo, puede configurar el almacén de configuración de una aplicación para las pruebas y otro para producción. A continuación, puede copiar entre ellos a través de un archivo de configuración de la aplicación para que no tendrá que escribir datos dos veces.

En este artículo se proporciona a una guía para importar y exportar datos con la configuración de la aplicación.

## <a name="import-data"></a>Importar datos

Importación aporta la configuración de almacén de datos en una configuración de la aplicación desde un origen existente, en lugar de escribirla manualmente. Use la función de importación para migrar datos a un almacén de configuración de aplicación o datos agregados de varios orígenes. Configuración de la aplicación admite la importación desde un archivo JSON, YAML o propiedades.

Importar datos mediante el uso del [portal de Azure](https://aka.ms/azconfig/portal) o [CLI de Azure](./scripts/cli-import.md). Desde el portal de Azure, siga estos pasos:

1. Vaya a su almacén de configuración de aplicación y seleccione **Import/Export**.

2. En el **importación** ficha, seleccione **el servicio de origen** > **archivo de configuración**.

3. Seleccione **idioma** > **tipo de archivo**.

4. Seleccione el **carpeta** icono y busque el archivo para importar.

    ![Archivo de importación](./media/import-file.png)

5. Seleccione un **separador**y, opcionalmente, escriba un **prefijo** que se usará para los nombres de clave importados.

6. Si lo desea, seleccione un **etiqueta**.

7. Seleccione **aplicar** para finalizar la importación.

    ![Archivo de importación finalizada](./media/import-file-complete.png)

## <a name="export-data"></a>Exportar datos

Export escribe los datos de configuración almacenados en la configuración de la aplicación a otro destino. Utilice la función de exportación, por ejemplo, para guardar los datos en un almacén de configuración de aplicación a un archivo que está integrado en el código de aplicación durante la implementación.

Exportar datos mediante el uso del [portal de Azure](https://aka.ms/azconfig/portal) o [CLI de Azure](./scripts/cli-export.md). Desde el portal de Azure, siga estos pasos:

1. Vaya a su almacén de configuración de aplicación y seleccione **Import/Export**.

2. En el **exportar** ficha, seleccione **servicio de destino** > **archivo de configuración**.

3. Opcionalmente, escriba un **prefijo** y seleccione un **etiqueta** y un punto en el tiempo para las claves que se exportarán.

4. Seleccione un **tipo de archivo** > **separador**.

5. Seleccione **aplicar** para finalizar la exportación.

    ![Archivo de exportación finalizada](./media/export-file-complete.png)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de una aplicación web de ASP.NET](./quickstart-aspnet-core-app.md)  
