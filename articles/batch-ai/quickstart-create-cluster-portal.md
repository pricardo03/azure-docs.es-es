---
title: 'Guía de inicio rápido de Azure: creación de un clúster de Batch AI con Azure Portal | Microsoft Docs'
description: 'Guía de inicio rápido: creación de un clúster de Batch AI para el entrenamiento de modelos de aprendizaje automático e inteligencia artificial con Azure Portal'
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 78c743448a7f7439875d3598d6ba5d4eb6dc12fc
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408943"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-portal"></a>Inicio rápido: Creación de un clúster para trabajos de entrenamiento de Batch AI con Azure Portal

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

En esta guía de inicio rápido se muestra cómo usar Azure Portal para crear un clúster de Batch AI que sirve para el entrenamiento de modelos de aprendizaje automático e inteligencia artificial. Batch AI es un servicio administrado que permite a los científicos de datos y a los investigadores de la AI entrenar modelos de AI y aprendizaje automático a escala en clústeres de máquinas virtuales de Azure.

Inicialmente, el clúster tiene un único nodo con GPU y un servidor de archivos adjunto. Al completar esta guía de inicio rápido tendrá un clúster que podrá usar para entrenar modelos de aprendizaje profundo y que podrá escalar verticalmente. Envíe trabajos de entrenamiento al clúster con las herramientas de Batch AI o [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md), o [Visual Studio Tools for AI](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-ssh-key-pair"></a>Creación del par de claves SSH

Necesita un par de claves SSH para completar esta guía de inicio rápido. Si ya tiene un par de claves SSH, puede omitir este paso.

Para crear un par de claves SSH, ejecute el siguiente comando desde un shell de Bash y siga las instrucciones en pantalla. Por ejemplo, puede usar [Azure Cloud Shell](../cloud-shell/overview.md) o, en Windows, el [subsistema de Windows para Linux](/windows/wsl/install-win10). La salida del comando incluye el nombre del archivo de clave pública. Copie el contenido del archivo de clave pública (`cat ~/.ssh/id_rsa.pub`) en el portapapeles o en otra ubicación a la que pueda acceder en un paso posterior.

```bash
ssh-keygen -t rsa -b 2048
```

Para información más detallada sobre la creación de pares de claves SSH, consulte [Creación y uso de un par de claves SSH pública y privada para máquinas virtuales Linux en Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-batch-ai-workspace"></a>Creación de un área de trabajo de Batch AI

Empiece por crear un área de trabajo de Batch AI para organizar los recursos de Batch AI. Un área de trabajo puede contener uno o varios clústeres u otros recursos de Batch AI.

1. Seleccione **Todos los servicios** y filtre por **Batch AI**.

2. Seleccione **Agregar área de trabajo**.

3. Escriba los valores de **Nombre del área de trabajo** y **Grupo de recursos**. Si lo desea, seleccione distintas opciones de **Suscripción** y **Ubicación** para el área de trabajo. Seleccione **Crear área de trabajo**.

  ![Creación de un área de trabajo de Batch AI](./media/quickstart-create-cluster-portal/create-workspace.png)

Cuando aparezca el mensaje **Implementación correcta**, vaya al recurso que creó y seleccione el área de trabajo.

## <a name="create-a-file-server"></a>Creación de un servidor de archivos

Un servidor de archivos de Batch AI es un nodo NFS único que se puede montar automáticamente en los nodos del clúster. Es una de las maneras de proporcionar almacenamiento para los datos de entrada y salida de los trabajos de entrenamiento.

1. En el área de trabajo, seleccione **Servidor de archivos** > **Add batch ai file server** (Agregar servidor de archivos de Batch AI).

2. Especifique los valores para **Nombre del servidor de archivos** y **Tamaño de máquina virtual**. En esta guía de inicio rápido se sugiere un tamaño de máquina virtual *Estándar D1_v2* para el servidor de archivos. Elija un tamaño diferente si necesita almacenar mayores cantidades de datos de entrada o salida para los trabajos de entrenamiento.

3. Escriba un **Nombre de usuario de administrador** y copie el contenido de su archivo de clave pública SSH en **Clave SSH**. Acepte los valores predeterminados restantes y seleccione **Create File server** (Crear servidor de archivos).

  ![Creación del servidor de archivos de Batch AI](./media/quickstart-create-cluster-portal/create-file-server.png)

El servidor de archivos tarda unos minutos en implementarse.

Una vez creado el servidor, haga clic en **Propiedades** y tome nota de la **Configuración de montaje**. Puede acceder mediante SSH a la dirección IP pública del servidor para cargar y descargar datos de entrenamiento y archivos de salida en el directorio indicado (*/data*).

![Propiedades del servidor de archivos](./media/quickstart-create-cluster-portal/file-server-properties.png)

## <a name="create-a-cluster"></a>Creación de un clúster

Los siguientes pasos sirven para crear un clúster con un solo nodo con GPU. El nodo del clúster ejecuta una imagen de Ubuntu Server predeterminada diseñada para hospedar aplicaciones en contenedores que puede usar para la mayoría de las cargas de trabajo de entrenamiento. El nodo del clúster monta el servidor de archivos en su punto de montaje. 

1. En el área de trabajo de Batch AI, seleccione **Clúster** > **Add batch ai cluster** (Agregar clúster de Batch AI).

2. Especifique los valores de **Nombre del clúster** y la siguiente configuración. El tamaño de máquina virtual sugerido tiene una unidad GPU NVIDIA Tesla K80.
  
   |Configuración  |Valor  |
   |---------|---------|
   |**Tamaño de VM**     |NC6 estándar|
   |**Número objetivo de nodos**     |1|

3. Escriba un **Nombre de usuario de administrador** y copie el contenido de su archivo de clave pública SSH en **Clave SSH**. Acepte los valores predeterminados para los valores restantes de esta página y seleccione **Next: Node setup** (Siguiente: configurar el nodo).

   ![Incorporación de la información básica del clúster](./media/quickstart-create-cluster-portal/create-cluster.png)

4. En **Mount Volumes** (Volúmenes de montaje), seleccione **Referencias del servidor de archivos** > **Agregar**. Seleccione el servidor de archivos que creó anteriormente. Escriba una **Ruta de acceso de montaje relativa** del servidor de archivos para cada nodo del clúster. Seleccione **Guardar y continuar**.

   ![Incorporación de la referencia del servidor de archivos](./media/quickstart-create-cluster-portal/file-server-reference.png)

Guarde la configuración de nodo y seleccione **Crear clúster**.

Batch AI tarda unos minutos en asignar el nodo. Durante de este tiempo, el valor de **Estado de asignación** del clúster es **Cambio de tamaño**. Pocos minutos después, el estado del clúster es **Estable** y se inician los nodos.

![Inicio del clúster](./media/quickstart-create-cluster-portal/cluster-resizing.png)

Seleccione el nombre del clúster para comprobar el estado del nodo. Cuando el estado de un nodo es **Inactivo**, está preparado para ejecutar trabajos de entrenamiento.

### <a name="list-cluster-nodes"></a>Lista de nodos del clúster

Si necesita conectarse a los nodos del clúster (en este caso, un único nodo) para realizar tareas de mantenimiento o instalar aplicaciones, obtenga la información de conexión del portal. Una vez creado el clúster, haga clic en **Nodos** y anote la configuración de la **Conexión** SSH (dirección IP y número de puerto).

![Nodos de clúster](./media/quickstart-create-cluster-portal/cluster-nodes.png)

Utilice esta información para establecer una conexión SSH al nodo. Por ejemplo, sustituya la dirección IP y el número de puerto correctos del nodo en el siguiente comando:

```bash
ssh myusername@137.135.82.15 -p 50000
``` 

### <a name="resize-the-cluster"></a>Cambio de tamaño del clúster

Para usar el clúster para entrenar un modelo puede que necesite más recursos de proceso. Por ejemplo, para aumentar el tamaño a 2 nodos para un trabajo de entrenamiento distribuido, seleccione **Escalar** y establezca el **Número de destino de los nodos** en 2. Guarde la configuración.

![Escalar clúster](./media/quickstart-create-cluster-portal/scale-cluster.png)

El cambio de tamaño del clúster tarda unos minutos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los ejemplos y tutoriales de Batch AI, utilice el área de trabajo de Batch AI, el servidor de archivos y el clúster que se crearon en esta guía de inicio rápido.

Se le cobrará por el servidor de archivos y el clúster de Batch AI mientras se ejecuten las máquinas virtuales subyacentes, aunque no haya trabajos programados. Si desea conservar la configuración del clúster cuando no tenga trabajos para ejecutar, cambie el tamaño del clúster a 0 nodos. Más adelante, vuelva a cambiarlo de tamaño a 1 o más nodos para ejecutar los trabajos. 

Cuando ya no la necesite, elimine el área de trabajo de Batch AI que contiene el servidor de archivos y el clúster. Para ello, seleccione el área de trabajo de Batch AI y **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido ha aprendido a crear un clúster de Batch AI y un servidor de archivos adjunto mediante Azure Portal. Para información sobre cómo usar un clúster de Batch AI para entrenar un modelo, continúe con la guía de inicio rápido sobre el entrenamiento de un modelo de aprendizaje profundo.

> [!div class="nextstepaction"]
> [Entrenamiento de un modelo de aprendizaje profundo](./quickstart-tensorflow-training-cli.md)
