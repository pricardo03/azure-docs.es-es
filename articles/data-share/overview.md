---
title: ¿Qué es Azure Data Share (versión preliminar)?
description: Introducción a Azure Data Share (versión preliminar)
author: joannapea
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 7d4e51ec9564bfb123cf73d9fe89d040f42fe650
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807540"
---
# <a name="what-is-azure-data-share-preview"></a>¿Qué es Azure Data Share (versión preliminar)?

En la actualidad, los datos se ven como un recurso estratégico clave que muchas organizaciones necesitan compartir de forma sencilla y segura con sus clientes y asociados. Hoy en día, los clientes tiene a sus disposición varias formas de hacerlo, entre las que se incluyen el FTP, el correo electrónico y las API, entre otras. Por eso, es fácil que las organizaciones pierdan el rastro de las personas y entidades con quienes han compartido sus datos. El uso compartido de datos a través de FTP o creando una infraestructura de API propia a menudo es caro de aprovisionar y administrar. El uso de estos métodos para compartir datos a gran escala lleva asociada una sobrecarga de administración. 

Muchas organizaciones necesitan ser capaces de asumir la responsabilidad de los datos que han compartido. Además de dicha responsabilidad, muchas organizaciones desean poder controlar, administrar y supervisar todos el uso compartido de sus datos de una manera sencilla. En el mundo actual, donde se espera que los datos sigan creciendo a un ritmo exponencial, las organizaciones necesitan una forma sencilla de compartir los macrodatos. Los clientes exigen que los datos estén totalmente actualizados para estar seguros de que son capaces de sacar conclusiones oportunas.

Azure Data Share, versión preliminar, permite a las organizaciones compartir datos de forma simple y segura con varios clientes y asociados. Con unos pocos clics, puede aprovisionar una nueva cuenta de Azure Data Share, agregar conjuntos de datos e invitar a sus clientes y asociados a Data Share. Los proveedores de datos siempre tienen el control de los datos que han compartido. Azure Data Share simplifica la administración y supervisión de los datos que se comparten, cuándo y por quién. 

Los proveedores de datos pueden seguir manteniendo el control de sus datos. Para ello, especifican las condiciones de uso de su instancia de Azure Data Share. Quien consuma estos datos debe aceptar dichas condiciones para poder recibirlos. Los proveedores de datos pueden especificar la frecuencia con la que los consumidores de sus datos reciben actualizaciones. El proveedor de datos puede revocar el acceso a las nuevas actualizaciones en cualquier momento. 

Azure Data Share le ayuda a mejorar las conclusiones, ya que facilita la combinación de datos de terceros, con el fin de enriquecer los escenarios de inteligencia artificial y análisis. Use fácilmente las eficacia de las herramientas de análisis de Azure para preparar, procesar y analizar los datos compartidos mediante Azure Data Share. 

## <a name="scenarios-for-azure-data-share"></a>Escenarios de Azure Data Share

Azure Data Share puede usarse en varios sectores diferentes. Por ejemplo, un minorista puede compartir datos recientes de un punto de ventas con sus proveedores. Mediante Azure Data Share, un minorista puede configure un recurso compartido de datos que contenga los datos de un punto de ventas para todos sus proveedores y compartir las ventas por hora o diariamente. 

Azure Data Share también se puede usar para establecer un mercado de datos para un sector concreto. Por ejemplo, una institución gubernamental o de investigación que comparte regularmente datos anónimos acerca del crecimiento de la población con terceros. 

Otro caso de uso de Azure Data Share consiste en establecer un consorcio de datos. Por ejemplo, varias instituciones de investigación diferentes puede compartir datos con un único cuerpo de confianza. Los datos se analizan, agregan o procesan mediante las herramientas de análisis de Azure y, después, se comparten los datos con las partes interesadas. 

## <a name="how-it-works"></a>Cómo funciona

Azure Data Share usa un enfoque de uso compartido basado en instantáneas donde los datos parten de la suscripción de Azure del proveedor de datos y llegan a la suscripción de Azure del consumidor de datos. Los proveedores de datos aprovisionan recursos compartidos de datos e invitan a ellos a los destinatarios. Los consumidores de datos reciben una invitación a su recurso compartido de datos a través de correo electrónico. Una vez que un consumidor de datos acepta la invitación, pueden desencadenar una instantánea completa de los datos que ha compartido con él. Estos datos se reciben en la cuenta de almacenamiento de los consumidores de los datos. Los consumidores de los datos pueden recibir actualizaciones regulares e incrementales de los datos compartidos con ellos, con el fin de que siempre tengan la versión más reciente de los datos. 

Los proveedores de datos pueden ofrecer a sus consumidores de datos actualizaciones incrementales de los datos compartidos con ellos a través de una programación de instantáneas. Las programaciones de instantáneas se ofrecen cada hora o diariamente. Cuando un consumidor de datos acepta y configura su recurso compartido de datos, puede suscribirse a una programación de instantáneas. Esto es útil en aquellos escenarios en los que los datos compartidos se actualizan de forma periódica y el consumidor de datos necesita los datos más actualizados. 

![flujo de recurso compartido de datos](media/data-share-flow.png)

Cuando un consumidor de datos acepta un recurso compartido de datos, puede recibir los datos en la cuenta de almacenamiento que prefiera. Por ejemplo, si el proveedor de datos comparte datos mediante Azure Blob Storage, el consumidor de datos puede recibirlos en Azure Data Lake Store. 

## <a name="key-capabilities"></a>Principales capacidades

Azure Data Share permite a los proveedores de datos:

* Compartir datos desde Azure Storage y Azure Data Lake Store con clientes y asociados de fuera de una organización

* Realizar un seguimiento de los clientes y asociados con los que se han compartido datos

* Saber con qué frecuencia los consumidores de datos reciben actualizaciones de sus datos

* Permitir que los clientes extraigan la versión más reciente de los datos cuando la necesiten o que puedan recibir automáticamente cambios incrementales de los datos a intervalos que usted defina

Azure Data Share permite a los consumidores de datos: 

* Ver una descripción del tipo de datos que se comparte

* Ver las condiciones de uso de los datos

* Aceptar o rechazar una invitación de Azure Data Share

* Desencadenar una instantánea completa o incremental de una instancia de Azure Data Share que una organización ha compartido con usted

* Suscribirse a una instancia de Azure Data Share para recibir la copia más reciente de los datos a través de la copia incremental de instantáneas

* Aceptar los datos que se hayan compartido con usted en una cuenta de Azure Blob Storage o Azure Data Lake Gen2

Todas las funcionalidades claves mencionadas anteriormente se admiten a través de Azure o mediante las API REST. Para obtener más detalles acerca del uso de Azure Data Share a través de las API REST, consulte la documentación de referencia. 

## <a name="security"></a>Seguridad

Azure Data Share aprovecha la seguridad subyacente que ofrece Azure para proteger los datos tanto en reposo como en tránsito. Datos se cifran en reposo, cuando son compatibles con el mecanismo de almacenamiento subyacente. También se cifran en tránsito. Los metadatos de un recurso compartido de datos también se cifran tanto en reposo como en tránsito. 

Se pueden establecer controles de acceso en el nivel de recursos de Azure Data Share para asegurarse de que solo acceden los que tienen autorización. 

Azure Data Share aprovecha las identidades administradas para recursos de Azure (que antes se conocían como archivos MSI) para la administración automática de identidades en Azure Active Directory. Las identidades administradas para recursos de Azure se aprovechan para acceder a las cuentas de almacenamiento que se utilizan para el uso compartido de datos. No hay intercambio de credenciales entre un proveedor de datos y un consumidor de datos. Para más información, consulte la [página de identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities) (Identidades administradas para los recursos de Azure). 

## <a name="supported-regions"></a>Regiones admitidas

Para obtener una lista de regiones en las que Azure Data Share está disponible, consulte la página de [productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/) y busque Azure Data Share. 

Azure Data Share no almacena datos en sí mismo. Los datos se almacenan en las cuentas de almacenamiento subyacentes que se comparten. Por ejemplo, si un productor de datos almacena sus datos en una cuenta de Azure Data Lake Store que se encuentra la región Oeste de EE. UU., ahí es donde se almacenan los datos. Si comparte datos con una cuenta de Azure Storage ubicada en la región Oeste de Europa, los datos se transfieren directamente a la cuenta de Azure Storage que se encuentra en Oeste de Europa. 

No es preciso que el servicio Azure Data Share esté disponible en su región para aprovechar el servicio. Por ejemplo, si tiene datos almacenados en una cuenta de Azure Storage ubicada en una región en la que Azure Data Share aún no esté disponible, puede aprovechar el servicio para compartir los datos. 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).
