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
ms.openlocfilehash: 851a8705b3cfa9c88e369af7b961a653dee8fd7a
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959857"
---
# <a name="import-or-export-configuration-data"></a>Importación o exportación de datos de configuración

Azure admite datos de configuración de la aplicación importación y exportación las operaciones. Estos permiten trabajar con datos de configuración de forma masiva como así como intercambiar datos entre la configuración de aplicaciones, almacenar y proyecto de código. Por ejemplo, puede configurar el almacén de configuración de una aplicación para las pruebas y otro para la configuración de aplicación de producción y copia entre ellos a través de un archivo para que no es necesario escribir los datos dos veces.

En este artículo se proporciona a una guía para importar y exportar datos con la configuración de la aplicación.

## <a name="import-data"></a>Importar datos

Importación ofrece configuración de almacén de datos en la configuración de la aplicación desde un origen existente, en lugar de especificarse manualmente. Puede usar la función de importación para migrar datos a un almacén de configuración de aplicación o datos agregados de varios orígenes. Configuración de la aplicación admite la importación desde un archivo JSON, YAML o propiedades.

Puede importar datos mediante el [portal Azure](https://aka.ms/azconfig/portal) o [CLI de Azure](./scripts/cli-import.md). Desde el portal de Azure, siga estos pasos:

1. Vaya a su almacén de configuración de aplicación y haga clic en **Import/Export**.

2. En el **importación** ficha, elija **el servicio de origen** y **archivo de configuración**.

3. Elija **idioma** y **tipo de archivo**.

4. Haga clic en el **carpeta** icono y busque el archivo para importar.

    ![Archivo de importación](./media/import-file.png)

5. Elija un **separador** y, opcionalmente, escriba un **prefijo** que se usará para los nombres de clave importados.

6. Seleccionar opcionalmente un **etiqueta**.

7. Haga clic en **aplicar** para completar la importación.

    ![Archivo de importación completa](./media/import-file-complete.png)

## <a name="export-data"></a>Exportar datos

Export escribe los datos de configuración almacenados en la configuración de la aplicación a otro destino. Puede usar la función de exportación, por ejemplo, para guardar los datos en un almacén de configuración de aplicación a un archivo que se incrustarán con el código de aplicación durante la implementación.

Puede exportar datos mediante el [portal Azure](https://aka.ms/azconfig/portal) o [CLI de Azure](./scripts/cli-export.md). Desde el portal de Azure, siga estos pasos:

1. Vaya a su almacén de configuración de aplicación y haga clic en **Import/Export**.

2. En el **exportar** ficha, elija **servicio de destino** y **archivo de configuración**.

3. Opcionalmente, escriba un **prefijo** y elija un **etiqueta** y un punto en el tiempo para las claves que se exportarán.

4. Elija un **tipo de archivo** y **separador**.

5. Haga clic en **aplicar** para completar la exportación.

    ![Archivo de exportación completa](./media/export-file-complete.png)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de una aplicación web de ASP.NET](./quickstart-aspnet-core-app.md)  
