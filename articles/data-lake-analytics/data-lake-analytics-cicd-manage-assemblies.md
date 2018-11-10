---
title: Procedimiento recomendado para administrar ensamblados de U-SQL en la canalización de CI/CD para Azure Data Lake
description: Conozca el procedimiento recomendado para administrar ensamblados de C# de U-SQL en la canalización de CI/CD con Azure DevOps.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670292"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>Procedimiento recomendado para administrar ensamblados de U-SQL en la canalización de CI/CD

En este artículo, aprenderá a administrar código fuente de ensamblados de U-SQL con el nuevo proyecto de base de datos U-SQL incorporado. También aprenderá a configurar una canalización de integración e implementación continua (CI/CD) para el registro de ensamblados con Azure DevOps.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>Usar el proyecto de base de datos U-SQL para administrar el código fuente de ensamblados

[El proyecto de base de datos U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md) es un tipo de proyecto de Visual Studio que ayuda a los desarrolladores a desarrollar, administrar e implementar sus bases de datos U-SQL rápida y fácilmente. Todos los objetos de base de datos U-SQL (excepto las credenciales) se pueden administrar con el proyecto de base de datos U-SQL. 

La manera recomendada de administrar el código fuente de ensamblados de C# y los scripts U-SQL de DDL del registro de ensamblados es:

* Usar el **proyecto de base de datos U-SQL** para administrar scripts U-SQL del registro de ensamblados.
* Usar la **biblioteca de clases (para la aplicación U-SQL)** para administrar el código fuente de C# y las dependencias de los operadores, las funciones y los agregadores definidos por el usuario (UDO/UDF/UDAG).
* Usar el proyecto de base de datos U-SQL para hacer referencia al proyecto de biblioteca de clases. 

Un proyecto de base de datos U-SQL puede hacer referencia a un proyecto de biblioteca de clases (para la aplicación U-SQL). Se pueden crear ensamblados registrados en la base de datos U-SQL mediante el código fuente de C# de referencia desde este proyecto de biblioteca de clases (para la aplicación U-SQL).

Puede seguir los pasos de creación de proyectos y adición de referencias siguientes:
1. Crear un proyecto de biblioteca de clases (para la aplicación U-SQL) a través de **Archivo > Nuevo > Proyecto**. El proyecto se encuentra bajo el nodo **Azure Data Lake > U-SQL**.
   ![Herramientas de Data Lake para Visual Studio: Creación de proyecto de biblioteca de clases de C#](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. Agregar el código de C# definido por el usuario en el proyecto de biblioteca de clases (para la aplicación U-SQL). 
3. Crear un proyecto de U-SQL a través de **Archivo > Nuevo > Proyecto**. El proyecto se encuentra bajo el nodo **Azure Data Lake > U-SQL**.
   ![Herramientas de Data Lake para Visual Studio: Creación de proyecto de base de datos U-SQL](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. Agregar una referencia al proyecto de biblioteca de clases de C# para el proyecto de base de datos U-SQL.

    ![Herramientas de Data Lake para Visual Studio: Adición de referencia de proyecto de base de datos de U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Herramientas de Data Lake para Visual Studio: Adición de referencia de proyecto de base de datos de U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. Crear un script de ensamblado en el proyecto de base de datos U-SQL a través de **clic con el botón derecho en el proyecto > Agregar nuevo elemento**.
   ![Herramientas de Data Lake para Visual Studio: Adición de script de ensamblado](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. Abra el script de ensamblado y, en la vista de diseño del ensamblado, elija el ensamblado de referencia en el menú desplegable **Cree el ensamblado desde la referencia**.

    ![Herramientas de Data Lake para Visual Studio: Creación de un ensamblado a partir de una referencia](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Agregue **Dependencias administradas** y **Archivos adicionales**, si los hay. Al agregar archivos adicionales, la herramienta usa la ruta de acceso relativa para asegurarse de que puede encontrar los ensamblados tanto en la máquina local como en la máquina de compilación más adelante. 

**@_DeployTempDirectory** en la ventana del editor de la parte inferior es una variable predefinida que dirige la herramienta a la carpeta de salida de compilación. En la carpeta de salida de compilación, cada ensamblado tiene una subcarpeta denominada con el nombre del ensamblado. Todos los archivos adicionales y DLL están en esa subcarpeta. 

## <a name="build-a-u-sql-database-project"></a>Compilación de un proyecto de base de datos U-SQL

La salida de compilación del proyecto de base de datos U-SQL es un paquete de implementación de la base de datos U-SQL, denominado con el sufijo `.usqldbpack`. El paquete `.usqldbpack` es un archivo ZIP que incluye todas las instrucciones DDL en un único script U-SQL en la carpeta **DDL** y todos los archivos adicionales y DLL compilados para los ensamblados en la carpeta **Temp**.

## <a name="deploy-a-u-sql-database"></a>Implementación de la base de datos U-SQL

El paquete `.usqldbpack` puede implementarse tanto en una cuenta local como en una cuenta de Azure Data Lake Analytics mediante Visual Studio o el SDK de implementación. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Implementación de una base de datos U-SQL en Visual Studio

Puede implementar una base de datos U-SQL mediante un proyecto de base de datos U-SQL o un paquete `.usqldbpack` en Visual Studio.

#### <a name="deploy-through-a-u-sql-database-project"></a>Implementación mediante un proyecto de base de datos U-SQL

1.  Haga clic con el botón derecho en el proyecto de base de datos U-SQL y, después, seleccione **Implementar**.
2.  En el **Asistente para implementar la base de datos de U-SQL**, seleccione la **cuenta de ADLA** en la que desea implementar la base de datos. Se admite tanto una cuenta local como una cuenta de ADLA.
3.  El **origen de la base de datos** se rellena automáticamente y señala al paquete .usqldbpack en la carpeta de salida de compilación del proyecto.
4.  Escriba un nombre en **Nombre de base de datos** para crear una base de datos. Si ya hay una base de datos con el mismo nombre en la cuenta de Azure Data Lake Analytics de destino, todos los objetos definidos en el proyecto de base de datos se crean sin volver a crear la base de datos.
5.  Para implementar la base de datos de U-SQL, seleccione **Enviar**. Se cargarán todos los recursos (ensamblados y archivos adicionales) y un trabajo de U-SQL que incluya todas las instrucciones DDL enviadas.

    ![Herramientas de Data Lake para Visual Studio: Implementación un proyecto de base de datos U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Herramientas de Data Lake para Visual Studio: Implementación mediante el asistente del proyecto de base de datos de U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>Implementación de la base de datos U-SQL en Azure DevOps

`PackageDeploymentTool.exe` proporciona interfaces de programación y línea de comandos que ayudan a implementar bases de datos de U-SQL. El SDK se incluye en el [paquete Nuget del SDK de U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), que se encuentra en `build/runtime/PackageDeploymentTool.exe`.

En Azure DevOps, puede usar la tarea de línea de comandos y este SDK para configurar la canalización de automatización para la actualización de la base de datos U-SQL. [Más información sobre el SDK y cómo configurar la canalización de CI/CD para la implementación de bases de datos de U-SQL](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Pasos siguientes

* [Procedimiento para configurar una canalización de CI/CD para Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
* [Prueba de código de Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
* [Ejecución local de scripts U-SQL](data-lake-analytics-data-lake-tools-local-run.md)
