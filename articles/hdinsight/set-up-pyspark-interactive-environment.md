---
title: 'Herramientas de Azure HDInsight: configuración de un entorno interactivo de PySpark para Visual Studio Code'
description: Aprenda a usar Obtenga información sobre cómo usar Herramientas de Azure HDInsight para Visual Studio Code para crear y enviar consultas y scripts.
keywords: VScode,Herramientas de Azure HDInsight,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Hive interactivo,Consulta interactiva
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: bf47915ba93a4a3a7dec338395cfe0ce6aa3cdf6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993848"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Configuración del entorno interactivo de PySpark para Visual Studio Code

En los pasos siguientes se muestra cómo instalar paquetes de Python mediante la ejecución de **HDInsight: PySpark interactivo**.

## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>Configuración del entorno interactivo de PySpark en macOS y Linux
Si usa **python 3.x**, debe usar el comando **pip3** para los pasos siguientes:

1. Asegúrese de que **Python** y **pip** estén instalados.
 
    ![Versión de pip de Python](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Instale Jupyter.
    ```
    sudo pip install jupyter
    ```
   Es posible que vea el siguiente mensaje de error en Linux y macOS:

   ![Error 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Instale **libkrb5-dev** (solo para Linux). Puede aparecer el siguiente mensaje de error:

   ![Error 2](./media/set-up-pyspark-interactive-environment/error2.png)
       
   ```Resolve:
   sudo apt-get install libkrb5-dev 
   ```

3. Instale **sparkmagic**.
   ```
   sudo pip install sparkmagic
   ```

4. Asegúrese de que **ipywidgets** esté instalado correctamente, para lo que debe ejecutar lo siguiente:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Instale los kernels de contenedor.](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Instale los kernels de contenedor. Ejecute **pip show sparkmagic**. La salida muestra la ruta de acceso de la instalación de **sparkmagic**. 

    ![ubicación de sparkmagic](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Vaya a la ubicación y ejecute lo siguiente:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec install](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Compruebe el estado de la instalación.

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec list](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Para los kernels disponibles: 
    - **python2** y **pysparkkernel** corresponden a **python 2.x**. 
    - **python3** y **pyspark3kernel** corresponden a **python 3.x**. 

8. Reinicie Visual Studio Code y vuelva al editor de scripts que ejecuta **HDInsight: PySpark interactivo**.

## <a name="next-steps"></a>Pasos siguientes

### <a name="demo"></a>Demostración
* HDInsight para VS Code: [Vídeo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Herramientas y extensiones
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](hdinsight-for-vscode.md)
* [Uso de Azure Toolkit for IntelliJ para crear y enviar aplicaciones de Scala para Apache Spark](spark/apache-spark-intellij-tool-plugin.md)
* [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark mediante SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark mediante VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Uso de las herramientas de HDInsight de Azure Toolkit for Eclipse con el fin de crear aplicaciones Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Uso de las herramientas de HDInsight para IntelliJ con Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponibles para Jupyter Notebook en un clúster de Apache Spark para HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Uso de paquetes externos con cuadernos de Jupyter Notebook](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualización de datos de Apache Hive con Microsoft Power BI en Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](hdinsight-connect-hive-zeppelin.md)
