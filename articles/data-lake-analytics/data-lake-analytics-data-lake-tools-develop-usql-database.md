---
title: Uso de un proyecto de base de datos de U-SQL para desarrollar una base de datos de U-SQL para Azure Data Lake | Microsoft Docs
description: Aprenda a desarrollar una base de datos de U-SQL mediante Herramientas de Azure Data Lake para Visual Studio.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a8099a8c9bcec8ed5a7bb480dae58d2c077b8fe0
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37890905"
---
# <a name="use-u-sql-database-project-to-develop-u-sql-database-for-azure-data-lake"></a>Uso de un proyecto de base de datos de U-SQL para desarrollar una base de datos de U-SQL para Azure Data Lake

La base de datos de U-SQL proporciona vistas estructuradas de los datos sin estructurar, administra datos estructurados en tablas y proporciona un sistema de catálogo de metadatos general para organizar los datos estructurados y el código personalizado. La base de datos es el concepto que agrupa estos objetos relacionados.

Obtenga más información sobre la [base de datos de U-SQL y el lenguaje de definición de datos (DDL)](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/data-definition-language-ddl-statements-u-sql). 

El proyecto de base de datos de U-SQL es un tipo de proyecto de Visual Studio que ayuda a los desarrolladores a desarrollar, administrar e implementar sus bases de datos de U-SQL rápida y fácilmente.

## <a name="create-a-u-sql-database-project"></a>Creación de un proyecto de base de datos de U-SQL

Las Herramientas de Azure Data Lake para Visual Studio agregaron una nueva plantilla de proyecto llamada proyecto de base de datos de U-SQL en la versión 2.3.3000.0. Para crear un proyecto de U-SQL, vaya a **Archivo > Nuevo > Proyecto**. El proyecto de base de datos de U-SQL puede encontrarse en **Azure Data Lake > U-SQL node** (Nodo U-SQL).

![Creación de proyecto de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-using-database-project"></a>Desarrollo de objetos de bases de datos de U-SQL mediante un proyecto de base de datos

Haga clic con el botón derecho en el proyecto de base de datos U-SQL y, después, haga clic en **Agregar > Nuevo elemento**. Todos los tipos de objetos compatibles pueden encontrarse en el asistente para agregar un nuevo elemento. 

1.  Para un objeto no ensamblado, por ejemplo, una función con valores de tabla, se crea un nuevo script U-SQL después de agregar el nuevo elemento. Puede empezar a desarrollar la instrucción DDL para ese objeto en el editor.
2.  Para un objeto de ensamblado, la herramienta proporciona un editor de interfaz de usuario fácil de usar que le ayuda a registrar el ensamblado e implementar .dll y otros archivos. Siga estos pasos para agregar una definición de objeto de ensamblado al proyecto de base de datos de U-SQL:

1.  Agregue las referencias del proyecto de C# que incluye el UDO/UDAG/UDF para el proyecto de base de datos de U-SQL.

    ![Adición de referencia de proyecto de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Adición de referencia de proyecto de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  En la vista de diseño del ensamblado, seleccione el ensamblado referenciado en el menú desplegable **Cree el ensamblado desde la referencia**.

    ![Creación de ensamblado a partir de referencia para Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Agregue **dependencias administradas** y **archivos adicionales** si los hay. Al agregar archivos adicionales, la herramienta usará la ruta de acceso relativa para asegurarse de que puede encontrar los ensamblados en la máquina local y en la máquina de compilación más adelante. @_DeployTempDirectory es una variable predefinida que dirige la herramienta a la carpeta de salida de compilación. En la salida de la compilación, cada ensamblado tiene una subcarpeta denominada como el nombre del ensamblado, con todos los archivos .dll y archivos adicionales incluidos en dicha subcarpeta. 
 
## <a name="build-u-sql-database-project"></a>Compilación de un proyecto de base de datos de U-SQL

La salida de compilación del proyecto de base de datos de U-SQL es un paquete de implementación de la base de datos de U-SQL, denominado con el sufijo `.usqldbpack`. El paquete `.usqldbpack` es un archivo ZIP que incluye todas las instrucciones DDL en un único script U-SQL en la carpeta **DDL** y todos los archivos DLL y archivos adicionales para los ensamblados en la carpeta **Temp**.

Obtenga más información sobre [cómo compilar el proyecto de base de datos de U-SQL con la línea de comandos de MSBuild y la tarea de compilación de Visual Studio Team Services](data-lake-analytics-cicd-overview.md#build-u-sql-database-project).

## <a name="deploy-u-sql-database"></a>Implementación de la base de datos de U-SQL

El paquete .usqldbpack puede implementarse tanto en una cuenta local como en una cuenta de Azure Data Lake Analytics mediante Visual Studio o el SDK de implementación. 

### <a name="deploy-u-sql-database-in-visual-studio"></a>Implementación de la base de datos de U-SQL en Visual Studio

Puede implementar una base de datos de U-SQL mediante un proyecto de base de datos de U-SQL o un paquete .usqldbpack en Visual Studio.

#### <a name="deploy-through-u-sql-database-project"></a>Implementación mediante un proyecto de base de datos de U-SQL

1.  Haga clic con el botón derecho en el proyecto de base de datos de U-SQL y seleccione **Implementar**.
2.  En el asistente para implementar la base de datos de U-SQL, seleccione la **cuenta de ADLA** en la que desea implementar la base de datos. Se admite tanto una cuenta (Local) como una cuenta de ADLA.
3.  El **origen de la base de datos** se rellena automáticamente señalando al paquete .usqldbpack de la carpeta de salida de compilación del proyecto
4.  Escriba el **nombre de la base de datos** para crear una base de datos. Si ya hay una base de datos con el mismo nombre en la cuenta de Azure Data Lake Analytics de destino, todos los objetos definidos en el proyecto de base de datos se crean sin volver a crear la base de datos.
5.  Haga clic en **Enviar** para implementar la base de datos de U-SQL. Se cargarán todos los recursos (ensamblados y archivos adicionales) y un trabajo de U-SQL incluirá todas las instrucciones DDL enviadas.

    ![Implementación mediante proyecto de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Asistente para la implementación mediante proyecto de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-u-sql-database-deployment-package"></a>Implementación mediante el paquete de implementación de base de datos de U-SQL

1.  Abra **Explorador de servidores** y expanda la **cuenta de Azure Data Lake Analytics** en la que desea implementar la base de datos.
2.  Haga clic con el botón derecho en Bases de datos U-SQL y seleccione **Implementar base de datos**.
3.  Defina **Origen de la base de datos** como la ruta de acceso del paquete de implementación de la base de datos de U-SQL (archivo .usqldbpack).
4.  Escriba el **nombre de la base de datos** para crear una base de datos. Si ya hay una base de datos con el mismo nombre en la cuenta de Azure Data Lake Analytics de destino, todos los objetos definidos en el proyecto de base de datos se crean sin volver a crear la base de datos.

    ![Implementación mediante paquete de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Asistente para la implementación mediante paquete de base de datos de U-SQL de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-using-sdk"></a>Implementación de la base de datos de U-SQL mediante el SDK

`PackageDeploymentTool.exe` proporciona interfaces de programación y línea de comandos que ayudan a implementar bases de datos de U-SQL. El SDK se incluye en el [paquete Nuget del SDK de U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), que se encuentra en `build/runtime/PackageDeploymentTool.exe`.

[Más información sobre el SDK y cómo configurar la canalización de CI/CD para la implementación de bases de datos de U-SQL](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-visual-studio-team-service).

## <a name="reference-a-u-sql-database-project"></a>Referencia de un proyecto de base de datos de U-SQL

Un proyecto de U-SQL puede hacer referencia a un proyecto de base de datos de U-SQL. La referencia afecta a dos cargas de trabajo:

- Compilación del proyecto: los entornos de bases de datos referenciadas se configuran antes de compilar los scripts U-SQL. 
- Ejecución local en cuenta (Local-project): los entornos de bases de datos referenciadas se implementan en la cuenta (Local-project) antes de la ejecución del script U-SQL. [Más información sobre la ejecución local y las diferencias entre las cuentas (Local-machine) y (Local-project)](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-u-sql-database-reference"></a>Cómo agregar una referencia de base de datos de U-SQL

1. Haga clic con el botón derecho en el **Explorador de soluciones** y seleccione **Agregar referencia de base de datos U-SQL...**.

    ![Adición de referencia de proyecto de base de datos de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. Configure una referencia de base de datos desde un proyecto de base de datos de U-SQL en la solución actual o un archivo de paquete de base de datos de U-SQL.
3. Proporcione el nombre de la base de datos.

    ![Asistente para la adición de referencia de proyecto de base de datos de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de canalizaciones de CI/CD para Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Prueba de código de Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
- [Ejecución local de scripts U-SQL](data-lake-analytics-data-lake-tools-local-run.md)
