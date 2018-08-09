---
title: 'Prueba de código de ciencia de datos en Azure con el conjunto de datos de predicción de ingresos de adultos UCI: proceso de ciencia de datos en equipo y Visual Studio Team Services'
description: Prueba de código de ciencia de datos con datos de predicción de ingresos de adultos UCI
services: machine-learning, team-data-science-process
documentationcenter: ''
author: weig
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2018
ms.author: weig
ms.openlocfilehash: 46d156ce09b1ebcdcceb27ede6e7fa1595d30da6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439504"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Prueba de código de ciencia de datos con el conjunto de datos de predicción de ingresos de adultos UCI
En este artículo se proporcionan instrucciones preliminares para probar el código en un flujo de trabajo de ciencia de datos. Estas pruebas ofrecen a los científicos de datos una manera sistemática y eficiente de comprobar la calidad y el resultado esperado de su código. Se utiliza el [proyecto de proceso de ciencia de datos en equipo (TDSP), que utiliza el conjunto de datos de ingresos de adultos UCI,](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) que publicamos con anterioridad para mostrar cómo se pueden realizar pruebas de código. 

## <a name="introduction-on-code-testing"></a>Introducción a las pruebas de código
La "prueba unitaria" es una práctica antigua para el desarrollo de software. Pero para la ciencia de datos, a menudo no resulta claro lo que significa y cómo se debe probar el código para distintas fases de un ciclo de vida de ciencia de datos, tales como:

* Preparación de los datos
* Examen de calidad de datos
* Modelado
* Implementación del modelo 

En este artículo se reemplaza el término "prueba unitaria" por "prueba de código". Prueba se refiere a las funciones que ayudan a evaluar si el código para un determinado paso de un ciclo de vida de ciencia de datos produce resultados "según lo previsto". La persona que escribe la prueba define qué es "según lo previsto", dependiendo del resultado de la función (por ejemplo, control de la calidad de datos o modelado).

En este artículo se proporcionan referencias como recursos útiles.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Visual Studio Team Services para el marco de pruebas
En este artículo se describe cómo realizar y automatizar las pruebas mediante Visual Studio Team Services (VSTS). Puede optar por usar herramientas alternativas. También se muestra cómo configurar una compilación automática mediante VSTS y agentes de compilación. Para los agentes de compilación, usamos máquinas virtuales de ciencia de datos de Azure (DSVM).

## <a name="flow-of-code-testing"></a>Flujo de la prueba de código
El flujo de trabajo global de la prueba de código en un proyecto de ciencia de datos tiene el siguiente aspecto: 

![Gráfico del flujo de la prueba de código](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Pasos detallados

Use los pasos siguientes para configurar y ejecutar pruebas de código y una compilación automatizada mediante un agente de compilación y VSTS:

1. Cree un proyecto en la aplicación de escritorio de Visual Studio:

    ![Pantalla "Crear nuevo proyecto" en Visual Studio](./media/code-test/create_project.PNG)

   Después de crear el proyecto, podrá encontrarlo en el Explorador de soluciones en el panel derecho:
    
    ![Pasos para crear un proyecto](./media/code-test/create_python_project_in_vs.PNG)

    ![Explorador de soluciones](./media/code-test/solution_explorer_in_vs.PNG)

1. Introducir el código del proyecto en el repositorio de código de proyecto de VSTS: 

    ![Repositorio de código de proyecto](./media/code-test/create_repo.PNG)

1. Supongamos que ha hecho algún trabajo de preparación de datos, como la ingesta de datos, la ingeniería de características y la creación de columnas de etiqueta. Desea asegurarse de que el código genera los resultados esperados. Este es un código que puede usar para comprobar si el código de procesamiento de datos funciona correctamente:

    * Compruebe que los nombres de columna son correctos:
    
      ![Código para hacer coincidir los nombres de columna](./media/code-test/check_column_names.PNG)

    * Compruebe que los niveles de respuesta son correctos:

      ![Código para hacer coincidir los niveles](./media/code-test/check_response_levels.PNG)

    * Compruebe que el porcentaje de respuesta es razonable:

      ![Código para el porcentaje de respuesta](./media/code-test/check_response_percentage.PNG)

    * Compruebe el índice de ausencia de cada columna en los datos:
    
      ![Código para el índice de ausencia](./media/code-test/check_missing_rate.PNG)


1. Una vez que ha terminado el procesamiento de datos y el trabajo de ingeniería de características, y ha entrenado un buen modelo, asegúrese de que dicho modelo puede puntuar nuevos conjuntos de datos correctamente. Puede usar las dos pruebas siguientes para comprobar los niveles de predicción y la distribución de valores de etiqueta:

    * Compruebe los niveles de predicción:
    
      ![Código para comprobar los niveles de predicción](./media/code-test/check_prediction_levels.PNG)

    * Compruebe la distribución de los valores de predicción:

      ![Código para comprobar los valores de predicción](./media/code-test/check_prediction_values.PNG)

1. Coloque todas las funciones de prueba juntas en un script de Python denominado **test_funcs.py**:

    ![Script de Python para las funciones de prueba](./media/code-test/create_file_test_func.PNG)


1. Una vez preparados los códigos de prueba, puede configurar el entorno de pruebas en Visual Studio.

   Cree un archivo de Python llamado **test1.py**. En este archivo, cree una clase que incluya todas las pruebas que desea hacer. En el siguiente ejemplo se muestran seis pruebas preparadas:
    
    ![Archivo de Python con una lista de pruebas en una clase](./media/code-test/create_file_test1_class.PNG)

1. Esas pruebas se pueden detectar automáticamente si coloca **codetest.testCase** después del nombre de la clase. Abra el Explorador de pruebas en el panel derecho y seleccione **Ejecutar todo**. Todas las pruebas se ejecutarán secuencialmente y le indicarán si la prueba es correcta o no.

    ![Ejecución de las pruebas](./media/code-test/run_tests.PNG)

1. Inserte el código en el repositorio del proyecto mediante comandos de Git. El trabajo más reciente se verá reflejado en breve en VSTS.

    ![Comandos de Git para insertar código en el repositorio](./media/code-test/git_check_in.PNG)

    ![Trabajo más reciente en VSTS](./media/code-test/git_check_in_most_recent_work.PNG)

1. Configure la compilación automática y la prueba en VSTS:

    a. En el repositorio del proyecto, seleccione **Compilación y lanzamiento** y luego seleccione **+Nuevo** para crear un nuevo proceso de compilación.

       ![Selections for starting a new build process](./media/code-test/create_new_build.PNG)

    b. Siga las indicaciones para seleccionar la ubicación del código fuente, el nombre del proyecto, el repositorio y la información de rama.
    
       ![Source, name, repository, and branch information](./media/code-test/fill_in_build_info.PNG)

    c. Seleccione una plantilla. Dado que no hay ninguna plantilla de proyecto de Python, empiece seleccionando **Proceso vacío**. 

       ![List of templates and "Empty process" button](./media/code-test/start_empty_process_template.PNG)

    d. Asigne un nombre a la compilación y seleccione el agente. Puede elegir aquí el valor predeterminado si desea utilizar un DSVM para finalizar el proceso de compilación. Para más información acerca de los agentes de configuración, consulte [Build and release agents](https://docs.microsoft.com/vsts/build-release/concepts/agents/agents?view=vsts) (Agentes de compilación y lanzamiento).
    
       ![Build and agent selections](./media/code-test/select_agent.PNG)

    e. Seleccione **+** en el panel izquierdo, para agregar una tarea para esta fase de compilación. Dado que vamos a ejecutar el script de Python **test1.py** para terminar todas las comprobaciones, esta tarea utiliza un comando de PowerShell para ejecutar código de Python.
    
       !["Add tasks" pane with PowerShell selected](./media/code-test/add_task_powershell.PNG)

    f. En los detalles de PowerShell, rellene la información necesaria, como el nombre y la versión de PowerShell. Elija **Script en línea** como el tipo. 
    
       In the box under **Inline Script**, you can type **python test1.py**. Make sure the environment variable is set up correctly for Python. If you need a different version or kernel of Python, you can explicitly specify the path as shown in the figure: 
    
       ![PowerShell details](./media/code-test/powershell_scripts.PNG)

    g. Seleccione **Guardar y poner en cola** para finalizar el proceso de definición de compilación.

       !["Save & queue" button](./media/code-test/save_and_queue_build_definition.PNG)

Ahora, cada vez que se inserte una nueva confirmación en el repositorio de código, se iniciará automáticamente el proceso de compilación. (Aquí usamos maestro como el repositorio, pero puede definir cualquier rama). El proceso ejecuta el archivo **test1.py** en la máquina del agente para asegurarse de que todos los elementos definidos en el código se ejecutan correctamente. 

Si las alertas están configuradas correctamente, se le notificará por correo electrónico cuando finalice la compilación. También puede comprobar el estado de compilación en VSTS. Si se produce un error, puede comprobar los detalles de la compilación y averiguar qué parte se interrumpe.

![Notificación por correo electrónico del éxito de la compilación](./media/code-test/email_build_succeed.PNG)

![Notificación de VSTS del éxito de la compilación](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Pasos siguientes
* Consulte el [repositorio de predicción de ingresos UCI](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) para ejemplos concretos de pruebas unitarias para escenarios de ciencia de datos.
* Siga el esquema y los ejemplos anteriores del escenario de predicción de ingresos UCI en sus propios proyectos de ciencia de datos.

## <a name="references"></a>Referencias
* [Proceso de ciencia de datos en equipo (TDSP)](https://aka.ms/tdsp)
* [Herramientas de pruebas de Visual Studio](https://www.visualstudio.com/vs/features/testing-tools/)
* [Recursos de pruebas de VSTS](https://www.visualstudio.com/team-services/)
* [Máquinas virtuales de ciencia de datos](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)