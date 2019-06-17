---
title: Uso de Habitat para implementar una aplicación en Azure
description: Aprenda a implementar sistemáticamente la aplicación en máquinas virtuales y contenedores de Azure.
keywords: azure, chef, devops, máquinas virtuales, introducción, automatizar, habitat
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 2bdcd4c504822a2e60156b0ac565465e0cf23a85
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60388818"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Uso de Habitat para implementar una aplicación en Azure
[Habitat](https://www.habitat.sh/) es un sistema de entorno en tiempo de ejecución y empaquetado de aplicaciones que empaqueta la aplicación junto con su automatización como unidad de implementación. Esto crea la portabilidad definitiva de la aplicación, que permite su implementación en contenedores, máquinas virtuales, equipos sin sistema operativo o PaaS, sin necesidad de reescritura ni reempaquetado.

En este artículo se describen las principales ventajas de uso de Habitat.

## <a name="modernize-and-move-legacy-applications"></a>Modernización y traslado de aplicaciones heredadas
Libera las aplicaciones heredadas de middleware y sistemas operativos anteriores mediante su reempaquetado con Habitat. El artefacto resultante es portátil y redistribuye fácilmente las plataformas en infraestructuras más recientes, como máquinas virtuales o contenedores que se ejecutan en la nube.

## <a name="accelerate-container-adoption"></a>Aceleración de la adopción de contenedores
Habitat resuelve la implementación continua de aplicaciones complejas y orientadas a microservicios de forma precisa mediante la representación de dependencias en tiempo de ejecución. Deje atrás la simple implementación azul/verde de componentes individuales y diseñe un comportamiento de implementación sofisticado sin generar flujos de orquestación complejos.

## <a name="run-any-application-anywhere"></a>Ejecución de una aplicación en cualquier parte
Con Habitat, las aplicaciones pueden ejecutarse sin modificar en cualquier entorno de tiempo de ejecución. Aquí se incluyen desde máquinas virtuales y sin sistema operativo hasta contenedores (como Docker), sistemas de administración de clústeres (como Mesosphere o Kubernetes) y sistemas PaaS (como Pivotal Cloud Foundry).

## <a name="integrate-into-the-chef-devops-workflow"></a>Integración en el flujo de trabajo de Chef DevOps
Habitat es un proyecto de código abierto de Chef Software con una sólida comunidad de soporte técnico. Hábitat aprovecha la enorme experiencia de Chef con la automatización de la infraestructura para ofrecer funcionalidades de automatización sin precedentes para las aplicaciones. Chef ofrece compatibilidad comercial con Habitat y crea una perfecta integración entre Habitat y Chef Automate para automatizar completamente el ciclo de versión de aplicaciones, desde el desarrollo hasta la implementación.

## <a name="next-steps"></a>Pasos siguientes

* [Pruebe Habitat](https://www.habitat.sh/learn/)
