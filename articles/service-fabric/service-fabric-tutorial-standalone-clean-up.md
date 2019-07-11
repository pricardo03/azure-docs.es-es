---
title: 'Tutorial de limpieza del clúster de Service Fabric independiente: Azure Service Fabric | Microsoft Docs'
description: En este tutorial aprenderá a limpiar el clúster independiente.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 9127ad1fe148f85779913454adf6578a9a8a1055
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274089"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Tutorial: Limpieza del clúster independiente

Los clústeres de Service Fabric independientes ofrecen la opción de elegir un entorno propio y crear un clúster como parte del enfoque "Cualquier sistema operativo, cualquier nube" que toma Service Fabric. En esta serie de tutoriales se crea un clúster independiente hospedado en AWS o Azure y se instala en él una aplicación.

Este tutorial es la cuarta parte de una serie. Esta parte del tutorial muestra cómo limpiar los recursos de AWS o de Azure que creó para hospedar el clúster de Service Fabric.

En la parte número cuatro de la serie, se aprende a:

> [!div class="checklist"]
> * Limpiar un clúster de Service Fabric
> * Limpiar los recursos de AWS o de Azure.

## <a name="clean-up-service-fabric-cluster"></a>Limpiar un clúster de Service Fabric

1. Utilice RDP en la máquina virtual que usó para instalar Service Fabric.
2. Abra PowerShell.
3. Cambie el directorio a la carpeta extraída del segundo tutorial.
4. Ejecute el comando siguiente para eliminar el clúster de Service Fabric:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. Escriba `Y` cuando se le solicite, si se realizó correctamente, la salida tendrá un aspecto similar al siguiente, con sus propias direcciones IP:

  ```powershell
  Best Practices Analyzer completed successfully.
  Removing configuration from machine 172.31.21.141
  Removing configuration from machine 172.31.27.1
  Removing configuration from machine 172.31.20.163
  Configuration removed from machine 172.31.21.141
  Configuration removed from machine 172.31.27.1
  Configuration removed from machine 172.31.20.163
  The cluster is successfully removed.
  ```

## <a name="clean-up-aws-resources"></a>Limpieza de los recursos de AWS

1. Inicie sesión en la cuenta de AWS.
2. Vaya a la consola de EC2.
3. Seleccione los tres nodos que creó en la primera parte del tutorial.
4. Haga clic en **Actions** > **Instance State** > **Terminate** (Acciones > Estado de instancia > Terminar).

## <a name="clean-up-azure-resources"></a>Limpieza de los recursos de Azure

1. Inicie sesión en el Portal de Azure.
2. Vaya a la sección **Máquinas virtuales**.
3. Active las casillas de los tres nodos que creó en la primera parte del tutorial.
4. Haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la cuarta parte de la serie aprendió a limpiar los recursos que se crearon en los pasos anteriores.

> [!div class="checklist"]
> * Limpiar los recursos

> [!div class="nextstepaction"]
> [Volver al principio](service-fabric-tutorial-standalone-create-infrastructure.md)
