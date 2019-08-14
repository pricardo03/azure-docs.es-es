---
title: Guía de personalización de tareas de Análisis de código de seguridad de Microsoft Azure
description: Este artículo incluye información sobre la personalización de tareas en la extensión Análisis de código de seguridad
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: ab219b71eb8cd6f6172b7d02a639301c67811b49
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718191"
---
# <a name="how-to-configure--customize-the-build-tasks"></a>Instrucciones: configuración y personalización de las tareas de compilación

En esta página se describen en detalle las opciones de configuración disponibles en cada una de las tareas de compilación, desde las tareas de las herramientas de análisis de código de seguridad, hasta las tareas posteriores al procesamiento.

## <a name="anti-malware-scanner-task"></a>Tarea de Anti-Malware Scanner

> [!NOTE]
> La tarea de compilación Anti-Malware requiere un agente de compilación habilitado para Windows Defender. Esta condición se cumple para los agentes de compilación en la versión "Hosted VS2017" y posteriores. (No se ejecutará en un agente heredado o VS2015 "hospedado"). Las firmas no se pueden actualizar en estos agentes, aunque la firma siempre debe ser relativamente reciente, con una antigüedad menor a 3 horas.

Las capturas de pantalla y los detalles de la configuración se incluyen más adelante.

![Personalización de la tarea de compilación de Anti-Malware Scanner](./media/security-tools/5-add-anti-malware-task600.png) 

- Configuración de Type = **Basic**
- Con Type = **Custom**, se pueden proporcionar argumentos de línea de comandos para personalizar el examen.

Windows Defender usa el cliente de Windows Update para descargar e instalar firmas. Si se produce un error en la actualización de firmas en el agente de compilación, es probable que el código de error HRESULT proceda de Windows Update. Para obtener más información sobre los errores de Windows Update y la mitigación, visite [esta página](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) y esta otra [página de TechNet](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx)

## <a name="binskim-task"></a>Tarea de BinSkim

> [!NOTE]
> Como requisito previo para ejecutar la tarea BinSkim, la compilación debe cumplir una de las condiciones siguientes.
>    - La compilación genera artefactos binarios a partir de código administrado.
>    - Se han confirmado artefactos binarios que le gustaría analizar con BinSkim.

Las capturas de pantalla y los detalles de la configuración se incluyen más adelante. 

![Configuración de BinSkim](./media/security-tools/7-binskim-task-details.png)  
1. Establezca la configuración de compilación en Depurar para generar archivos de depuración * **.pdb**. BinSkim los usa para asignar los problemas encontrados en el archivo binario de salida con el código fuente. 
2. Elija Type = **Basic** y Function = **Analyze** para evitar la búsqueda y creación de su propia línea de comandos. 
3. **Destino**: uno o más especificadores para un archivo, un directorio o un patrón de filtro que se resuelve en uno o varios archivos binarios que se van a analizar. 
    - Cuando se agregan varios destinos, deben separarse con un **punto y coma (;)** . 
    - Puede ser un único archivo o contener caracteres comodín.
    - Los directorios siempre deben terminar con \*.
    - Ejemplos:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;
4. Si selecciona Type = **Command Line**, 
     - Asegúrese de que el primer argumento de **BinSkim.exe** es el verbo **analyze** (analizar) con rutas de acceso completas o rutas de acceso relativas al directorio de origen.
     - Para las entradas de **Command Line**, cuando se agreguen varios destinos deben separarse por un espacio.
     - Puede omitir el parámetro de archivo **/o** o **/output**; se agregará automáticamente o se reemplazará. 
     - **Configuración de línea de comandos estándar** 

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose
          > [!NOTE]
          > El carácter \* final es muy importante cuando se especifica un directorio o directorios para el destino. 

Para obtener más información sobre BinSkim en relación con los argumentos de la línea de comandos, las reglas por identificador o los códigos de salida, visite la [guía de usuario de BinSkim](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md).

## <a name="credential-scanner-task"></a>Tarea de Credential Scanner
Las capturas de pantalla y los detalles de la configuración se incluyen más adelante.
 
![Personalización de Credential Scanner](./media/security-tools/3-taskdetails.png)

Las opciones disponibles incluyen 
  - **Formato de salida**: TSV/CSV/SARIF/PREfast.
  - **Versión de la herramienta**: (recomendada: la más reciente).
  - **Scan Folder** (carpeta de examen): la carpeta del repositorio que se va a examinar.
  - **Searchers File Type** (tipo de archivo del buscador): opciones para encontrar el archivo de buscador utilizado para el examen.
  - **Suppressions File** (archivo de eliminaciones): se puede usar un archivo [JSON](https://json.org/) para eliminar los problemas en el registro de salida (más detalles en la sección Recursos). 
  - **Verbose Output** (resultado detallado): se explica por sí solo. 
  - **Tamaño del lote**: el número de subprocesos simultáneos que se usan para ejecutar instancias de Credential Scanner en paralelo. El valor predeterminado es 20 (el valor debe estar en el intervalo entre 1 y 2 147 483 647).
  - **Match Timeout** (tiempo de espera de coincidencia): la cantidad de tiempo que se invertirá en encontrar un buscador antes de detener la comprobación. 
  - **File Scan Read Buffer Size** (tamaño de búfer de lectura para el examen de archivos): tamaño del búfer durante la lectura de contenido en bytes. (El valor predeterminado es 524 288). 
  - **Maximum File Scan Read Bytes** (bytes de lectura máximos para el examen de archivos): número de máximo de bytes que van a leerse en un archivo determinado durante un análisis de contenido. (El valor predeterminado es 104 857 600). 
  - **Ejecutar esta tarea** (en las **Opciones de control**): especifica cuándo se debe ejecutar la tarea. Elija "Condiciones personalizadas" para especificar condiciones más complejas. 
  - **Versión**: versión de la tarea de compilación en Azure DevOps. No se usa con frecuencia. 

## <a name="microsoft-security-risk-detection-task"></a>Tarea de Microsoft Security Risk Detection
> [!NOTE]
> Tiene que crear y configurar una cuenta con el servicio de detección de riesgos como requisito previo para poder usar esta tarea. Este servicio requiere un proceso de incorporación independiente; no es Plug and Play como la mayoría de las otras tareas de esta extensión. Consulte [Microsoft Security Risk Detection](https://aka.ms/msrddocs) y [Microsoft Security Risk Detection: How To](https://docs.microsoft.com/security-risk-detection/how-to/) (Microsoft Security Risk Detection: instrucciones) para obtener instrucciones.

Los detalles de la configuración se incluyen más adelante.

Escriba los datos necesarios; cada opción muestra texto de ayuda al mantener el puntero.
   - **Azure DevOps Service Endpoint Name for MSRD** (nombre del punto de conexión de servicio de Azure DevOps para MSRD): si ha creado un tipo genérico de punto de conexión de servicio de Azure DevOps para almacenar la dirección URL de la instancia de MSRD (a la que se incorporó) y el token de acceso de la API REST, puede elegir ese punto de conexión de servicio. De lo contrario, haga clic en el vínculo Administrar para crear y configurar un nuevo punto de conexión de servicio para esta tarea de MSRD. 
   - **Id. de cuenta**: es un GUID que se puede recuperar de la dirección URL de la cuenta de MSRD.
   - **URLs to Binaries** (direcciones URL para archivos binarios): Una lista de direcciones URL disponibles públicamente delimitada por punto y coma (que va a usar el equipo de pruebas de vulnerabilidad para descargar los archivos binarios).**URLs of the Seed Files** (direcciones URL de los archivos de valores iniciales):
   - una lista de direcciones URL disponibles públicamente delimitada por punto y coma (que va a usar el equipo de pruebas de vulnerabilidad para descargar los valores iniciales). Este campo es opcional si se descargan los archivos de valores iniciales junto con los archivos binarios.
   - **OS Platform Type** (tipo de plataforma de SO): el tipo de plataforma del sistema operativo (Windows o Linux) de los equipos en los que se va a ejecutar el trabajo de pruebas de vulnerabilidad.
   - **Windows Edition / Linux Edition** (edición de Windows o Linux): edición del sistema operativo de los equipos en los que se va a ejecutar el trabajo de pruebas de vulnerabilidad. Puede sobrescribir el valor predeterminado si sus máquinas tienen una edición de sistema operativo diferente.
   - **Package Installation Script** (script de instalación del paquete): proporcione el script que se va a ejecutar en una máquina de pruebas para instalar el programa del destino de prueba y sus dependencias antes de enviar el trabajo de pruebas de vulnerabilidad.
   - **Job Submission Parameters** (parámetros de envío del trabajo):
       - **Seed Directory** (directorio de valores iniciales): ruta de acceso al directorio en el equipo de pruebas de vulnerabilidad que contiene los valores iniciales.
       - **Seed Extension** (extensión de los valores iniciales): la extensión de archivo de los valores iniciales.
       - **Test Driver Executable** (ejecutable del controlador de prueba): ruta de acceso al archivo ejecutable de destino en la máquina de pruebas de vulnerabilidad.
       - **Test Driver Executable Architecture** (arquitectura del ejecutable del controlador de prueba): arquitectura del archivo ejecutable de destino (x86 o AMD64).
       - **Test Driver Arguments** (argumentos del controlador de prueba): argumentos de la línea de comandos pasados al archivo ejecutable de destino de la prueba. Tenga en cuenta que el símbolo "%testfile%", incluidas las comillas dobles, se reemplazará automáticamente por la ruta de acceso completa al archivo de destino que se espera que el controlador de prueba analice, y es obligatorio.
       - **Test Driver Process Exits Upon Test Completion** (el proceso del controlador de prueba finaliza al completarse la prueba): active esta casilla para finalizar el controlador de prueba al completarse la prueba. Desactive esta casilla si el controlador de prueba debe cerrarse de manera forzada.
       - **Duración máxima (en segundos)** : proporcione una estimación del mayor tiempo que cabe esperar para que el programa de destino analice un archivo de entrada. Cuanto más preciso sea esta estimación, mayor será la eficacia de la ejecución de la prueba de vulnerabilidad.
       - **Test Driver Can Be Run Repeatedly** (el controlador de prueba puede ejecutarse repetidamente): active la casilla si el controlador de prueba se puede ejecutar repetidamente sin depender de un estado global persistente o compartido.
       - **Test Driver Can Be Renamed** (se puede cambiar el nombre del controlador de prueba): active la casilla si al cambiar el nombre del ejecutable del controlador de prueba, este puede seguir funcionando correctamente.
       - **The Fuzzing Application Runs as a Single OS Process** (la aplicación de pruebas de vulnerabilidad se ejecuta como un proceso de sistema operativo único): active la casilla si el controlador de prueba se ejecuta en un único proceso de sistema operativo. Desactive la casilla si el controlador de prueba genera procesos adicionales.


## <a name="roslyn-analyzers-task"></a>Tarea de analizadores basados en Roslyn
> [!NOTE]
> Como requisito previo para ejecutar la tarea del analizador basado en Roslyn, la compilación debe cumplir las siguientes condiciones.
>  - La definición de compilación incluye la tarea de compilación integrada MSBuild o VSBuild para compilar código de C# (o VB). Esta tarea se basa en la entrada y la salida de esa tarea de compilación específica para volver a ejecutar la compilación de MSBuild con los analizadores basados en Roslyn habilitados.
>  - El agente de compilación que ejecuta esta tarea de compilación tiene instalado Visual Studio 2017 versión 15.5 o posterior (versión 2.6.x del compilador).
>

Los detalles de la configuración se incluyen más adelante.

Las opciones disponibles incluyen 
- **Ruleset** (conjunto de reglas): SDL obligatorio, SDL recomendado, o bien puede usar un conjunto de reglas personalizado propio.
- **Analyzers Version** (versión del analizador): (recomendada: la más reciente).
- **Compiler Warnings Suppressions File** (archivo de eliminación de advertencias del compilador): archivo de texto con la lista de los identificadores de las advertencias que se deben eliminar. 
- **Ejecutar esta tarea** (en las **Opciones de control**): especifica cuándo se debe ejecutar la tarea. Elija "**Condiciones personalizadas**" para especificar condiciones más complejas. 

> [!NOTE]
> - Los analizadores basados en Roslyn están integrados en el compilador y solo se pueden ejecutar como parte de la compilación de CSC.exe. Por lo tanto, esta tarea requiere que se vuelva a reproducir o ejecutar el comando del compilador que se ejecutó anteriormente en la compilación. Para ello, se envía una consulta de VSTS para los registros de tareas de compilación de MSBuild (no hay otra manera de que la tarea obtenga de forma confiable la línea de comandos de compilación de MSBuild a partir de la definición de compilación; se planteó la opción de agregar un cuadro de texto de forma libre para permitir que los usuarios escriban sus líneas de comandos, pero sería difícil mantenerlos actualizados y sincronizados con la compilación principal). Las compilaciones personalizadas requieren que se vuelva a reproducir todo el conjunto de comandos, no solo los comandos del compilador, y no es sencillo ni confiable habilitar los analizadores basados en Roslyn en estos casos. 
> - Los analizadores basados en Roslyn se integran con el compilador y requieren que se invoque la compilación. Esta tarea de compilación se implementa al volver a compilar proyectos de C# que se crearon previamente solo con la tarea de compilación MSBuild/VSBuild en la misma compilación o definición de compilación. Sin embargo, en este caso, los analizadores deben estar habilitados. Si esta tarea de compilación se ejecuta en el mismo agente que la tarea de compilación original, los resultados de esta tarea de compilación sobrescribirán a los resultados de la tarea de compilación original de MSBuild/VSBuild en la carpeta de orígenes "s". El resultado de la compilación será el mismo, pero se recomienda ejecutar MSBuild, copiar los resultados en el directorio de almacenamiento provisional de los artefactos y, después, ejecutar Roslyn.
>

Para obtener recursos adicionales para la tarea del analizador basado en Roslyn, consulte los [analizadores basados en Roslyn en Microsoft Docs](https://docs.microsoft.com/dotnet/standard/analyzers/).

Puede encontrar el paquete de analizador que se instaló y usó en esta tarea de compilación [aquí](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers). 

## <a name="tslint-task"></a>Tarea de TSLint

Para obtener más información sobre TSLint, visite el [repositorio de GitHub de TSLint](https://github.com/palantir/tslint).
>[!NOTE] 
>Como quizá sepa, TSLint dejará de usarse en algún momento de 2019 (fuente: [repositorio de GitHub de TSLint](https://github.com/palantir/tslint)). Actualmente, el equipo está investigando la posibilidad de usar [ESLint](https://github.com/eslint/eslint) como una alternativa, y está planeada la creación de una nueva tarea para ESLint.

## <a name="publish-security-analysis-logs-task"></a>Tarea de Publish Security Analysis Logs
Las capturas de pantalla y los detalles de la configuración se incluyen más adelante.

![Personalización de Publish Security Analysis](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Nombre del artefacto**: puede ser cualquier identificador de cadena.
- **Tipo de artefacto**: puede publicar los registros en el servidor de Azure-DevOps o en un recurso compartido al que pueda acceder el agente de compilación. 
- **Herramientas**: puede optar por conservar los registros de herramientas individuales o específicas, o bien puede seleccionar **All Tools** (todas las herramientas) para conservar todos los registros. 

## <a name="security-report-task"></a>Tarea de Security Report
Las capturas de pantalla y los detalles de la configuración se incluyen más adelante.  
![Configuración de Post-Analysis](./media/security-tools/4-createsecurityanalysisreport600.png) 
- **Informes**: elija los archivos de informe que quiere crear; se creará uno en cada formato **Consola**, **TSV**, y/o **HTML**. 
- **Herramientas**: seleccione las herramientas de la definición de compilación para las que quiere obtener un resumen de los problemas detectados. Para cada herramienta seleccionada, puede haber una opción para seleccionar si en el informe quiere ver solo los errores o los errores y las advertencias. 
- **Opciones avanzadas**: puede optar por registrar una advertencia o un error (y no realizar la tarea) en caso de que no haya registros para alguna de las herramientas seleccionadas.
Puede personalizar la carpeta de registros base donde se encuentran los registros, pero no es un escenario típico. 

## <a name="post-analysis-task"></a>Tarea de Post-Analysis
Las capturas de pantalla y los detalles de la configuración se incluyen más adelante.

![Personalización de Post-Analysis](./media/security-tools/a-post-analysis600.png) 
- **Herramientas**: seleccione las herramientas de la definición de compilación para las que quiere insertar una interrupción de la compilación en función de los resultados. Para cada herramienta seleccionada, puede haber una opción para seleccionar si quiere realizar una interrupción solo cuando haya errores o cuando haya errores y advertencias. 
- **Informe**: si quiere, puede escribir los resultados encontrados que provocan la interrupción de la compilación en la ventana de la consola de Azure DevOps y en el archivo de registro. 
- **Opciones avanzadas**: puede optar por registrar una advertencia o un error (y no realizar la tarea) en caso de que no haya registros para alguna de las herramientas seleccionadas.

## <a name="next-steps"></a>Pasos siguientes

Si tiene más preguntas sobre la extensión y las herramientas que se ofrecen, [consulte nuestra página de preguntas más frecuentes.](security-code-analysis-faq.md)


