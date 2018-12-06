---
title: Novedades de Azure Machine Learning Studio | Microsoft Docs
description: Nuevas características disponibles en Azure Machine Learning Studio.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.openlocfilehash: c93bb9a2fa7991df265b4767585d920c137a436d
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311150"
---
# <a name="whats-new-in-azure-machine-learning-studio"></a>Novedades de Azure Machine Learning Studio

## <a name="october-2018"></a>Octubre de 2018

El motor de lenguaje R en el módulo [Execute R Script](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script) (Ejecutar script R) ha agregado una nueva versión del entorno de ejecución de R, Microsoft R Open (MRO) 3.4.4. MRO 3.4.4 se basa en CRAN R 3.4.4 de código abierto y, por tanto, es compatible con paquetes que funcionan con esa versión de R. Puede encontrar más información sobre los paquetes de R admitidos en el artículo "[R Packages supported by Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/r-packages-supported-by-azure-machine-learning#bkmk_List)" (Paquetes de R admitidos por Azure Machine Learning Studio).

## <a name="march-2017"></a>Marzo de 2017 
Esta versión de las actualizaciones de Microsoft Azure Machine Learning proporciona la siguiente característica:

* Función dedicada para trabajos de BES de Azure Machine Learning

    El procesamiento de grupo de lote de Machine Learning emplea el servicio [Azure Batch](../../batch/batch-technical-overview.md) para proporcionar una escala administrada por el cliente para el servicio de ejecución de lotes de Azure Machine Learning. El procesamiento de grupo de lote le permite crear grupos de Azure Batch en los que puede enviar trabajos por lotes y hacer que se ejecuten de manera predecible.

    Para obtener más información, consulte [Servicio de Azure Batch para trabajos de Machine Learning](dedicated-capacity-for-bes-jobs.md).


## <a name="august-2016"></a>Agosto de 2016 
Esta versión de las actualizaciones de Microsoft Azure Machine Learning proporciona la siguiente característica:
* Ahora, los servicios web clásicos se pueden administrar en el nuevo portal [Servicios web Microsoft Azure Machine Learning](https://services.azureml.net/) donde pueden administrarse todos los aspectos de los servicios web.    
  * Dicho portal ofrece las [estadísticas de uso](manage-new-webservice.md) de los servicios web.
  * Simplifica las pruebas de llamadas de solicitud remota a Azure Machine Learning con datos de ejemplo.
  * Ofrece una nueva página de prueba de servicio de ejecución por lotes con datos de ejemplos e historial de envío de trabajos.
  * Facilita la administración de los puntos de conexión.

## <a name="july-2016"></a>Julio de 2016 
Esta versión de las actualizaciones de Microsoft Azure Machine Learning proporciona la siguiente característica:
* Los servicios web ahora se administran como recursos de Azure administrados a través de interfaces de [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) (ARM), con lo que se aportan las siguientes mejoras:
  * Nuevas [API de REST](https://msdn.microsoft.com/library/azure/Dn950030.aspx) para implementar y administrar los servicios web basados en Resource Manager
  * Nuevo portal [Servicios web Microsoft Azure Machine Learning](https://services.azureml.net/), donde pueden administrarse todos los aspectos de los servicios web
* Incorpora un nuevo modelo de implementación de servicios web en varias regiones y mediante suscripción que utiliza API basadas en Azure Resource Manager que emplean el proveedor de recursos de esta solución en los servicios web.
* Introduce nuevos [planes de precios](https://azure.microsoft.com/pricing/details/machine-learning/) y funcionalidades de administración de planes que utilizan el nuevo RP de Azure Resource Manager para las tareas de facturación.
  * Ahora puede [implementar el servicio web en varias regiones](how-to-deploy-to-multiple-regions.md) sin necesidad de crear una suscripción en cada región.
* Proporciona las [estadísticas de uso](manage-new-webservice.md)de los servicios web.
* Simplifica las pruebas de llamadas de solicitud remota a Azure Machine Learning con datos de ejemplo.
* Ofrece una nueva página de prueba de servicio de ejecución por lotes con datos de ejemplos e historial de envío de trabajos.

Además, Machine Learning Studio se ha actualizado para poder implementar según el modelo de servicios web nuevo o seguir implementando con el modelo de servicios web clásico. 

