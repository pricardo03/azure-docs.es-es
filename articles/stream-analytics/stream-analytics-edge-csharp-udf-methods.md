---
title: Desarrollar funciones definidas por el usuario de .NET Standard para trabajos perimetrales de Azure Stream Analytics (versión preliminar)
description: Obtenga información sobre cómo escribir funciones definidas por el usuario c# para trabajos perimetrales de Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 9aa61e95eb808c38646fa9b8cefd4004f5477ee6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974670"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-edge-jobs-preview"></a>Desarrollar funciones definidas por el usuario de .NET Standard para trabajos perimetrales de Azure Stream Analytics (versión preliminar)

Azure Stream Analytics ofrece un lenguaje de consulta similar a SQL para realizar transformaciones y cálculos sobre transmisiones de datos del evento. Hay muchas funciones integradas, pero algunos escenarios complejos requieren flexibilidad adicional. Con las funciones definidas por el usuario (UDF) de .NET Standard, puede invocar sus propias funciones escritas en cualquier lenguaje de .NET Standard (C#, F#, etc.) para ampliar el lenguaje de consulta de Stream Analytics. Las UDF le permiten realizar cálculos matemáticos complejos, importar modelos de aprendizaje automático personalizados mediante ML.NET y usar una lógica personalizada de imputación para datos que faltan. La característica UDF para trabajos perimetrales de Stream Analytics está actualmente en versión preliminar y no debe usarse en las cargas de trabajo de producción.

## <a name="overview"></a>Información general
Las herramientas de Visual Studio para Azure Stream Analytics facilitan la escritura de las UDF, probar localmente los trabajos (incluso sin conexión) y publicar el trabajo de Stream Analytics en Azure. Una vez publicado en Azure, puede implementar el trabajo en dispositivos de IoT mediante IoT Hub.

Hay tres maneras de implementar las UDF:

* Archivos de CodeBehind en un proyecto ASA
* UDF desde un proyecto local
* Un paquete existente desde una cuenta de almacenamiento de Azure

## <a name="package-path"></a>Ruta de acceso de paquete

El formato de cualquier paquete UDF tiene la ruta de acceso `/UserCustomCode/CLR/*`. Las bibliotecas de vínculos dinámicos (DLL) y los recursos se copian en la carpeta `/UserCustomCode/CLR/*`, lo que ayuda a aislar los archivos DLL de usuario del sistema y los archivos DLL de Azure Stream Analytics.

## <a name="supported-types-and-mapping"></a>Asignación y tipos admitidos

|**Tipo de UDF (C#)**  |**Tipo de Azure Stream Analytics**  |
|---------|---------|
|Booleano  |  bigint   |
|int32  |  bigint   |
|int64  |  bigint   |
|float  |  double   |
|double  |  double   |
|string  |  nvarchar(max)   |
|dateTime  |  dateTime   |
|struct  |  IRecord   |
|objeto  |  IRecord   |
|Matriz  |  IArray   |
|dictionary<string, object>  |  IRecord   |

## <a name="codebehind"></a>CodeBehind
Puede escribir funciones definidas por el usuario en el CodeBehind **Script.asql**. Las herramientas de Visual Studio compilarán automáticamente el archivo de código subyacente en un archivo de ensamblado. Los ensamblados se empaquetan como archivos zip y se cargan en su cuenta de almacenamiento al enviar el trabajo en Azure. Puede aprender a escribir un UDF de C# con CodeBehind siguiendo el tutorial de [UDF de C# para los trabajos perimetrales de Stream Analytics](stream-analytics-edge-csharp-udf.md). 

## <a name="local-project"></a>Proyecto local
Las funciones definidas por el usuario pueden escribirse en un ensamblado que se menciona más adelante en una consulta de Azure Stream Analytics. Esta es la opción recomendada para funciones complejas que requieren toda la eficacia de un lenguaje de .NET Standard más allá de su lenguaje de expresiones, como la lógica de procedimientos o la recursividad. También podrían utilizarse UDF de un proyecto local cuando necesite compartir la lógica de la función en varias consultas de Azure Stream Analytics. Agregar UDF a su proyecto local le ofrece la capacidad de depurar y probar sus funciones localmente desde Visual Studio.

Para hacer referencia a un proyecto local:

1. Cree una nueva biblioteca de clases en la solución.
2. Escriba el código en la clase. Recuerde que las clases deben definirse como *pública* y los objetos deben definirse como *público estático*. 
3. Compile el proyecto.
4. Haga referencia a la nueva clase en el proyecto de Azure Stream Analytics.
5. Agregue una nueva función en el proyecto de Azure Stream Analytics.
6. Configure la ruta de acceso de ensamblado en el archivo de configuración del trabajo, `EdgeJobConfig.json`.
7. Vuelva a generar el proyecto de función y el proyecto de Azure Stream Analytics.  

### <a name="example"></a>Ejemplo

En este ejemplo, **UDFTest** es un proyecto de biblioteca de clases de C# y **ASAEdgeUDFDemo** es el proyecto perimetral de Azure Stream Analytics, que hará referencia a **UDFTest**.

![Proyecto de Azure Stream Analytics en IoT Edge en Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. Compile el proyecto de C#, que le permitirá agregar una referencia a la UDF en C# de las consultas de Azure Stream Analytics.
    
   ![Compilar un proyecto de Azure Stream Analytics en IoT Edge en Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. Agregue la referencia al proyecto de C# en el proyecto perimetral de ASA. Haga clic con el botón derecho en el nodo Referencias y elija Agregar referencia.

   ![Agregar una referencia a un proyecto de C# en Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. Elija el nombre del proyecto de C# en la lista. 
    
   ![Elegir el nombre del proyecto de C# en la lista de referencia](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. Debería ver el **UDFTest** listado en **Referencias** en el **Explorador de soluciones**.

   ![Ver la referencia de funciones definidas por el usuario en el Explorador de soluciones perimetral de Azure Stream Analytics](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. Haga clic con el botón derecho en la carpeta **Funciones** carpeta y elija **Nuevo elemento**.

   ![Agregar un nuevo elemento a Funciones en la solución perimetral de Azure Stream Analytics en Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Agregue una función de C# **SquareFunction.json** a su proyecto de Azure Stream Analytics.

   ![Seleccionar una función de C Sharp de la lista de elementos perimetral de Stream Analytics en Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. Haga doble clic en la función en el **Explorador de soluciones** para abrir el cuadro de diálogo de configuración.

   ![Configuración de la función C sharp en Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. En la configuración de la función de C#, elija **Cargar de la referencia de proyecto de ASA** y los nombres de método, la clase y el ensamblado relacionados en la lista desplegable. Para hacer referencia a los métodos, tipos y funciones en la consulta perimetral de Stream Analytics, se deben definir las clases como *pública* y los objetos como *público estático*.

   ![Configuración de la función de Stream Analytics C sharp](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Paquetes existentes

Puede crear las UDF de .NET Standard en cualquier IDE que prefiera e invocarlos desde la consulta de Azure Stream Analytics. En primer lugar, compile el código y todos los archivos DLL del paquete. El formato del paquete tiene la ruta de acceso `/UserCustomCode/CLR/*`. Después, cargue `UserCustomCode.zip` a la raíz del contenedor en su cuenta de almacenamiento de Azure.

Una vez que se hayan cargado los paquetes zip de ensamblado a la cuenta de almacenamiento de Azure, puede usar las funciones de las consultas de Azure Stream Analytics. Todo lo que necesita hacer es incluir la información de almacenamiento en la configuración del trabajo perimetral de Stream Analytics. No se puede probar la función localmente con esta opción porque las herramientas de Visual Studio no descargarán el paquete. Se redistribuye la ruta de acceso del paquete directamente al servicio. 

Para configurar la ruta de acceso de ensamblado en el archivo de configuración de trabajo, “EdgeJobConfig.json”:

Expanda la sección **Configuración de código definido por el usuario** y rellene la configuración con los siguientes valores sugeridos:

 |**Configuración**  |**Valor sugerido**  |
 |---------|---------|
 |Origen del ensamblado  |  Referencia de proyecto local o de CodeBehind   |
 |Recurso  |  Elegir datos de la cuenta actual   |
 |Subscription  |  Elija su suscripción.   |
 |Cuenta de almacenamiento  |  Elija la cuenta de almacenamiento.   |
 |Contenedor  |  Elija el contenedor que creó en la cuenta de almacenamiento.   |

    ![Azure Stream Analytics Edge job configuration in Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-job-config.png)

## <a name="limitations"></a>Limitaciones
Actualmente, la versión preliminar de UDF tiene las siguientes limitaciones:

* Los lenguajes de .NET Standard solo pueden utilizarse para Azure Stream Analytics en IoT Edge. Para los trabajos en la nube, puede escribir funciones definidas por el usuario de JavaScript. Para obtener más información, visite el tutorial [ Funciones definidas por el usuario en JavaScript para Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md).

* Las UDF de .NET Standard solo pueden crearse en Visual Studio y publicarse en Azure. Las versiones de solo lectura de las UDF de .NET Standard pueden verse en **Funciones** en Azure Portal. No se admite la creación de funciones de .NET Standard en Azure Portal.

* El editor de consultas de Azure Portal muestra un error cuando se usan UDF de .NET Standard en el portal. 

* Dado que el código personalizado comparte el contexto con el motor de Azure Stream Analytics, el código personalizado no puede hacer referencia a todo lo que tiene un espacio de nombres o dll_name en conflicto con el código de Azure Stream Analytics. Por ejemplo, no puede hacer referencia a *Newtonsoft Json*.

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Write a C# user-defined function for an Azure Stream Analytics Edge job in Visual Studio (Preview)](stream-analytics-edge-csharp-udf.md) (Escribir una función C# definido por el usuario para un trabajo de Azure Stream Analytics Edge [versión preliminar])
* [Tutorial: Funciones definidas por el usuario en JavaScript para Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md)
* [Uso de Visual Studio para ver trabajos de Azure Stream Analytics](stream-analytics-vs-tools.md)
