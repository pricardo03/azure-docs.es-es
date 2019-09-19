---
title: Migración al recurso de Azure para la creación
titleSuffix: Azure Cognitive Services
description: Migre a una clave de recurso de creación de Azure.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/08/2019
ms.author: diberry
ms.openlocfilehash: 5828d4f453afeccfaac206195f064e43a20d1ba0
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70844775"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migración a una clave de creación de recursos de Azure

La autenticación de creación de Language Understanding (LUIS) cambió de una cuenta de correo electrónico a un recurso de Azure. 

## <a name="why-migrate"></a>¿Por qué migrar?

El uso de un recurso de Azure para la creación le permite, como propietario del recurso, controlar el acceso a la creación. Puede crear recursos de creación y asignarles nombres para administrar distintos grupos de autores. 

Por ejemplo, si está creando dos tipos de aplicaciones de LUIS, con distintos miembros, puede crear dos recursos de creación diferentes y asignar colaboradores. El recurso de creación de Azure controla la autorización. 

> [!Note]
> Antes de la migración, los coautores se conocen como _colaboradores_. Después de la migración, el rol de _colaborador_ de Azure se usa para la misma funcionalidad.

## <a name="what-is-migrating"></a>¿Qué es la migración?

La migración incluye:

* **Todas** las aplicaciones del propietario.
* Una migración **unidireccional**.

El propietario no puede elegir un subconjunto de las aplicaciones que se van a migrar y el proceso no es reversible. 

La migración no es: 

* Un proceso que recopila colaboradores y que se mueve o agrega automáticamente al recurso de creación de Azure. Como propietario de la aplicación, usted debe completar este paso. Este paso requiere permisos para el recurso adecuado.
* Proceso para crear y asignar un recurso de predicción en tiempo de ejecución. Si necesita un recurso de predicción en tiempo de ejecución, se trata de [un proceso independiente](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) que no se modifica. 

## <a name="how-are-the-apps-migrating"></a>¿Cómo se migran las aplicaciones?

El [portal de LUIS](https://www.luis.ai) proporciona el proceso de migración. 

Se le pedirá que realice la migración si:

* Tiene aplicaciones en el sistema de autenticación por correo electrónico para la creación.
* Y usted es el propietario de la aplicación. 

Puede retrasar el proceso de migración si cancela la ventana. Se le pedirá periódicamente que realice la migración hasta que migre o se pase la fecha límite de la migración. Puede iniciar el proceso de migración desde el icono de candado de la barra de navegación superior.

## <a name="migration-for-the-app-owner"></a>Migración para el propietario de la aplicación

### <a name="before-you-migrate"></a>Antes de la migración

* **De manera opcional**, haga una copia de seguridad de las aplicaciones desde la lista de aplicaciones del portal de LUIS; para ello, exporte cada aplicación o use la [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) de exportación.
* **Opcionalmente**, guarde la lista del colaborador de cada aplicación. Esta lista de correo electrónico se proporciona como parte del proceso de migración.
* **Obligatorio**, debe tener una [suscripción a Azure](https://azure.microsoft.com/free/). Una parte del proceso de suscripción requiere información de facturación. Sin embargo, puede usar el plan de tarifa gratis (`F0`) cuando use LUIS. 

**La creación de la aplicación de LUIS es gratuita**, lo que viene indicado por el nivel `F0`. [Más información sobre planes de tarifa](luis-boundaries.md#key-limits).

Si no tiene una suscripción a Azure, [suscríbase](https://azure.microsoft.com/free/). 

### <a name="migration-steps"></a>Pasos de migración

Siga [estos pasos de migración](luis-migration-authoring-steps.md).

### <a name="after-you-migrate"></a>Después de migrar 

Después del proceso de migración, todas las aplicaciones de LUIS están asignadas ahora a un único recurso de creación de LUIS.

Puede crear más recursos de creación y asignar desde la página **Administración -> Recursos de Azure** del _portal de LUIS_. 

Puede agregar colaboradores al recurso de creación desde _Azure Portal_, en la página **Access Control (IAM)** correspondiente a ese recurso. Consulte cómo [agregar acceso de un colaborador](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource) para más información.

|Portal|Propósito|
|--|--|
|[Las tablas de Azure](https://azure.microsoft.com/free/)|* Crear recursos de predicción y creación.<br>* Asignar colaboradores.|
|[LUIS](https://www.luis.ai)|* Migrar a recursos de creación nuevos.<br>* Asignar o anular la asignación de recursos de predicción y creación a las aplicaciones desde la página **Administrar -> Recursos de Azure**.| 

## <a name="migration-for-the-app-contributor"></a>Migración para el colaborador de la aplicación

### <a name="before-the-app-is-migrated"></a>Antes de migrar la aplicación

Puede optar por exportar una aplicación en la que sea colaborador y, a continuación, volver a importar la aplicación en LUIS. El proceso de importación crea una aplicación nueva con un nuevo identificador de aplicación, para el que usted es el propietario.

### <a name="after-the-app-is-migrated"></a>Después de migrar la aplicación

Después del proceso de migración, si necesita acceso a la aplicación original, es necesario que el propietario de la aplicación lo agregue al recurso de creación de Azure como colaborador.  

## <a name="next-steps"></a>Pasos siguientes

* [Migración de una aplicación a un recurso de creación](luis-migration-authoring-steps.md)