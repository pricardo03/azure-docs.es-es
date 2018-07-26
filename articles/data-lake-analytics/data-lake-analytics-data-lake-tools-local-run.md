---
title: Ejecución de scripts U-SQL de Azure Data Lake en la máquina local | Microsoft Docs
description: Aprenda a usar las Herramientas de Azure Data Lake para Visual Studio para ejecutar trabajos de U-SQL en la máquina local.
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
ms.openlocfilehash: a7f43c7e17f36d9b4e0767744eee9604c2628ea8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888973"
---
# <a name="run-u-sql-script-on-your-local-machine"></a>Ejecución de scripts U-SQL en la máquina local

Al desarrollar scripts U-SQL, es habitual ejecutar el script U-SQL localmente, ya que ahorra tiempo y costos. Las Herramientas de Azure Data Lake para Visual Studio permiten ejecutar scripts U-SQL en la máquina local. 

## <a name="basic-concepts-for-local-run"></a>Conceptos básicos de la ejecución local

El gráfico siguiente muestra los componentes para la ejecución local y cómo se corresponden estos componentes con la ejecución en la nube.

|Componente|Ejecución local|Ejecución en la nube|
|---------|---------|---------|
|Storage|Carpeta raíz de datos local|Cuenta predeterminada de Azure Data Lake Store|
|Compute|Motor de ejecución local de U-SQL|Servicio Azure Data Lake Analytics|
|Entorno de ejecución|Directorio de trabajo en la máquina local|Clúster de Azure Data Lake Analytics|

Más información sobre los componentes de ejecución local:

### <a name="local-data-root-folder"></a>Carpeta raíz de datos local

La carpeta raíz de datos local es un "almacén local" de la cuenta de proceso local. Todas las carpetas del sistema de archivos local de la máquina local pueden ser una carpeta raíz de datos local. Es equivalente a la cuenta de Azure Data Lake Store predeterminada de una cuenta de Data Lake Analytics. Cambiar a una carpeta raíz de datos diferente es como cambiar a otra cuenta de almacén predeterminada. 

La carpeta raíz de datos se utiliza para:
- Almacenar metadatos, como bases de datos, tablas, funciones con valores de tabla y ensamblados.
- Buscar las rutas de acceso de entrada y salida que se definen como rutas de acceso relativas en el script U-SQL. El uso de rutas de acceso relativas facilita la implementación de sus scripts U-SQL en Azure.

### <a name="u-sql-local-run-engine"></a>Motor de ejecución local de U-SQL

El motor de ejecución local de U-SQL es una "cuenta de proceso local" para trabajos de U-SQL. Los usuarios pueden ejecutar trabajos de U-SQL localmente mediante las Herramientas de Azure Data Lake para Visual Studio. También se admite la ejecución local mediante línea de comandos e interfaces de programación del SDK de U-SQL de Azure Data Lake. [Obtenga más información sobre el SDK de U-SQL para Azure Data Lake](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directory"></a>Directorio de trabajo

Al ejecutar un script U-SQL, se necesita una carpeta del directorio de trabajo para almacenar en caché los resultados de la compilación, los registros de ejecución, etc. En las Herramientas de Azure Data Lake para Visual Studio, el directorio de trabajo es el directorio de trabajo del proyecto de U-SQL (que normalmente se encuentra en `<U-SQL project root path>/bin/debug>`). El directorio de trabajo se limpiará cada vez que se desencadene una nueva ejecución.

## <a name="local-run-in-visual-studio"></a>Ejecución local en Visual Studio

Las Herramientas de Azure Data Lake para Visual Studio tienen un motor de ejecución local integrado que muestra como una cuenta de proceso local. Para ejecutar un script U-SQL localmente, seleccione una cuenta de máquina local (Local-machine) o de proyecto local (Local-project) en el menú desplegable del margen del editor del script y haga clic en **Enviar**.

![Envío de scripts a una cuenta local con Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-run-with-local-machine-account"></a>Ejecución local con la cuenta (Local-machine)

La cuenta de (Local-machine) es una cuenta de proceso local compartida con una sola carpeta raíz de datos local como cuenta de almacén local. La carpeta raíz de datos se encuentra de manera predeterminada en “C:\Users\<nombre de usuario>\AppData\Local\USQLDataRoot” y también se puede configurar mediante **Herramientas > Data Lake > Options and Settings** (Opciones y configuración).

![Configuración de raíz de datos local de Herramientas de Data Lake para Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Para una ejecución local se necesita un proyecto de U-SQL. El directorio de trabajo del proyecto de U-SQL se usa para el directorio de trabajo de ejecución local de U-SQL. Los resultados de la compilación, los registros de ejecución y otros archivos relacionados con la ejecución del trabajo se generan y almacenan en la carpeta del directorio de trabajo durante la ejecución local. Tenga en cuenta que cada vez que vuelve a ejecutar el script, todos estos archivos del directorio de trabajo se limpian y vuelven a generar.

## <a name="local-run-with-local-project-account"></a>Ejecución local con la cuenta (Local-project)

La cuenta (Local-project) es una cuenta de proceso local aislada del proyecto para cada proyecto con carpeta raíz de datos local aislada. Cada proyecto de U-SQL activo que se abre en el Explorador de soluciones tiene una cuenta `(Local-project: <project name>)` correspondiente que aparece en el Explorador de servidores y en el margen del editor del script U-SQL. 

La cuenta (Local-project) proporciona un entorno de desarrollo limpio y aislado para desarrolladores. A diferencia de la cuenta (Local-machine), que tiene una carpeta raíz de datos local compartida que almacena metadatos y datos de entrada y salida para todos los trabajos locales, la cuenta (Local-project) crea una carpeta raíz de datos local temporal en el directorio de trabajo del proyecto de U-SQL cada vez que se ejecuta un script de U-SQL. Esta carpeta raíz de datos temporal se limpia con cada recompilación o nueva ejecución. 

El proyecto de U-SQL proporciona una buena experiencia para administrar este entorno de ejecución local aislado mediante una propiedad y una referencia de proyecto. Puede configurar tanto los orígenes de datos de entrada para los scripts de U-SQL del proyecto como los entornos de bases de datos referenciadas.

### <a name="manage-input-data-source-for-local-project-account"></a>Administración de origen de datos de entrada para la cuenta (Local-project)

El proyecto de U-SQL se encarga de la creación de la carpeta raíz de datos local y la configuración de datos para la cuenta (Local-project). Una carpeta raíz de datos temporal se limpia y vuelve a crear en el directorio de trabajo del proyecto de U-SQL con cada recompilación y ejecución local. Todos los orígenes de datos configurados por el proyecto de U-SQL se copian en esta carpeta raíz de datos local temporal antes de la ejecución de trabajos locales. 

Puede configurar la carpeta raíz de los orígenes de datos si **hace clic con el botón derecho en el proyecto de U-SQL y selecciona Propiedad > Probar origen de datos**. Cuando se ejecuta el script U-SQL en la cuenta (Local-project), todos los archivos y subcarpetas (incluidos los archivos de las subcarpetas) de la carpeta **Probar origen de datos** se copian en la carpeta raíz de datos local temporal. Una vez finalizada la ejecución de trabajos locales, los resultados también pueden encontrarse en la carpeta raíz de datos local temporal del directorio de trabajo del proyecto. Tenga en cuenta que todos estos resultados se eliminarán y limpiarán cuando el proyecto se recompile y limpie. 

![Las Herramientas de Data Lake para Visual Studio configuran el origen de datos de prueba del proyecto](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-referenced-database-environment-for-local-project-account"></a>Administración del entorno de bases de datos referenciadas para la cuenta (Local-project) 

Si una consulta U-SQL utiliza o consulta objetos de base de datos de U-SQL, los entornos de bases de datos deben estar listos localmente antes de ejecutar este script U-SQL localmente. Para la cuenta (Local-project), las dependencias de la base de datos de U-SQL pueden administrarse mediante las referencias de proyecto de U-SQL. Puede agregar referencias de proyecto de base de datos de U-SQL al proyecto de U-SQL. Antes de ejecutar scripts U-SQL en la cuenta (Local-project), se implementan todas las bases de datos referenciadas en la carpeta raíz de datos local temporal. Con cada ejecución, se limpia la carpeta raíz de datos temporal como un entorno aislado nuevo.

Artículos relacionados:
* [Más información sobre cómo administrar la definición de base de datos de U-SQL mediante el proyecto de base de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)
* [Más información sobre cómo administrar la referencia de base de datos de U-SQL en el proyecto de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)

## <a name="difference-between-local-machine-and-local-project-account"></a>Diferencias entre la cuenta (Local-machine) y la cuenta (Local-project)

La cuenta (Local-machine) tiene por objeto simular una cuenta de Azure Data Lake Analytics en la máquina local de los usuarios. Comparte la misma experiencia que una cuenta de Azure Data Lake Analytics. El objetivo de la cuenta (Local-project) es proporcionar un entorno de desarrollo local fácil de usar que ayude a los usuarios a implementar referencias de bases de datos y datos de entrada antes de ejecutar el script localmente. La cuenta (Local-machine) proporciona un entorno compartido permanente, accesible a través de todos los proyectos. La cuenta (Local-project) proporciona un entorno de desarrollo aislado para cada proyecto y se actualiza con cada ejecución. En función de lo anterior, la cuenta (Local-project) ofrece una experiencia de desarrollo más rápida al aplicar nuevos cambios rápidamente.

En el gráfico, se indican otras diferencias entre las cuentas (Local-machine) y (Local-project):

|Área de diferencia|(Local-machine)|(Local-project)|
|----------------|---------------|---------------|
|Acceso local|Accesible para todos los proyectos|Solo el proyecto correspondiente puede acceder a esta cuenta|
|Carpeta raíz de datos local|Carpeta local permanente. Configurada mediante **Herramientas > Data Lake > Options and Settings** (Opciones y configuración).|Carpeta temporal creada para cada ejecución local en el directorio de trabajo del proyecto de U-SQL. La carpeta se limpia con cada recompilación o nueva ejecución.|
|Datos de entrada para el script U-SQL|Ruta de acceso relativa en la carpeta raíz de datos local permanente|Definida mediante la **propiedad del proyecto de U-SQL > Probar origen de datos**. Se copian todos los archivos y subcarpetas en la carpeta raíz de datos temporal antes de la ejecución local.|
|Datos de salida para el script U-SQL|Ruta de acceso relativa en la carpeta raíz de datos local permanente|Enviados a la carpeta raíz de datos temporal. Los resultados se limpian con cada recompilación o nueva ejecución.|
|Implementación de bases de datos referenciadas|Las bases de datos referenciadas no se implementan automáticamente al ejecutarse en la cuenta (Local-machine). Lo mismo ocurre al enviarse a la cuenta de Azure Data Lake Analytics.|Las bases de datos referenciadas se implementan en la cuenta (Local-project) automáticamente antes de la ejecución local. Todos los entornos de base de datos se limpian y vuelven a implementar con cada recompilación o nueva ejecución.|

## <a name="local-run-with-u-sql-sdk"></a>Ejecución local con el SDK de U-SQL

Además de ejecutar scripts U-SQL localmente en Visual Studio, puede usar el SDK de U-SQL para Azure Data Lake para ejecutar scripts U-SQL localmente con la línea de comandos e interfaces de programación. Mediante estas interfaces, puede automatizar la prueba y ejecución local de U-SQL.

[Más información del SDK de U-SQL para Azure Data Lake](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Pasos siguientes

- [SDK de U-SQL para Azure Data Lake](data-lake-analytics-u-sql-sdk.md)
- [Configuración de canalizaciones de CI/CD para Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Uso de un proyecto de base de datos de U-SQL para desarrollar bases de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)
- [Prueba de código de Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
