---
title: Descripción de los modelos de objetos de Digital Twins y del grafo de inteligencia espacial | Microsoft Docs
description: Uso de Azure Digital Twins para crear modelos de relaciones entre personas, lugares y dispositivos
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 1c2068af510cb3733ce99a6ae7b40487a8c1a015
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323934"
---
# <a name="understanding-digital-twins-object-models-and-spatial-intelligence-graph"></a>Descripción de los modelos de objetos de Digital Twins y del grafo de inteligencia espacial

Azure Digital Twins es un servicio IoT de Azure que mejora las representaciones virtuales completas de entornos físicos y dispositivos, sensores y personas asociados. Mejora el desarrollo con la organización los conceptos específicos del dominio en modelos útiles que se encuentran en un grafo de inteligencia espacial. Estos conceptos modelan fielmente las relaciones y las interacciones entre personas, espacios y dispositivos.

Los _modelos de objetos de Digital Twins_ describen conceptos específicos de dominio, categorías y propiedades. Los modelos los predefinen los usuarios, que adaptan la solución a sus necesidades específicas. Combinados, estos modelos de objetos de Digital Twins predefinidos constituyen una _ontología_. La ontología de un edificio inteligente describiría regiones, locales, plantas, oficinas, zonas, salas de reuniones y salas de pensar. Una cuadrícula de energía describiría varias centrales eléctricas, subestaciones, recursos energéticos y clientes. Estos modelos de objetos de Digital Twins y las ontologías permiten la personalización de Azure Digital Twins para distintos escenarios y necesidades.

Con los _modelos de objetos de Digital Twins_ y la _ontología_ preparados, se puede rellenar un _grafo espacial_. Los grafos espaciales son representaciones virtuales de las muchas relaciones entre espacios, dispositivos y personas importantes para una solución de IoT. En el siguiente diagrama se muestra un ejemplo de un grafo espacial que usa una ontología de edificio inteligente.

![Creación de un grafo espacial con Digital Twins][1]

<a id="model" />

El grafo espacial reúne los espacios, los dispositivos, los sensores y los usuarios. Se vinculan entre sí de la misma manera que en el mundo real: el local 43 tiene cuatro plantas, cada una de ellas con varias zonas. Los usuarios se relacionan con las estaciones de trabajo y se les concede acceso a partes del grafo.  Por ejemplo, un administrador tendrá derechos para realizar cambios en el grafo espacial, mientras que un visitante solo podrá ver determinados datos del edificio.

## <a name="digital-twins-object-models"></a>Modelos de objetos de Digital Twins

Los modelos de objetos de Digital Twins admiten estas categorías principales de objetos:

- Los **espacios** son ubicaciones físicas o virtuales, como `Tenant`, `Customer`, `Region` o `Venue`.
- Los **dispositivos** son equipos físicos o virtuales, como `AwesomeCompany Device` o `Raspberry Pi 3`.
- Los **sensores** son objetos que detectan eventos, como `AwesomeCompany Temperature Sensor` o `AwesomeCompany Presence Sensor`.
- Los **usuarios** identifican a los ocupantes y sus características.

Otras categorías de objetos son:

- **Recursos**: conectados a un espacio, suelen representar recursos de Azure que van a utilizar los objetos del grafo espacial, como `IoTHub`.
- **Blobs**: conectados a los objetos (del tipo espacio, dispositivo, sensor y usuario), se usan como archivos con el tipo MIME y los metadatos, como `maps`, `pictures` o `manuals`.
- **Tipos extendidos**: enumeraciones extensibles que aumentan las entidades con características específicas, como `SpaceType` o `SpaceSubtype`.
- **Ontologías**: representan un conjunto de tipos extendidos, como `Default`, `Building`, `BACnet` o `EnergyGrid`.
- **Claves y valores de propiedad**: características personalizadas de los usuarios, los dispositivos, los sensores y los espacios. Se pueden usar además de las características integradas, por ejemplo, `DeltaProcessingRefreshTime` como clave y `10` como valor.
- **Roles**: conjuntos de permisos asignados a los usuarios y los dispositivos en el grafo espacial, como `Space Administrator`, `User Administrator` o `Device Administrator`.
- **Asignaciones de roles**: asociación entre un rol y un objeto en el grafo espacial, por ejemplo, conceder a un usuario o una entidad de servicio permiso para administrar un espacio en el grafo espacial.
- **Almacenes de claves de seguridad**: proporcionan las claves de seguridad para todos los dispositivos de la jerarquía bajo un objeto de espacio determinado con el fin de que el dispositivo se pueda comunicar con el servicio Digital Twins de manera segura.
- **Funciones definidas por el usuario** o **UDF**: permiten el procesamiento de los datos de telemetría del sensor personalizable dentro del grafo espacial. Por ejemplo, una UDF puede establecer un valor del sensor, realizar lógica personalizada en función de las lecturas de sensor y establecer la salida en un espacio, asociar metadatos a un espacio y enviar notificaciones cuando se cumplen las condiciones predefinidas. Actualmente, las UDF pueden escribirse en JavaScript.
- **Buscadores de coincidencias**: objetos que determinan qué UDF se ejecutará para un mensaje de telemetría determinado.
- **Puntos de conexión**: ubicaciones donde pueden enrutarse los mensajes de telemetría y los eventos de Digital Twins, como `Event Hub`, `Service Bus` o `Event Grid`.

<a id="graph" />

## <a name="spatial-intelligence-graph"></a>Grafo de inteligencia espacial

**Gráfico espacial**: grafo jerárquico de los espacios, los dispositivos y las personas que se definen en el **modelo de objetos de Digital Twins**. El grafo espacial es compatible con la _herencia_, el _filtrado_, el _recorrido_, el _escalado_ y la _extensibilidad_. Los usuarios pueden administrar el grafo espacial e interactuar con él con una colección de API REST (consulte a continuación).

El usuario que implementa una instancia del servicio Digital Twins en su suscripción se convierte en el administrador global del nodo raíz, lo cual le que concede automáticamente acceso completo a toda la estructura. A continuación, este usuario puede aprovisionar los espacios en el grafo mediante `Space` API. Se pueden aprovisionar dispositivos mediante `Device` API; sensores, mediante `Sensor` API; etc. También ofrecemos [herramientas de código abierto](https://github.com/Azure-Samples/digital-twins-samples-csharp) para aprovisionar el grafo en bloque.

La _herencia_ del grafo se aplica a los permisos y las propiedades que descienden de un nodo primario y a todos los nodos por debajo de este. Por ejemplo, cuando se asigna un rol a un usuario en un nodo determinado, el usuario tendrá los permisos de ese rol para el nodo especificado y todos los nodos debajo de él. Además, todos los nodos debajo de ese nodo determinado heredarán las claves de propiedad y el tipo extendido definidos para él.

El _filtrado_ de grafos permite a los usuarios restringir los resultados de la solicitud por identificador, nombre, tipo, subtipo, espacio primario, espacio asociado, tipo de datos del sensor, claves y valores de propiedad, traverse, minLevel, maxLevel y otros parámetros de filtro de OData.

El _recorrido_ del grafo permite a los usuarios recorrer el grafo espacial en toda su amplitud y profundidad. El grafo se puede recorrer en profundidad de arriba a abajo o de abajo a arriba con los parámetros de navegación `traverse`, `minLevel` y `maxLevel`. La amplitud del grafo se puede recorrer para obtener nodos secundarios directamente conectados a un espacio primario o a uno de los secundarios. Al consultar un objeto, podría obtener todos los objetos relacionados que tengan relación con ese objeto mediante el parámetro `includes` de las distintas versiones de GET API.

Azure Digital Twins garantiza la _escalabilidad_ del grafo para poder ocuparse de las cargas de trabajo del mundo real. Digital Twins puede usarse para representar grandes carteras de bienes inmuebles, infraestructura, dispositivos, sensores, telemetría y mucho más.

La _extensibilidad_ del grafo permite a los usuarios personalizar los modelos de objetos de Digital Twins subyacentes con nuevos tipos y ontologías. Los datos de Digital Twins también se pueden enriquecer con propiedades y valores extensibles.

### <a name="spatial-intelligence-graph-management-apis"></a>API de administración del grafo con inteligencia espacial

Una vez implementado Azure Digital Twins desde [Azure Portal](https://portal.azure.com), se genera automáticamente la dirección URL de [Swagger](https://swagger.io/tools/swagger-ui/) de las API de administración, que se muestran en la sección **Introducción** de Azure Portal en el siguiente formato:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Nombre del atributo personalizado | Reemplace por |
| --- | --- |
| `yourInstanceName` | El nombre de la instancia de Azure Digital Twins |
| `yourLocation` | La región de servidor en la que está hospedada la instancia |

 En la siguiente imagen se ve el formato de la dirección URL completo:

![API de administración del portal de Digital Twins][2]

Para más detalles sobre el uso de los grafos de inteligencia espacial, visite el preestreno de las API de administración de Azure Digital Twins.

> [!TIP]
> Se proporciona un preestreno de Swagger para demostrar el conjunto de características de la API.
> Se hospeda en [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Más información sobre [cómo usar Swagger](how-to-use-swagger.md).

Todas las llamadas a API se deben autenticar mediante [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Las API siguen las [convenciones de las directrices de API REST de Microsoft](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). La mayoría de las API que devuelven colecciones admiten opciones de consulta del sistema de [OData](http://www.odata.org/getting-started/basic-tutorial/#queryData).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la conectividad de los dispositivos y cómo enviar mensajes de telemetría al servicio Azure Digital Twins, lea [Entrada de datos de telemetría y conectividad de dispositivo](concepts-device-ingress.md).

Para más información sobre las limitaciones de la API de administración y las demás, lea el artículo sobre [limitaciones y administración de API de Azure Digital Twins](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
