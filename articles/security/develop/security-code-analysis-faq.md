---
title: Preguntas frecuentes sobre la documentación de Análisis de código de seguridad de Microsoft Azure
description: Este artículo incluye preguntas frecuentes sobre la extensión Análisis de código de seguridad
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
ms.openlocfilehash: 1fc5e83c2c46a7da2a4b56879a2d596405fd8bee
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718179"
---
# <a name="frequently-asked-questions"></a>Preguntas más frecuentes
¿Tiene alguna pregunta? Eche un vistazo a las preguntas frecuentes a continuación para obtener más información.

## <a name="general-faqs"></a>Preguntas frecuentes generales

### <a name="can-i-install-the-extension-on-my-tfs-not-azure-devops-server"></a>¿Puedo instalar la extensión en mi servidor TFS (no Azure DevOps)? 

No, la extensión no está disponible para descargarse e instalarse en TFS.

### <a name="do-i-have-to-run-microsoft-security-code-analysis-with-my-build"></a>¿Tengo que ejecutar Análisis de código de seguridad de Microsoft con mi compilación? 

Sí y no. En función del tipo de herramienta de análisis, el propio código fuente puede ser lo único que se necesita o es posible que se requiera el resultado de la compilación. Por ejemplo, puesto que Credential Scanner analiza los archivos dentro de la estructura de carpetas del repositorio de código, puede ejecutar las tareas de compilación de Credential Scanner y Publish Security Analysis Logs en una compilación independiente para recuperar los resultados.
En el caso de otras herramientas que analizan los artefactos posteriores a la compilación, como BinSkim, primero necesitará de la compilación.

### <a name="can-i-break-my-build-when-results-are-found"></a>¿Puedo interrumpir mi compilación cuando se encuentren los resultados? 
Sí, puede introducir una interrupción de la compilación cuando cualquier herramienta informe de un problema o un resultado en el archivo de registro. Simplemente agregue la tarea de compilación Post-Analysis y active la casilla de la herramienta para la que quiera interrumpir la compilación. Puede optar por interrumpir la compilación cuando alguna herramienta notifique errores, o advertencias y errores, en la interfaz de usuario de la tarea Post-Analysis.

### <a name="how-are-the-command-line-arguments-different-in-azure-devops-than-they-are-in-the-standalone-desktop-tools"></a>¿En qué se diferencian los argumentos de la línea de comandos en Azure DevOps y los mismos argumentos en las herramientas de escritorio independientes? 

Por lo general, las tareas de compilación de Azure DevOps son contenedores directos para los argumentos de la línea de comandos de las herramientas de seguridad. Todo lo que normalmente se pasaría a la herramienta en la línea de comandos desde el escritorio, puede pasarse a la entrada de argumentos de la tarea de compilación.
Esta es una lista de las diferencias destacables:
 - La herramienta se ejecutará desde la carpeta de origen del agente $(Build.SourcesDirectory) o %BUILD_SOURCESDIRECTORY%. Ejemplo: C:\agent\_work\1\s 
 - Las rutas de acceso de los argumentos pueden ser relativas a la raíz del directorio de origen mencionado anteriormente o pueden ser absolutos, ya sea mediante la ejecución de un agente local con ubicaciones de implementación conocidas de los recursos locales o mediante variables de compilación de Azure DevOps.
 - Las herramientas proporcionarán automáticamente una ruta de acceso o carpeta de archivo de salida. Si se proporciona una ruta de acceso de salida, se quitará y se reemplazará por una ruta de acceso a la ubicación de registros conocida en el agente de compilación.
 - Algunos parámetros de línea de comandos adicionales se depuran y se quitan en algunas herramientas, como la adición o eliminación de opciones para asegurarse de que no se inicia ninguna GUI.

### <a name="can-i-run-a-build-task-for-example-credential-scanner-across-multiple-repositories-in-an-azure-devops-build"></a>¿Puedo ejecutar una tarea de compilación (por ejemplo, Credential Scanner) en varios repositorios en una compilación de Azure DevOps? 

No, actualmente no se admite la ejecución de las herramientas de desarrollo seguro en varios repositorios mediante una sola canalización.

###  <a name="the-output-file-i-specified-is-not-being-created--i-cant-find-the-output-file-i-specified"></a>El archivo de salida que especifiqué no se está creando o no encuentro el archivo de salida especificado 

Actualmente, las tareas de compilación corrigen la entrada del usuario y actualizan la ubicación del archivo de salida generado en una ubicación común en el agente de compilación. Para obtener más información acerca de esta ubicación, consulte las siguientes preguntas.

### <a name="where-are-the-output-files-generated-by-the-tools-saved"></a>¿Dónde se guardan los archivos de salida generados por las herramientas? 

Las tareas de compilación agregan automáticamente las rutas de acceso de los resultados a la siguiente ubicación conocida en el agente de compilación $(Agent.BuildDirectory)\_sdt\logs. Ya que esta ubicación está normalizada, podemos garantizar que otros equipos que produzcan registros de análisis de código o que los consuman podrán acceder a ellos.

### <a name="can-i-queue-a-build-to-run-these-tasks-on-a-hosted-build-agent"></a>¿Se puede poner en cola una compilación para ejecutar estas tareas en un agente de compilación hospedado? 

Sí, todas las tareas y herramientas de la extensión se pueden ejecutar en un agente de compilación hospedado.

>[!NOTE]
> La tarea de compilación Anti-Malware requiere un agente de compilación habilitado para Windows Defender. Esta condición se cumple para los agentes de compilación en la versión "Hosted VS2017" o posteriores. (No se ejecutará en el agente heredado o VS2015 "hospedado"). Las firmas no se pueden actualizar en estos agentes, aunque la firma siempre debe ser relativamente reciente, con una antigüedad menor a 3 horas.
>

### <a name="can-i-run-these-build-tasks-as-part-of-a-release-pipeline-as-opposed-to-a-build-pipeline"></a>¿Puedo ejecutar estas tareas de compilación como parte de una canalización de versión (en lugar de una canalización de compilación)? 
En la mayoría de los casos, sí. Sin embargo, Azure DevOps no admite tareas que publican artefactos para que se ejecuten desde canalizaciones de versiones: "La única categoría de tareas que no se espera que funcione con las canalizaciones de versiones son las que publican artefactos. Esto se debe a que, a partir de ahora, no se admite la publicación de artefactos en la canalización de versiones".
Esto evita que la tarea "Publish Security Analysis Logs" se ejecute correctamente desde una canalización de versiones: se producirá un error, con un mensaje de error descriptivo.

### <a name="from-where-do-the-build-tasks-download-the-tools"></a>¿Desde dónde descargan las herramientas las tareas de compilación? 
Las tareas de compilación a) descargan paquetes NuGet para las herramientas de la siguiente [fuente de administración de paquetes de Azure DevOps](https://securitytools.pkgs.visualstudio.com/_packaging/SecureDevelopmentTools/nuget/v3/index.json) o mediante el administrador de paquetes de Node, que debe estar preinstalado en el agente de compilación (por ejemplo: "npm install tslint").

### <a name="what-effect-will-installing-the-extension-have-on-my-azure-devops-organization"></a>¿Qué efecto tendrá la instalación de la extensión en mi organización de Azure DevOps? 

Tras la instalación, las tareas de compilación de seguridad proporcionadas por la extensión estarán disponibles para que las usen todos los usuarios de la organización. Al crear o editar una canalización de Azure, estas tareas estarán disponibles para agregarse desde la lista de recopilación de tareas de compilación. Sin embargo, la instalación de la extensión en la organización de Azure DevOps no tiene ningún impacto adicional. No modifica ninguna configuración de cuenta o proyecto ni ninguna canalización.

### <a name="will-installing-the-extension-modify-my-existing-azure-pipelines"></a>¿La instalación de la extensión modifica mi instancia existente de Azure Pipelines? 

No. La instalación de la extensión hará que las tareas de compilación de seguridad estén disponibles para agregarlas a Azure Pipelines. Todavía es necesario que los usuarios agreguen o actualicen las definiciones de compilación para integrar las herramientas en el proceso de compilación.

## <a name="task-specific-faqs"></a>Preguntas más frecuentes específicas de las tareas

En esta sección se enumeran las preguntas frecuentes específicas de las tareas de compilación.

### <a name="credential-scanner-faqs"></a>Preguntas frecuentes de Credential Scanner

#### <a name="what-are-common-suppressions-scenarios-and-examples"></a>¿Cuáles son escenarios y ejemplos comunes de supresión? 
A continuación se detallan dos de los escenarios de supresión más comunes:
##### <a name="suppress-all-occurrences-of-a-given-secret-within-the-specified-path"></a>Suprimir todas las repeticiones de un secreto determinado dentro de la ruta de acceso especificada 
Como se muestra en el ejemplo siguiente, se necesita la clave hash del secreto ubicada en el archivo de salida de Credential Scanner.
   
        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "hash": "CLgYxl2FcQE8XZgha9/UbKLTkJkUh3Vakkxh2CAdhtY=",
                "_justification": "Secret used by MSDN sample, it is fake."
            }
          ]
        }

>[!WARNING]
> La clave hash se genera mediante una parte del valor coincidente o del contenido del archivo. Cualquier revisión de código fuente podría cambiar la clave hash y deshabilitar la regla de supresión. 

##### <a name="to-suppress-all-secrets-in-a-specified-file-or-to-suppress-the-secrets-file-itself"></a>Para suprimir todos los secretos de un archivo especificado (o para suprimir el archivo de secretos) 
La expresión de archivo puede ser un nombre de archivo o cualquier parte de postfijo del nombre o la ruta de acceso completa del archivo. No se admiten caracteres comodín. 

**Ejemplo** 

Archivo que se va a eliminar: [InputPath]\src\JS\lib\angular.js 

Reglas de eliminación válidas: 
- [InputPath]\src\JS\lib\angular.js: elimina el archivo en la ruta de acceso especificada
- \src\JS\lib\angular.js
- \JS\lib\angular.js
- \lib\angular.js
- angular.js: elimina cualquier archivo con el mismo nombre

        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "file": "\\files\\AdditonalSearcher.xml", 
                "_justification": "Additional CredScan searcher specific to my team"
            },
            {
                "file": "\\files\\unittest.pfx", 
                "_justification": "Legitimate UT certificate file with private key"
            }
          ]
        }      

>[!WARNING] 
> Todos los secretos futuros que se agreguen al archivo también se eliminarán automáticamente. 

#### <a name="what-are-recommended-secrets-management-guidelines"></a>¿Cuáles son las directrices de administración de secretos recomendadas? 
Aunque la detección de secretos incrustados en el código de forma oportuna y la mitigación de los riesgos resultan de gran utilidad, es incluso mejor si es posible evitar por completo que los secretos se inserten en el repositorio. En este sentido, Microsoft ha lanzado CredScan Code Analyzer como parte de la [extensión de Microsoft DevLabs](https://marketplace.visualstudio.com/items?itemName=VSIDEDevOpsMSFT.ContinuousDeliveryToolsforVisualStudio) para Visual Studio. Aunque se encuentra en versión preliminar, proporciona a los desarrolladores una experiencia en línea para detectar posibles secretos en el código, lo que les da la oportunidad de corregir dichos problemas en tiempo real. Para obtener más información, consulte [este](https://devblogs.microsoft.com/visualstudio/managing-secrets-securely-in-the-cloud/) blog sobre la administración de secretos de forma segura en la nube. A continuación se muestran algunos recursos adicionales que le ayudarán a administrar secretos y acceder a información confidencial desde dentro de sus aplicaciones de forma segura: 
 - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
 - [Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication)
 - [Managed Service Identity de Azure AD](https://azure.microsoft.com/blog/keep-credentials-out-of-code-introducing-azure-ad-managed-service-identity/)
 - [Managed Service Identity (MSI) para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
 - [Managed Service Identity de Azure](https://docs.microsoft.com/azure/app-service/overview-managed-identity)
 - [Biblioteca de AppAuthentication](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication)

#### <a name="can-i-write-my-own-custom-searchers"></a>¿Puedo escribir mis propios buscadores personalizados?

Credential Scanner se basa en un conjunto de buscadores de contenido que se suelen definir en el archivo **buildsearchers.xml**. El archivo contiene una matriz de objetos serializados en XML que representan un objeto ContentSearcher. El programa se distribuye con un conjunto de buscadores que se han probado debidamente, aunque también le permite implementar sus propios buscadores personalizados. 

Un buscador de contenido se define de la siguiente manera: 

- **Name**: nombre descriptivo del buscador que se usará en el archivo de salida de Credential Scanner. Se recomienda usar la convención de nomenclatura en mayúsculas y minúsculas para los nombres de los buscadores. 
- **RuleId**: identificador opaco estable del buscador. 
    - A los buscadores predeterminados de Credential Scanner se les asignan identificadores RuleId como CSCAN0010, CSCAN0020, CSCAN0030, etc. El último dígito está reservado para una posible división o combinación de grupos de expresión regular del buscador.
    - El valor de RuleId de los buscadores personalizados debe tener su propio espacio de nombres en el siguiente formato: CSCAN-{EspacioDeNombres}0010, CSCAN-{EspacioDeNombres}0020, CSCAN-{EspacioDeNombres}0030, etc.
    - El nombre completo del buscador es la combinación del RuleId y el nombre del buscador. Por ejemplo, CSCAN0010.KeyStoreFiles, CSCAN0020.Base64EncodedCertificate, etc.
- **ResourceMatchPattern**: expresión regular de las extensiones de archivo que se van a comprobar en el buscador.
- **ContentSearchPatterns**: matriz de cadenas que contienen las instrucciones de expresión regular que deben coincidir. Si no se han definido patrones de búsqueda, se devolverán todos los archivos que coincidan con el patrón de coincidencia de recursos.
- **ContentSearchFilters**: matriz de cadenas que contienen instrucciones de expresión regular para filtrar falsos positivos específicos del buscador.
- **Matchdetails**: un mensaje descriptivo o instrucciones de mitigación que se van a agregar a cada coincidencia del buscador.
- **Recommendation**: proporciona el contenido del campo de sugerencias para las coincidencias con el formato de informe PREfast.
- **Severity**: un entero para reflejar la gravedad del problema (número más alto = 1).
![Configuración de Credential Scanner](./media/security-tools/6-credscan-customsearchers.png)

### <a name="roslyn-analyzers-faqs"></a>Preguntas frecuentes de analizadores basados en Roslyn

#### <a name="what-are-the-most-common-errors-when-using-the-roslyn-analyzers-task"></a>¿Cuáles son los errores más comunes cuando se la tarea de analizadores basados en Roslyn?

**Error: El proyecto se restauró con la versión de Microsoft.NETCore.App x.x.x, pero con la configuración actual se usaría la versión y.y.y. Para solucionar este problema, asegúrese de que se usa la misma configuración para la restauración y para las operaciones posteriores como compilar o publicar. Normalmente, este problema puede producirse si la propiedad RuntimeIdentifier se establece durante la compilación o publicación, en lugar de durante la restauración:**

Los analizadores basados en Roslyn se ejecutan como parte de la compilación, por lo que el árbol de origen de la máquina de compilación debe estar en un estado que pueda compilarse. Es posible que una instrucción (probablemente "dotnet.exe publish") entre la compilación principal y los analizadores basados en Roslyn haya puesto el árbol de origen en un estado que no puede compilarse. Es posible que, al duplicar el paso que realiza una restauración de Nuget, justo antes de la instrucción de los analizadores basados en Roslyn, vuelva a poner el árbol de origen en un estado que puede compilarse.

**"csc. exe" se cerró con el código de error 1: no se puede crear una instancia del analizador AAAA desde C:\BBBB.dll: No se puede cargar el archivo o ensamblado 'Microsoft.CodeAnalysis, Version=X.X.X.X, Culture=neutral, PublicKeyToken=31bf3856ad364e35' o alguna de sus dependencias. El sistema no encuentra el archivo especificado.**

Asegúrese de que el compilador admite los analizadores basados en Roslyn. "csc.exe/version" debe devolver como mínimo la versión 2.6.x. En algunos casos, los archivos. csproj individuales pueden invalidar la instalación de Visual Studio de la máquina de compilación al hacer referencia a un paquete de Microsoft.Net.Compilers. Si no quiere usar una versión específica del compilador, quite las referencias a Microsoft.Net.Compilers. De lo contrario, asegúrese de que el paquete al que se hace referencia también use como mínimo la versión 2.6.x. Intente obtener el registro de errores, que puede encontrarse en el parámetro /errorlog: desde la línea de comandos de csc.exe (que se encuentra en el registro de la tarea de compilación de Roslyn). Puede tener un aspecto similar a /errorlog:F:\ts-services-123\_work\456\s\Some\Project\Code\Code.csproj.sarif

**El compilador de C# no es lo suficientemente reciente (debe ser una versión posterior a la 2.6)**

Las versiones más recientes del compilador de C# se publican aquí: https://www.nuget.org/packages/Microsoft.Net.Compilers. Para consultar la versión instalada, está ejecutando `C:\>csc.exe /version` desde el símbolo del sistema. Asegúrese de que no tiene ninguna referencia a un paquete de NuGet de Microsoft.Net.Compilers con una versión anterior a la 2.6.

**No se encuentran los registros de MSBuild/VSBuild**

Debido a la forma en que funciona la tarea, esta necesita consultar a Azure DevOps para obtener el registro de MSBuild de la tarea de compilación de MSBuild. Si esta tarea se ejecuta inmediatamente después de la tarea de compilación de MSBuild, el registro no estará disponible todavía. Realice otras tareas de compilación, incluidas las tareas de compilación de SecDevTools (como Binskim, Antimalware Scan y otras), entre la tarea de compilación de MSBuild y la tarea de compilación de analizadores basados en Roslyn. 

## <a name="next-steps"></a>Pasos siguientes

Si necesita ayuda adicional, el soporte técnico para el Análisis de código de seguridad de Microsoft está disponible de lunes a viernes de las 9:00 a. m. a las 5:00 p. m. hora estándar del Pacífico

  - Incorporación: póngase en contacto con los administradores técnicos de cuentas para comenzar. 
  >[!NOTE] 
  >Si aún no tiene una asociación de soporte técnico de pago con Microsoft, o si dispone de una oferta de soporte técnico que no le permite comprar servicios del catálogo de Phoenix, visite nuestra [página principal de servicios de soporte técnico](https://www.microsoft.com/enterprise/services/support) para obtener más información.

  - Soporte técnico: envíe un correo electrónico a nuestro equipo de [soporte técnico para el Análisis de código de seguridad de Microsoft](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request)