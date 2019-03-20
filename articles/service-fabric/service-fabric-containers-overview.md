---
title: Introducción a Service Fabric y los contenedores | Microsoft Docs
description: Información general sobre Service Fabric y el uso de contenedores para implementar aplicaciones de microservicios. Este artículo le proporciona información general de cómo se pueden utilizar los contenedores y las funcionalidades disponibles en Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/8/2018
ms.author: aljo, msfussell
ms.openlocfilehash: 5344f34e0e35d4d47b032b660726a4d70a4f1987
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807021"
---
# <a name="service-fabric-and-containers"></a>Service Fabric y contenedores

## <a name="introduction"></a>Introducción

Azure Service Fabric es una plataforma de sistemas distribuidos que facilita el empaquetado, la implementación y la administración de microservicios y contenedores escalables y confiables.

Service Fabric es el [orquestador de contenedores](service-fabric-cluster-resource-manager-introduction.md) de Microsoft que implementa microservicios en un clúster de máquinas. Service Fabric se beneficia de las lecciones aprendidas durante sus años de ejecución de servicios en Microsoft a gran escala.

Se pueden desarrollar microservicios de muchas maneras, desde usando los [modelos de programación de Service Fabric](service-fabric-choose-framework.md) o [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) hasta implementando [cualquier código que prefiera](service-fabric-guest-executables-introduction.md). O, si simplemente quiere [implementar y administrar contenedores](service-fabric-containers-overview.md), Service Fabric es también una buena opción.

De forma predeterminada Service Fabric permite implementar y activar estos servicios como procesos. Los procesos proporcionan la activación más rápida y el uso de densidad más alto de los recursos del clúster. Service Fabric puede implementar también servicios en imágenes de contenedor. También puede mezclar servicios en procesos y servicios en contenedores en la misma aplicación.

Para comenzar de inmediato y probar contenedores en Service Fabric, pruebe una guía de inicio rápido, tutorial o ejemplo:  

[Inicio rápido: Implementar una aplicación de contenedor de Linux en Service Fabric](service-fabric-quickstart-containers-linux.md)  
[Inicio rápido: Implementar una aplicación de contenedor de Windows en Service Fabric](service-fabric-quickstart-containers.md)  
[Inclusión de una aplicación .NET existente en un contenedor](service-fabric-host-app-in-a-container.md)  
[Ejemplos de contenedores de Service Fabric](https://azure.microsoft.com/resources/samples/service-fabric-containers/)  

## <a name="what-are-containers"></a>¿Qué son los contenedores?

Los contenedores vienen a solucionar el problema de ejecutar aplicaciones de forma fiable en distintos entornos de proceso proporcionando un entorno inmutable para que la aplicación se ejecute en él. Los contenedores encapsulan una aplicación y todas sus dependencias, como bibliotecas y archivos de configuración, en su propio "cuadro" aislado que contiene todo lo necesario para ejecutar el software en el contenedor. Siempre que se ejecuta el contenedor, la aplicación que está dentro de él dispone de todo lo necesario para ejecutarse como, por ejemplo, las versiones adecuadas de sus bibliotecas dependientes, todos los archivos de configuración y cualquier otra cosa que necesite para ejecutarse.

Los contenedores se ejecutan directamente en el kernel y tienen una vista aislada del sistema de archivos y de otros recursos. Una aplicación en un contenedor no tiene relación con ninguna otra aplicación o proceso fuera de su contenedor. Cada aplicación y su entorno en tiempo de ejecución, las dependencias y las bibliotecas del sistema se ejecutan dentro de un contenedor con acceso completo y privado a su propia vista aislada del sistema operativo. Además de proporcionar con facilidad todas las dependencias que la aplicación necesita para ejecutarse en diferentes entornos de proceso, la seguridad y el aislamiento de los recursos son ventajas importantes del uso de contenedores con Service Fabric que, de lo contrario, ejecutaría los servicios en un proceso.

En comparación con las máquinas virtuales, los contenedores ofrecen las siguientes ventajas:

* **Pequeño**: Contenedores usan un espacio de almacenamiento único y actualizaciones y las versiones de la capa para aumentar la eficacia.
* **Fast**: Los contenedores no tienen que arrancar un sistema operativo completo, para que puedan empezar con más rapidez, normalmente en segundos.
* **Portabilidad**: Para ejecutar en la nube o localmente, dentro de máquinas virtuales o directamente en las máquinas físicas, se puede portar una imagen de aplicación en contenedores.
* **Regulación de recursos**: Un contenedor puede limitar los recursos físicos que pueden consumir en el host.

### <a name="container-types-and-supported-environments"></a>Tipos de contenedor y entornos compatibles

Service Fabric admite contenedores en Linux y Windows, y admite el modo de aislamiento de Hyper-V en Windows.

#### <a name="docker-containers-on-linux"></a>Contenedores de Docker en Linux

Docker ofrece API para crear y administrar contenedores sobre contenedores de kernel de Linux. Docker Hub proporciona un repositorio central para almacenar y recuperar imágenes de contenedor.
Para ver un tutorial basado en Linux, consulte [Cree la primera aplicación de contenedor de Service Fabric en Linux](service-fabric-get-started-containers-linux.md).

#### <a name="windows-server-containers"></a>Contenedores de Windows Server

Windows Server 2016 proporciona dos tipos distintos de contenedores que difieren en el nivel de aislamiento que ofrecen. Los contenedores de Windows Server y los de Docker se parecen en que tienen un aislamiento del sistema de archivos y un espacio de nombres, pero comparten el kernel con el host en que se ejecutan. En Linux, este aislamiento tradicionalmente lo han proporcionado los cgroups y los espacios de nombres, y los contenedores de Windows Server se comportan de forma parecida.

Los contenedores de Windows compatibles con Hyper-V proporcionan mayor seguridad y aislamiento, ya que los contenedores no comparten el kernel del sistema operativo entre ellos ni con el host. Con este mayor nivel de aislamiento de seguridad, los contenedores habilitados para Hyper-V están diseñados para escenarios potencialmente hostiles y multiinquilino.
Para ver un tutorial basado en Windows, consulte [Cree la primera aplicación de contenedor de Service Fabric en Windows](service-fabric-get-started-containers.md).

La ilustración siguiente muestra los diferentes tipos de niveles de aislamiento y virtualización disponibles.
![Plataforma de Service Fabric][Image1]

## <a name="scenarios-for-using-containers"></a>Escenarios de uso de contenedores

Ejemplos típicos de buena elección de contenedor:

* **Levantamiento y movimiento de IIS**: Puede colocar una existente [ASP.NET MVC](https://www.asp.net/mvc) aplicación en un contenedor en lugar de migrar a ASP.NET Core. Estas aplicaciones de ASP.NET MVC dependen de Internet Information Services (IIS). Puede empaquetarlas en imágenes de contenedor a partir de la imagen de IIS creada previamente e implementarlas con Service Fabric. Consulte [Imágenes de contenedores en Windows Server](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server) para obtener información sobre los contenedores de Windows.

* **Combinación de contenedores y microservicios de Service Fabric**: Use una imagen de contenedor existente para parte de la aplicación. Por ejemplo, podría usar el [contenedor NGINX](https://hub.docker.com/_/nginx/) para el front-end web de su aplicación y los servicios con estado para la computación de back-end más intensa.

* **Reducir el impacto de los servicios de "vecinos ruidosos"**: Puede usar la capacidad de regulación de recursos de contenedores para restringir los recursos que utiliza un servicio en un host. Si hay servicios que consuman un gran número de recursos y afecten al rendimiento de otros (por ejemplo, una consulta de larga ejecución como operación), considere la posibilidad de poner estos servicios en contenedores con regulación de recursos.

## <a name="service-fabric-support-for-containers"></a>Compatibilidad de los contenedores con Service Fabric

Service Fabric admite la implementación de contenedores de Docker en Linux y de contenedores de Windows Server en Windows Server 2016, además de el modo de aislamiento de Hyper-V. 

Service Fabric proporciona un [modelo de aplicación](service-fabric-application-model.md)en el que un contenedor representa un host de la aplicación en el que se colocan varias réplicas de servicio. Service Fabric admite también un [escenario de archivo ejecutable invitado](service-fabric-guest-executables-introduction.md) en el que el usuario no usa los modelos de programación de Service Fabric integrados y, en su lugar, empaqueta una aplicación existente, escrita con cualquier lenguaje o plataforma, dentro de un contenedor. Este escenario es el caso habitual de los contenedores.

También puede ejecutar [servicios de Service Fabric dentro de un contenedor](service-fabric-services-inside-containers.md). La compatibilidad con la ejecución de servicios de Service Fabric dentro de contenedores está limitada actualmente.

Service Fabric ofrece varias funcionalidades de contenedor que le ayudarán a crear aplicaciones que se componen de microservicios en contenedores, como:

* Activación e implementación de la imagen de contenedor.
* Administración de recursos, incluida la configuración de valores de recursos predeterminados en clústeres de Azure.
* La autenticación de repositorios.
* La asignación de puerto a host de contenedor.
* La detección y comunicación entre contenedores.
* La capacidad de configurar y establecer variables de entorno.
* La capacidad de configurar credenciales de seguridad en el contenedor.
* Selección de diferentes modos de red para contenedores.

Para obtener una introducción completa sobre la compatibilidad de contenedores en Azure para, por ejemplo, crear un clúster de Kubernetes con Azure Kubernetes Service, crear un registro privado de Docker en Azure Container Registry, etc, consulte [Azure for Containers](https://docs.microsoft.com/azure/containers/).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la compatibilidad que ofrece Service Fabric para la ejecución de contenedores. A continuación, analizaremos algunos ejemplos de cada una de las características para mostrarle cómo utilizarlas.

[Cree la primera aplicación de contenedor de Service Fabric en Linux](service-fabric-get-started-containers-linux.md)  
[Cree la primera aplicación de contenedor en Service Fabric en Windows](service-fabric-get-started-containers.md)  
[Más información acerca de los contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png