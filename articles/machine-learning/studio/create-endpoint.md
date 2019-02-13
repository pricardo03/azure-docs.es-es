---
title: Creación de puntos de conexión de servicio Web
titleSuffix: Azure Machine Learning Studio
description: Crear punto de conexión de servicio web en Azure Machine Learning. Cada punto de conexión del servicio web se administra, limita y dirige de forma independiente.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 10/04/2016
ms.openlocfilehash: fc3a92aaf13f13682cfc56333618436ffe3d65ef
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55493378"
---
# <a name="creating-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Crear puntos de conexión para los servicios web de Azure Machine Learning Studio implementados
> [!NOTE]
>  En este tema se describen técnicas que se aplican a un servicio web Machine Learning **clásico**.
> 
> 

Al crear servicios web que puede vender a sus clientes, necesitará proporcionar modelos entrenados para cada cliente que aún siga vinculado al experimento desde el que se creó el servicio web. Además, las actualizaciones en el experimento pueden aplicarse de manera selectiva a un punto de conexión sin sobrescribir las personalizaciones.

Para ello, Azure Machine Learning Studio permite crear varios puntos de conexión en un servicio web implementado. Cada punto de conexión del servicio web se administra, limita y dirige de forma independiente. Cada punto de conexión es una dirección URL única y clave de autorización que se puede distribuir a sus clientes.



## <a name="adding-endpoints-to-a-web-service"></a>Incorporación de puntos de conexión al servicio web
Hay dos maneras de agregar un punto de conexión a un servicio web.

* De manera programática
* A través del portal de servicios web de Azure Machine Learning

Una vez creado el extremo, puede consumirlo a través de API sincrónicas, API de lotes y hojas de cálculo de Excel. Además de agregar extremos a través de esta interfaz de usuario, también puede usar las API de administración de extremos para agregar extremos mediante programación.

> [!NOTE]
> Si ha agregado más puntos de conexión al servicio web, no podrá eliminar el predeterminado.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Incorporación de un punto de conexión mediante programación
Puede agregar un punto de conexión al servicio web mediante programación utilizando el código de ejemplo [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Incorporación de un punto de conexión mediante el portal de servicios web de Azure Machine Learning
1. En Machine Learning Studio, en la columna de navegación izquierda, haga clic en Servicios web.
2. En la parte inferior del panel de servicios web, haga clic en **Manage endpoints**(Administrar puntos de conexión). El portal de servicios web de Azure Machine Learning se abre en la página de puntos de conexión del servicio web.
3. Haga clic en **Nuevo**.
4. Escriba un nombre y una descripción para el nuevo punto de conexión. Los nombres de los puntos de conexión deben tener 24 caracteres o menos y deben estar formados por letras en minúsculas o números. Seleccione el nivel de registro y si los datos de ejemplo están habilitados. Para obtener más información sobre el registro, consulte [Habilitación del registro para los servicios web Machine Learning](web-services-logging.md).

## <a name="next-steps"></a>Pasos siguientes
[Cómo consumir un servicio web Azure Machine Learning](consume-web-services.md)

