---
title: 'Tutorial: Escalado de una aplicación en Azure Spring Cloud | Microsoft Docs'
description: En este tutorial aprenderá a escalar una aplicación en Azure Spring Cloud desde Azure Portal
ms.service: spring-cloud
ms.topic: tutorial
ms.author: jeconnoc
author: jpconnock
ms.date: 10/06/2019
ms.openlocfilehash: cce7562c74577f6fd545bcaed3ee3e0968fd40b4
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132883"
---
# <a name="tutorial-scale-an-application-in-azure-spring-cloud"></a>Tutorial: Escalado de una aplicación en Azure Spring Cloud

En este tutorial se muestra cómo escalar cualquier aplicación de microservicios mediante el panel de Azure Spring Cloud de Azure Portal. Escale y reduzca verticalmente la aplicación al modificar el número de CPU virtuales (vCPU) y la cantidad de memoria. Escale y reduzca horizontalmente la aplicación al modificar el número de instancias de esta. Cuando haya terminado, sabrá cómo realizar ajustes manuales rápidos en cada aplicación del servicio. El escalado surte efecto en segundos y no requiere cambios en el código ni la reimplementación.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 
* Una instancia del servicio Azure Spring Cloud implementada.  Siga nuestro [inicio rápido](spring-cloud-quickstart-launch-app-cli.md) para comenzar.
* Al menos una aplicación ya creada en esa instancia de servicio.


## <a name="navigate-to-the-scale-page-in-the-azure-portal"></a>Vaya a la página Escalar de Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Vaya a la página de **Introducción a Azure Spring Cloud**.

1. Seleccione el grupo de recursos que contiene el servicio.

1. Vaya a la pestaña **Aplicaciones** del encabezado **Configuración** del menú del lado izquierdo.

1. Seleccione la aplicación que desea escalar. En este ejemplo se escalará la aplicación denominada "account-service". Esto le llevará a la página **Introducción** de la aplicación.

1. Vaya a la pestaña **Escalar** del encabezado **Configuración** del menú del lado izquierdo. Debería ver las opciones de los atributos de escalado que se muestran en la sección siguiente.

## <a name="scale-your-application"></a>Escalado de la aplicación

Puede modificar los atributos de escalado, pero tenga en cuenta lo siguiente.

* **CPU**: el número máximo de CPU permitido es 4 por instancia de aplicación. El número total de CPU de una aplicación será el valor establecido aquí multiplicado por el número de instancias de la aplicación.

* **Memoria/GB**: la cantidad máxima de memoria permitida es 8 GB por instancia de aplicación.  La cantidad total de memoria de una aplicación será el valor establecido aquí multiplicado por el número de instancias de la aplicación.

* **Recuento de instancias de aplicaciones**: Puede escalar horizontalmente hasta 20 instancias en el nivel estándar. Este valor cambia el número de instancias independientes en ejecución de la aplicación de microservicios.

Asegúrese de hacer clic en el botón **Guardar** para aplicar la configuración del escalado.

![Escalado de servicios en Azure Portal](media/spring-cloud-tutorial-scale-manual/scale-up-out.png)

Tras unos segundos se mostrarán los cambios realizados en la página **Introducción** con más detalles disponibles en la pestaña**Instancias de la aplicación**. El escalado no requiere cambios en el código ni la reimplementación.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a escalar manualmente las aplicaciones de Azure Spring Cloud.  Para aprender a supervisar la aplicación, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Aprenda a supervisar la aplicación](spring-cloud-tutorial-distributed-tracing.md).
