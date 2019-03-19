---
title: Configuración de gMSA para servicios de contenedor de Azure Service Fabric | Microsoft Docs
description: Aprenda ahora a configurar gMSA para un contenedor que se ejecuta en Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo, subramar
ms.openlocfilehash: 4ad697e01ef9e023232e2a2a16e4584a2779f84a
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806307"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Configuración de gMSA para contenedores de Windows que se ejecutan en Service Fabric

Para configurar la gMSA (cuentas de servicio administradas de grupo), se coloca un archivo de especificación de credenciales (`credspec`) en todos los nodos del clúster. El archivo se puede copiar en todos los nodos mediante una extensión de VM.  El archivo `credspec` debe contener la información de cuenta de gMSA. Para obtener más información sobre el archivo `credspec`, vea [Service Accounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts) (Cuentas de servicio). La especificación de credenciales y la etiqueta `Hostname` se especifican en el manifiesto de aplicación. La etiqueta `Hostname` debe coincidir con el nombre de cuenta de gMSA bajo la que se ejecuta el contenedor.  La etiqueta `Hostname` permite al contenedor autenticarse en otros servicios en el dominio mediante la autenticación Kerberos.  En el fragmento de código siguiente se muestra un ejemplo para especificar `Hostname` y `credspec` en el manifiesto de aplicación:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
A continuación, lea los siguientes artículos:

* [Implementación de un contenedor de Windows en Service fabric en Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementación de un contenedor de Docker en Service Fabric en Linux](service-fabric-get-started-containers-linux.md)
