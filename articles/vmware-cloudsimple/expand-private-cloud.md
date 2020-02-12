---
title: Expansión de la nube privada de Azure VMware Solutions (AVS)
description: Describe cómo expandir una nube privada de AVS existente para agregar capacidad en un clúster nuevo o existente.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3286b7537056a6c2f282533aa629ebbe47612690
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025306"
---
# <a name="expand-an-avs-private-cloud"></a>Expansión de una nube privada de AVS

AVS ofrece la flexibilidad para expandir dinámicamente una nube privada de AVS. Puede comenzar con una configuración más pequeña y, luego, expandir a medida que necesita más capacidad. O bien, puede crear una nube privada de AVS en función de las necesidades actuales y, luego, expandirla a medida que crezca el consumo.

Una nube privada de AVS se compone de uno o varios clústeres de vSphere. Cada clúster puede tener de 3 a 16 nodos. Al expandir una nube privada de AVS, agrega nodos al clúster existente o crea un nuevo clúster. Para expandir un clúster existente, los nodos adicionales deben ser del mismo tipo (SKU) que los nodos existentes. Para crear un nuevo clúster, los nodos pueden ser de otro tipo. Para más información sobre los límites de la nube privada de AVS, consulte la sección de límites en el artículo [Introducción a la nube privada de AVS](cloudsimple-private-cloud.md).

Una nube privada de AVS se crea con un **Centro de datos** predeterminado en vCenter. Cada centro de datos actúa como entidad de administración de nivel superior. Para un clúster nuevo, AVS brinda la opción de agregar el centro de datos existente o crear un nuevo centro de datos.

Como parte de la configuración del nuevo clúster, AVS configura la infraestructura de VMware. La configuración incluye los valores de almacenamiento de grupos de discos vSAN, alta disponibilidad de VMware y Distributed Resource Scheduler (DRS).

Una nube privada de AVS puede expandirse varias veces. Las expansiones pueden realizarse solo cuando se respetan los límites de nodo generales. Cada vez que expande una nube privada de AVS, agrega nodos al clúster existente o crea uno nuevo.

## <a name="before-you-begin"></a>Antes de empezar

Los nodos deben aprovisionarse antes de poder expandir la nube privada de AVS. Para más información sobre el aprovisionamiento de nodos, consulte el artículo [Aprovisionamiento de nodos para VMware Solution by AVS: Azure](create-nodes.md). Para crear un nuevo clúster, debe tener al menos tres nodos disponibles de la misma SKU.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="expand-an-avs-private-cloud"></a>Expansión de una nube privada de AVS

1. [Acceda al portal de AVS](access-cloudsimple-portal.md).

2. Abra la página **Recursos** y seleccione la nube privada de AVS que quiere expandir.

3. En la sección de resumen, haga clic en **Expand** (Expandir).

    ![Expansión de una nube privada de AVS](media/resources-expand-private-cloud.png)

4. Elija si quiere expandir el clúster existente o crear un nuevo clúster de vSphere. Cuando hace cambios, la información de resumen en la página se actualiza.

    * Para expandir el clúster existente, haga clic en **Expand existing cluster** (Expandir clúster existente). Seleccione el clúster que quiere expandir y escriba el número de nodos que va a agregar. Cada clúster puede tener un máximo de 16 nodos.
    * Para agregar un nuevo clúster, haga clic en **Create new cluster** (Crear nuevo clúster). Escriba un nombre para el clúster. Seleccione un centro de datos existente o escriba un nombre para crear un nuevo centro de datos. Elija el tipo de nodo. Puede elegir un tipo de nodo diferente al crear un nuevo clúster de vSphere, pero no al expandir un clúster de vSphere existente. Seleccione el número de nodos. Cada nuevo clúster debe tener al menos tres nodos.

    ![Expandir la nube privada de AVS: agregar nodos](media/resources-expand-private-cloud-add-nodes.png)

5. Haga clic en **Enviar** para expandir la nube privada de AVS.

## <a name="next-steps"></a>Pasos siguientes

* [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Más información sobre las [nubes privadas de AVS](cloudsimple-private-cloud.md)