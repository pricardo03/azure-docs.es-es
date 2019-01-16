---
title: 'Azure Data Factory: Preguntas más frecuentes | Microsoft Docs'
description: Obtenga respuestas a las preguntas más frecuentes acerca de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: be0cdeed81c66e1a848b44d2429c1c67bce9b4f3
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024100"
---
# <a name="azure-data-factory-faq"></a>Preguntas más frecuentes de Azure Data Factory
Este artículo ofrece respuestas a las preguntas más frecuentes sobre Azure Data Factory.  

## <a name="what-is-azure-data-factory"></a>¿Qué es Azure Data Factory? 
Data Factory es un servicio de integración de datos basado en la nube que automatiza el movimiento y la transformación de datos. Al igual que las máquinas de una fábrica transforman las materias primas en productos acabados, Azure Data Factory organiza los servicios existentes que recopilan datos sin procesar y los transforman en información lista para utilizar. 

Con Azure Data Factory, puede crear flujos de trabajo controlados por datos para mover datos entre almacenes de datos locales y en la nube. Puede procesar y transformar datos mediante servicios de proceso, como Azure HDInsight, Azure Data Lake Analytics e Integration Runtime de SQL Server Integration Services (SSIS). 

Con Data Factory, puede ejecutar el procesamiento de datos en un servicio en la nube basado en Azure o en su propio entorno de proceso autohospedado, como SSIS, SQL Server u Oracle. Después de crear una canalización que realice la acción que necesita, puede programarla para que se ejecute periódicamente (por ejemplo, cada hora, diariamente o semanalmente), programar una ventana de tiempo o desencadenarla a partir de la aparición de un evento. Para más información, consulte [Introducción a Azure Data Factory](introduction.md).

### <a name="control-flows-and-scale"></a>Escala y flujos de control 
Para admitir los diversos flujos y patrones del almacenamiento de datos actual, Data Factory permite un modelado flexible de la canalización de datos que incluye paradigmas completos de programación del flujo de control que incluyen la ejecución condicional, la bifurcación de las canalizaciones de datos y la posibilidad de pasar parámetros explícitamente dentro de estos flujos y entre ellos. Flujo de control también abarca la transformación de datos mediante la distribución de actividades a motores de ejecución externos y funcionalidades de flujo de datos que incluyen el movimiento de datos a escala mediante la actividad de copia.

Data Factory proporciona la libertad de modelar cualquier estilo de flujo necesario para la integración de datos y que se puede enviar a petición o varias veces según una programación. Estos son algunos de los flujos habituales que permite este modelo:   

- Flujos de control:
    - Encadenamiento de actividades en una secuencia dentro de una canalización.
    - Creación de una rama de actividades dentro de una canalización.
    - Parámetros
        - Defina parámetros en el nivel de canalización y pase argumentos al invocar la canalización a petición o mediante un desencadenador.
        - Las actividades pueden consumir los argumentos que se pasan a la canalización.
    - Paso a estado personalizado
        - Los resultados de la actividad, incluido el estado, se pueden usar en una posterior actividad de la canalización.
    - Contenedores de bucle
        - Para cada 
- Flujos basados en el desencadenador:
    - Las canalizaciones se pueden desencadenar a petición o por tiempo de reloj.
- Flujos delta:
    - Use parámetros y defina el límite máximo de la copia delta al mover tablas de dimensiones o de referencia desde un almacén relacional de forma local o en la nube para cargar los datos en Data Lake. 

Para más información, consulte [Tutorial: Flujos de control](tutorial-control-flow.md).

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Transformación de los datos a escala con canalizaciones sin código
La nueva experiencia de herramientas basadas en explorador proporciona la creación e implementación de canalizaciones sin código con una experiencia actual, interactiva y basada en la web.

Para los desarrolladores de datos visuales y los ingenieros de datos, la interfaz de usuario web de ADF es el entorno idóneo de desarrollo sin código que usará para crear canalizaciones. Está totalmente integrado con el repositorio de Git en línea de Visual Studio y proporciona integración de CI/CD y desarrollo iterativo con opciones de depuración.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Potentes SDK multiplataforma para usuarios avanzados
Si es un usuario avanzado y busca una interfaz programática, la versión 2 de ADF le proporciona un amplio conjunto de SDK que se pueden aprovechar para crear, administrar y supervisar canalizaciones mediante su IDE favorito.
1.  SDK de Python
2.  Powershell CLI
3.  Los usuarios de SDK de C# también pueden aprovechar las API REST documentadas para interactuar con ADF V2

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Desarrollo y depuración iterativos mediante herramientas visuales
Las herramientas visuales de Azure Data Factory (ADF) le permiten realizar un desarrollo y depuración iterativos. Puede crear canalizaciones y realizar series de pruebas con la funcionalidad Depurar en el lienzo de la canalización sin escribir ni una sola línea de código. Puede ver los resultados de las series de pruebas en la ventana Salida del lienzo de la canalización. Después de realizar correctamente una serie de pruebas, puede agregar más actividades a la canalización y continuar con la depuración de manera iterativa. También puede cancelar las series de pruebas cuando están en curso. No es necesario que publique los cambios en el servicio Data Factory antes de hacer clic en Depurar. Esto resulta útil en escenarios en los que se desea garantizar que los cambios o nuevas adiciones funcionan según lo esperado antes de actualizar el flujo de trabajo de la factoría de datos en entornos de desarrollo, pruebas o producción. 

### <a name="deploy-ssis-packages-to-azure"></a>Implementación de paquetes SSIS en Azure 
Si desea mover las cargas de trabajo de SSIS, puede crear una instancia de Data Factory y aprovisionar una instancia de Integration Runtime para la integración de SSIS en Azure. Integration Runtime de SSIS de Azure es un clúster totalmente administrado de máquinas virtuales de Azure (nodos) que se dedican a ejecutar los paquetes SSIS en la nube. Para obtener instrucciones paso a paso, vea el tutorial [Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md). 
 
### <a name="sdks"></a>SDK
Si es un usuario avanzado y busca una interfaz programática, ADF le proporciona un amplio conjunto de SDK que puede usar para crear, administrar o supervisar canalizaciones mediante su IDE favorito. Entre los lenguajes compatibles se incluye: .NET, PowerShell, Python y REST.

### <a name="monitoring"></a>Supervisión
Puede supervisar las factorías de datos mediante PowerShell, SDK o las herramientas de supervisión visual de la interfaz de usuario del explorador. Puede supervisar y administrar a petición flujos personalizados basados en desencadenadores y controlados por tiempo de una manera eficaz. Cancele tareas que ya existen, vea los errores de un solo vistazo, explore en profundidad para obtener mensajes de error detallados y depure los problemas, todo ello desde un único panel sin cambios de contexto y sin tener que desplazarse entre pantallas. 

### <a name="new-features-for-ssis-in-adf"></a>Nuevas características de SSIS en ADF
Desde el lanzamiento inicial de la versión preliminar pública en 2017, Data Factory ha agregado las siguientes características para SSIS:

-   Compatibilidad con tres configuraciones o variantes más de Azure SQL Database (DB) para hospedar el catálogo de proyectos y paquetes de SSIS (SSISDB):
-   Azure SQL Database con puntos de conexión de servicio de red virtual
-   Instancia administrada (MI)
-   Grupo elástico
-   Compatibilidad con la red virtual de Azure Resource Manager en lugar de la red virtual clásica que caerá en desuso en el futuro. Esto le permite insertar o unir su instancia de Integration Runtime para la integración de SSIS en Azure en una red virtual que está configurada para Azure SQL Database con acceso a puntos de conexión de servicio de red virtual, Instancias administradas o datos locales. Para ello, consulte: https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network. 
-   Compatibilidad con la autenticación de Azure Active Directory (AAD) en lugar de la autenticación de SQL para conectarse a SSISDB. Esto le permite usar la autenticación de AAD con su identidad administrada de ADF para los recursos de Azure
-   Compatibilidad para usar su propia licencia de SQL Server local para conseguir ahorros sustanciales de costos con la opción Ventaja híbrida de Azure (AHB).
-   Compatibilidad con la versión empresarial de Integration Runtime para la integración de SSIS en Azure que le permite usar características avanzadas o premium, configuraciones personalizadas para instalar componentes o extensiones adicionales y ecosistemas de terceros. Para más información, consulte: https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/. 
-   Una integración más profunda de SSIS en ADF que le permite invocar o desencadenar actividades de primera clase de ejecución de paquetes SSIS en canalizaciones de ADF y programarlas mediante SSMS. Para más información, consulte: https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/ 


## <a name="what-is-integration-runtime"></a>¿Qué es Integration Runtime?
Integration Runtime es la infraestructura de proceso que Azure Data Factory usa para proporcionar las siguientes funcionalidades de integración de datos en diversos entornos de red:

- **Movimiento de datos**: Para el movimiento de datos, Integration Runtime mueve los datos entre los almacenes de origen y de destino e incluye funcionalidad de conectores integrados, conversión de formato, asignación de columnas y transferencia de datos eficaz y escalable.
- **Distribución de actividades**: para la transformación, Integration Runtime permite ejecutar paquetes SSIS de forma nativa.
- **Ejecución de paquetes SSIS**: ejecuta de forma nativa paquetes SSIS en un entorno de proceso de Azure administrado. Integration Runtime permite también distribuir y supervisar actividades de transformación que se ejecutan en una gran variedad de servicios de proceso, como Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server, etc.

Puede implementar una o varias instancias de Integration Runtime según sea necesario para mover y transformar los datos. Integration Runtime se puede ejecutar en una red de Azure pública o en una red privada (local, Azure Virtual Network o en la nube privada virtual de Amazon Web Services [VPC]). 

Para más información, consulte [Integration Runtime en Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>¿Cuál es el límite del número de Integration Runtimes?
No hay ningún límite estricto acerca del número de instancias de Integration Runtime que puede tener en una factoría de datos. Sin embargo, existe un límite acerca del número de núcleos de máquina virtual que Integration Runtime puede usar por suscripción para la ejecución de paquetes SSIS. Para más información, consulte los [Límites de Data Factory](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>¿Cuáles son los conceptos de nivel superior de Azure Data Factory?
Una suscripción de Azure puede tener una o varias instancias de Azure Data Factory (o factorías de datos). Azure Data Factory contiene cuatro componentes principales que funcionan juntos como plataforma en la que pueda crear flujos de trabajo orientados a datos con pasos para moverlos y transformarlos.

### <a name="pipelines"></a>Procesos
Una factoría de datos puede tener una o más canalizaciones. Una canalización es una agrupación lógica de actividades para realizar una unidad de trabajo. Juntas, las actividades de una canalización realizan una tarea. Por ejemplo, una canalización puede contener un grupo de actividades que ingiere datos de un blob de Azure y luego ejecuta una consulta de Hive en un clúster de HDInsight para particionar los datos. La ventaja es que puede usar una canalización para administrar las actividades como un conjunto en lugar de tener que administrar individualmente cada actividad. Puede encadenar juntas las actividades en una canalización para hacerlas funcionar de forma secuencial o puede hacerlas funcionar de forma independiente en paralelo.

### <a name="activity"></a>Actividad
Las actividades representan un paso del procesamiento en una canalización. Por ejemplo, puede utilizar una actividad de *Copia* para copiar datos de un almacén de datos a otro. De igual forma, puede usar una actividad de Hive, que ejecuta una consulta de Hive en un clúster de Azure HDInsight para transformar o analizar los datos. Data Factory admite tres tipos de actividades: actividades de movimiento de datos, actividades de transformación de datos y actividades de control.

### <a name="datasets"></a>Conjuntos de datos
Los conjuntos de datos representan las estructuras de datos de los almacenes de datos que simplemente apuntan o hacen referencia a los datos que desea utilizar en sus actividades como entradas o salidas. 

### <a name="linked-services"></a>Servicios vinculados
Los servicios vinculados son muy similares a las cadenas de conexión que definen la información de conexión necesaria para que Data Factory se conecte a recursos externos. Considérelos de esta forma: un servicio vinculado define la conexión al origen de datos y un conjunto de datos representa la estructura de los datos. Por ejemplo, un servicio vinculado de Azure Storage especifica la cadena de conexión para conectarse a la cuenta de Azure Storage. Además, un conjunto de datos de Azure Blob especifica el contenedor de blobs y la carpeta que contiene los datos.

Los servicios vinculados tienen dos fines en Data Factory:

- Para representar un *almacén de datos* que incluye, entre otros, una instancia de SQL Server local, una instancia de base de datos de Oracle, un recurso compartido de archivos o una cuenta de Azure Blob Storage. Para obtener una lista de almacenes de datos compatibles, consulte [Actividad de copia en Azure Data Factory](copy-activity-overview.md).
- Para representar un *recurso de proceso* que puede hospedar la ejecución de una actividad. Por ejemplo, la actividad HDInsight Hive se ejecuta en un clúster de Hadoop para HDInsight. Para ver una lista de actividades de transformación y de entornos de proceso admitidos, consulte el artículo [Transformar datos en Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Desencadenadores
Los desencadenadores representan unidades de procesamiento que determinan cuándo se pone en marcha una ejecución de canalización. Existen diferentes tipos de desencadenadores para diferentes tipos de eventos. 

### <a name="pipeline-runs"></a>Ejecuciones de la canalización
Una ejecución de una canalización es una instancia de la ejecución de la canalización. Normalmente, crea instancias de una ejecución de canalización al pasar argumentos a los parámetros definidos en la canalización. Puede pasar los argumentos manualmente o dentro de la definición del desencadenador.

### <a name="parameters"></a>Parámetros
Los parámetros son pares clave-valor en una configuración de solo lectura. Defina parámetros en una canalización y pase los argumentos para los parámetros definidos durante la ejecución de un contexto de ejecución. El contexto de ejecución se crea mediante un desencadenador o desde una canalización que ejecuta manualmente. Las actividades dentro de la canalización consumen los valores de parámetro.

Un conjunto de datos es un parámetro fuertemente tipado y una entidad que puede reutilizar o a la que puede hacer referencia. Una actividad puede hacer referencia a conjuntos de datos y puede consumir las propiedades definidas en la definición del conjunto de datos.

Un servicio vinculado también es un parámetro fuertemente tipado que contiene información de conexión a un almacén de datos o a un entorno de proceso. También es una entidad que puede reutilizar o a la que puede hacer referencia.

### <a name="control-flows"></a>Flujos de control
Los flujos de control organizan actividades de canalización que incluyen el encadenamiento de actividades en una secuencia, la creación de ramas, los parámetros que define en el nivel de canalización y los argumentos que pasa al invocar la canalización a petición o desde un desencadenador. Los flujos de control también incluyen el paso a un estado personalizado y contenedores de bucle (es decir, los iteradores Para cada).


Para más información sobre los conceptos de Data Factory, consulte los siguientes artículos:

- [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md)
- [Canalizaciones y actividades](concepts-pipelines-activities.md)
- [Integration runtime](concepts-integration-runtime.md) (Tiempo de ejecución de integración)

## <a name="what-is-the-pricing-model-for-data-factory"></a>¿Qué es el modelo de precios de Data Factory?
Consulte los [detalles de precios de Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) para obtener información al respecto.

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>¿Cómo puedo mantenerme actualizado con información acerca de Data Factory?
Para ver la información más actualizada acerca de Azure Data Factory, vaya a los sitios siguientes:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Página principal de la documentación](/azure/data-factory)
- [Página principal del producto](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Inmersión técnica profunda 

### <a name="how-can-i-schedule-a-pipeline"></a>¿Cómo puedo programar una canalización? 
Puede usar el desencadenador de programador o el desencadenador de ventana de tiempo para programar una canalización. El desencadenador utiliza una programación del calendario del reloj y podrá utilizarlo para programar canalizaciones periódicamente o mediante patrones periódicos basados en calendarios (por ejemplo, semanalmente los lunes a las 6 p.m. y los jueves a las 9 p.m.). Para obtener más información, consulte [Desencadenadores y ejecución de la canalización](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>¿Puedo pasar parámetros a una ejecución de canalización?
Sí, los parámetros son conceptos de primera clase de nivel superior en ADF. Puede definir parámetros en el nivel de canalización y pasar argumentos al ejecutar la canalización a petición o mediante un desencadenador.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>¿Puedo definir valores predeterminados para los parámetros de la canalización? 
Sí. Puede definir valores predeterminados para los parámetros de las canalizaciones. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>¿Una actividad de una canalización puede consumir los argumentos que se pasan a una ejecución de canalización? 
Sí. Cada actividad dentro de la canalización puede utilizar el valor del parámetro que se pasa a la canalización y que se ejecuta con la construcción de `@parameter`. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>¿Puede utilizarse una propiedad de salida de actividad en otra actividad? 
Sí. Un resultado de la actividad se puede utilizar en una actividad posterior con la construcción de `@activity`.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>¿Cómo puedo controlar correctamente los valores NULL en una salida de actividad? 
Puede usar la construcción de `@coalesce` en las expresiones para controlar correctamente los valores NULL. 

## <a name="next-steps"></a>Pasos siguientes
Para obtener instrucciones paso a paso para crear una factoría de datos, consulte los siguientes tutoriales:

- [Inicio rápido: Crear una factoría de datos](quickstart-create-data-factory-dot-net.md)
- [Tutorial: Copia de datos en la nube](tutorial-copy-data-dot-net.md)
