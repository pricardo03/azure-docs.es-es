---
title: 'Acerca de los recursos de Batch AI: Azure | Microsoft Docs'
description: Información general de las áreas de trabajo, clústeres, servidores de archivos, experimentos y trabajos en el servicio Batch AI en Microsoft Azure.
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
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 4a9e3529f9d68ecdc614ea69cffc6897891f4548
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44056957"
---
# <a name="overview-of-resources-in-batch-ai"></a>Información general de los recursos de Batch AI

Cuando empiece a usar el servicio Batch AI, querrá entender los recursos disponibles de dicho servicio. Al igual que con otros servicios de Azure, creará recursos de Batch AI en uno o más *grupos de recursos* de Azure. Cree una o varias *áreas de trabajo* de Batch AI en un grupo de recursos. Cada área de trabajo contiene una mezcla de *clústeres*, *servidores de archivos* y *experimentos* de Batch AI. Un experimento de Batch AI encapsula un grupo de *trabajos*.

En la imagen siguiente se muestra una jerarquía de recursos de ejemplo para Batch AI. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

En las secciones siguientes se detallan más a fondo los recursos de Batch AI.

## <a name="workspace"></a>Área de trabajo

Un área de trabajo de Batch AI es una colección de alto nivel del resto de recursos de Batch AI. Las áreas de trabajo ayudan a separar el trabajo en función de los diferentes grupos o proyectos. Por ejemplo, puede crear un área de trabajo para el desarrollo y otra para las pruebas.

## <a name="cluster"></a>Clúster

Un clúster de Batch AI contiene los recursos de proceso para ejecutar trabajos. Todos los nodos de un clúster tienen el mismo tamaño de máquina virtual e imagen del sistema operativo. Batch AI ofrece muchas opciones para crear clústeres que se personalizan para distintas necesidades. Por lo general, se configura un clúster diferente para cada categoría con la capacidad de procesamiento necesaria para completar un proyecto. Escale y reduzca los clústeres de Batch AI verticalmente según la demanda y el presupuesto. Para obtener más información, consulte [Work with Batch AI clusters](clusters.md) (Trabajo con clústeres de Batch AI).

## <a name="file-server"></a>Servidor de archivos

Opcionalmente, cree un servidor de archivos en Batch AI para almacenar datos, scripts de aprendizaje y registros de salida. Un servidor de archivos de Batch AI es un NFS administrado de un solo nodo que se pueden montar automáticamente en nodos del clúster para proporcionar una ubicación de almacenamiento centralmente accesible y sencilla para los trabajos. Para la mayoría de los casos, se necesita un solo servidor de archivos en un área de trabajo y se pueden separar los datos para los trabajos de aprendizaje en directorios distintos. Si NFS no es adecuado para las cargas de trabajo, Batch AI admite otras opciones de almacenamiento, como [Azure Storage](use-azure-storage.md), o soluciones personalizadas, como un sistema de archivos Gluster o Lustre.

## <a name="experiment"></a>Experimento

Un experimento agrupa una colección de trabajos relacionados que se consultan y administran conjuntamente. Cada área de trabajo podría tener varios experimentos, donde cada experimento intenta resolver un problema específico.

## <a name="job"></a>Trabajo

Un trabajo es una sola tarea o un script que se debe ejecutar, por ejemplo, para entrenar un modelo de aprendizaje profundo. Cada trabajo ejecuta un script específico en un clúster en el área de trabajo. (El script podría almacenarse en un servidor de archivos de Batch AI u otra solución de almacenamiento). Cada trabajo de Batch AI tiene un tipo de marco de trabajo asociado: TensorFlow, Horovod, CNTK, Caffe, Caffe2, pyTorch, Chainer, MPI personalizado o personalizado. Para cada marco, el servicio Batch AI configura la infraestructura necesaria y administra los procesos de trabajo. Cada experimento puede tener varios trabajos que son similares, aparte de algunos cambios en parámetros diferentes.

## <a name="next-steps"></a>Pasos siguientes

* Ejecute su primer [trabajo de aprendizaje de Batch AI](quickstart-tensorflow-training-cli.md).

* Consulte el ejemplo de [recetas de aprendizaje](https://github.com/Azure/BatchAI/tree/master/recipes) para diferentes plataformas.