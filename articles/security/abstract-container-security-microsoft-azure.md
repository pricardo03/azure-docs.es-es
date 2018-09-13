---
title: Descripción breve de la seguridad de contenedores en Microsoft Azure
description: Descripción breve de las notas del producto sobre la seguridad de contenedores en Microsoft Azure.
author: TomShinder
ms.author: TomSh
ms.date: 09/05/2018
ms.topic: article
ms.service: security
ms.openlocfilehash: 2ce3aec3b7a588076584ebdf400f8cafcdeb02fe
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842901"
---
# <a name="container-security-in-microsoft-azure"></a>Seguridad de contenedores en Microsoft Azure
## <a name="abstract"></a>Descripción breve

La tecnología de contenedores está provocando un cambio estructural en el mundo de la informática en nube. Los contenedores permiten ejecutar varias instancias de una aplicación en una sola instancia de un sistema operativo, por lo que los recursos se usan de forma más eficaz. Los contenedores proporcionan flexibilidad y coherencia a las organizaciones. Dado que la aplicación puede desarrollarse en un equipo de escritorio, probarse en una máquina virtual y, a continuación, implementarse para producción en la nube, permiten la implementación continua. Los contenedores proporcionan operaciones simplificadas, agilidad, escalabilidad y menores costos debido a la optimización de los recursos.

Dado que la tecnología de contenedor es relativamente nueva, muchos profesionales de TI tienen problemas de seguridad relativos a la falta de visibilidad y el uso en un entorno de producción. Los equipos de desarrollo a menudo no son conscientes de las prácticas recomendadas de seguridad. Estas notas del producto pueden ayudar a los equipos de operaciones de seguridad y a los desarrolladores en la selección de los enfoques para proteger el desarrollo y las implementaciones de los contenedores en la plataforma de Microsoft Azure.

Estas notas del producto describen los contenedores, la implementación y administración de contenedores, y los servicios de plataforma nativa. También describe los problemas de seguridad en tiempo de ejecución que se producen con el uso de contenedores en la plataforma de Azure. En las figuras y ejemplos, este documento se centra en Docker como modelo de contenedor y en Kubernetes como el orquestador de contenedores. La mayoría de las recomendaciones de seguridad también se aplica a otros modelos de contenedor de asociados de Microsoft en la plataforma de Azure.

[Descargar las notas del producto](https://azure.microsoft.com/mediahandler/files/resourcefiles/container-security-in-microsoft-azure/Open%20Container%20Security%20in%20Microsoft%20Azure.pdf)