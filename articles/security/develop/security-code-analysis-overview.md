---
title: Información general sobre la documentación de Análisis de código de seguridad de Microsoft Azure
description: Este artículo incluye información general sobre la extensión Análisis de código de seguridad
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
ms.openlocfilehash: 283d63bafc583f2ac9da3294644aaebd17d7c950
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718171"
---
# <a name="about-microsoft-security-code-analysis"></a>Acerca de Análisis de código de seguridad de Microsoft

La **extensión Análisis de código de seguridad de Microsoft** permite a los equipos integrar sin problemas el análisis de códigos de seguridad en sus canalizaciones de CI/CD de Azure DevOps, tal y como lo recomiendan los expertos del [Ciclo de vida de desarrollo de seguridad (SDL)](https://www.microsoft.com/securityengineering/sdl/practices) de Microsoft. La seguridad se simplifica a través de una experiencia de usuario coherente que abstrae las complejidades de tener que ejecutar diversas herramientas. Con una entrega de las herramientas basada en NuGet, los equipos ya no tienen que administrar la instalación ni actualización de las herramientas. Con las interfaces de línea de comandos y de tarea de compilación básica, todos los usuarios, desde los expertos en herramientas más experimentados hasta los desarrolladores cotidianos, pueden tener tanto o tan poco control sobre las herramientas como deseen. Los equipos también pueden aprovechar las eficaces capacidades de procesamiento posterior, como la publicación de registros para retención, la generación de informes procesables orientados a desarrolladores y la configuración de interrupciones de compilación en regresiones.

## <a name="why-microsoft-security-code-analysis"></a>Por qué el Análisis de código de seguridad de Microsoft

### <a name="security-simplified"></a>Seguridad simplificada

Agregar herramientas de análisis de códigos de seguridad a su canalización de Azure DevOps es tan sencillo como agregar nuevas tareas. Personalícelas o quédese con las predeterminadas. Las tareas se ejecutan como parte de la canalización de DevOps y generan registros que detallan todo tipo de conclusiones.

### <a name="clean-builds"></a>Compilaciones limpias

Después de solucionar los problemas iniciales informados por las herramientas, puede configurar la extensión para interrumpir las compilaciones en los nuevos problemas. Nunca fue tan fácil configurar la compilación de integración continua en cada solicitud de incorporación de cambios.

### <a name="set-it-and-forget-it"></a>Establézcalas y olvídese

Las herramientas y tareas de compilación se pueden establecer para que se mantengan actualizadas (y lo están de manera predeterminada). Si hay una versión actualizada de la herramienta, no es necesario descargarla e instalarla, esta extensión se encarga automáticamente. 

>>>
### <a name="under-the-hood"></a>Una mirada al interior

Las tareas de compilación de la extensión Análisis de código de seguridad de Microsoft abstraen las complejidades de:
  - Ejecutar herramientas de análisis estáticos de seguridad.
  - Procesar los resultados de los archivos de registro para crear un informe de resumen o interrumpir la compilación.
>>>

## <a name="security-code-analysis-toolset"></a>Conjunto de herramientas de Análisis de código de seguridad

La extensión Análisis de código de seguridad de Microsoft hace que tenga disponible de inmediato las versiones más recientes de herramientas de análisis importantes. La extensión incluye tanto herramientas internas de Microsoft como de código abierto. Las herramientas se descargan automáticamente en el agente hospedado en la nube una vez que usted configura y ejecuta la canalización con la tarea de compilación correspondiente. A continuación, encontrará el conjunto de herramientas que están disponibles en la extensión actualmente. Manténgase atento para conocer más y envíenos sus sugerencias de herramientas que podrían agregarse.

### <a name="anti-malware-scanner"></a>Anti-Malware Scanner

La tarea de compilación de Anti-Malware Scanner se incluye ahora en la extensión Análisis de código de seguridad de Microsoft. Debe ejecutarse en un agente de compilación que ya tenga instalado Windows Defender. Para obtener más información, visite el [sitio web de Defender](https://aka.ms/defender). 

### <a name="binskim"></a>BinSkim

BinSkim es un detector portátil ligero ejecutable (PE) que valida la configuración del compilador/vinculador y otras características binarias pertinentes para la seguridad. La tarea de compilación proporciona un contenedor de línea de comandos alrededor de la aplicación BinSkim.exe. BinSkim es una herramienta de código abierto y, para obtener más información, visite [BinSkim en GitHub](https://github.com/Microsoft/binskim).

### <a name="credential-scanner"></a>Credential Scanner

Las contraseñas y otros secretos almacenados en el código fuente actualmente son un problema importante. Credential Scanner es una herramienta de análisis estático de propiedad de Microsoft que detecta credenciales, secretos, certificados y otros contenidos confidenciales en el código fuente y en la salida de la compilación.

### <a name="microsoft-security-risk-detection"></a>Detección de riesgos de seguridad de Microsoft

Detección de riesgos de seguridad es el servicio de pruebas de vulnerabilidad ante datos aleatorios o inesperados basado en la nube exclusivo de Microsoft para identificar errores de seguridad infringibles en el software. Este servicio requiere un proceso de incorporación independiente. Para obtener más información, visite [MSRD en docs.microsoft.com](https://docs.microsoft.com/security-risk-detection/).

### <a name="roslyn-analyzers"></a>Analizadores basados en Roslyn

Herramienta de análisis estático integrada en el compilador de Microsoft para analizar código administrado (C# y VB). Para obtener más información, visite [Analizadores basados en Roslyn en docs.microsoft.com](https://docs.microsoft.com/dotnet/standard/analyzers/).

### <a name="tslint"></a>TSLint

TSLint es una herramienta de análisis estático extensible que comprueba el código TypeScript para mejorar la legibilidad, el mantenimiento y los errores de funcionalidad. Es ampliamente compatible con los editores y sistemas de compilación modernos, y se puede personalizar con sus propias reglas de lint, configuraciones y formateadores. TSLint es una herramienta de código abierto y, para obtener más información, visite [TSLint en GitHub](https://github.com/palantir/tslint).

## <a name="analysis-and-post-processing-of-results"></a>Análisis y procesamiento posterior de los resultados

La extensión Análisis de código de seguridad de Microsoft también tiene tres tareas útiles de procesamiento posterior para ayudarle a procesar y analizar los resultados encontrados por las tareas de las herramientas de seguridad. Normalmente se agregan en la canalización después de todas las demás tareas de las herramientas.

### <a name="publish-security-analysis-logs"></a>Publish Security Analysis Logs
La tarea de compilación Publish Security Analysis Logs conserva los archivos de registro de las herramientas de seguridad que se ejecutan durante la compilación para su investigación y seguimiento.

Se pueden publicar en los artefactos del servidor de Azure (como archivo ZIP) o copiar en un recurso compartido de archivos accesible desde el agente de compilación privado.

### <a name="security-report"></a>Security Report
La tarea de compilación Security Report analiza los archivos de registro creados por las herramientas de seguridad que se ejecutan durante la compilación y crea un único archivo de informe de resumen con todos los problemas detectados por las herramientas de análisis.

La tarea se puede configurar para que informe de los hallazgos de herramientas específicas o para todas las herramientas, aunque también puede elegir el nivel de problemas (errores o errores y advertencias) que deben notificarse.

### <a name="post-analysis-build-break"></a>Post-Analysis (interrupción de la compilación)
La tarea de compilación Post-Analysis permite al cliente insertar una interrupción de la compilación y hacer que la compilación fracase en caso de que una o más herramientas de análisis informen sobre hallazgos o problemas en el código.

La tarea se puede configurar para interrumpir la compilación por problemas encontrados por herramientas específicas o para todas las herramientas, y también en función de la gravedad de los problemas encontrados (errores o advertencias).

>[!NOTE]
>Las tareas de compilación individuales se completarán correctamente, por diseño, siempre y cuando la herramienta se complete correctamente, tanto si hay hallazgos como si no, para que la compilación pueda ejecutarse hasta su finalización, lo que permite que se ejecuten todas las herramientas.

## <a name="next-steps"></a>Pasos siguientes

Para obtener instrucciones sobre cómo incorporar e instalar Análisis de código de seguridad, consulte [nuestra guía de incorporación e instalación](security-code-analysis-onboard.md).

Para obtener más información sobre la configuración de las tareas de compilación, consulte nuestra [guía de configuración](security-code-analysis-customize.md).

Si tiene más preguntas sobre la extensión y las herramientas que se ofrecen,[ consulte nuestra página de preguntas más frecuentes](security-code-analysis-faq.md).