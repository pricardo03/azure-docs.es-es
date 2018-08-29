---
title: Cambios del registro de flujo de grupos de seguridad de red de Azure | Microsoft Docs
description: Obtenga información sobre los cambios del registro de flujo de grupos de seguridad de red de Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: b3a5ca929c83f70c31d667c2d9c811623691cff8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180671"
---
# <a name="network-security-group-flow-logs--version-2--upcoming-changes"></a>Registros de flujo de grupos de seguridad de red, versión 2: próximos cambios

El formato de los registros de flujo de grupos de seguridad de red cambiará a partir del 15 de septiembre de 2018. Los campos agregados ofrecen información sobre el estado del flujo y el número incremental de bytes y paquetes transferidos en cada dirección. Este cambio afectará a las aplicaciones que analizan los registros de flujo y, por tanto, necesitan actualizarse. En este artículo se describen los detalles del cambio.

## <a name="what-is-changing"></a>¿Qué está cambiando?

La versión de los registros de flujo de grupos de seguridad de red cambiarán de "Versión": 1 a "Versión": 2, con campos adicionales agregados a la propiedad *flowTuples* de los registros. Los detalles del formato se proporcionan en [How a flow is modeled in version 2](#how-is-a-flow-modeled-in-version-2) (Modelado de un flujo en la versión 2).

### <a name="version-1-flow-tuple-format"></a>Formato de tupla del flujo en la versión 1

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"
```

### <a name="version-2-flow-tuple-format"></a>Formato de tupla del flujo en la versión 2

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186"
```

## <a name="when-will-this-change-take-place"></a>¿Cuándo se llevará a cabo este cambio?

Este cambio entrará en vigor a partir del **15 de septiembre de 2018** en la región Centro-oeste de EE. UU. El lanzamiento del cambio en las regiones de la nube pública se completará el 31 de noviembre de 2018 y, a partir de entonces, solo estarán disponibles los registros de la versión 2.

## <a name="am-i-affected-by-the-change"></a>¿El cambio me afecta?

Este cambio puede afectarle si crea o usa una aplicación que procesa datos de registros de flujo de grupos de seguridad de red.

### <a name="if-i-use-traffic-analytics"></a>¿Y si uso Análisis de tráfico?

No. Análisis de tráfico no se verá afectado durante la transición de los registros de flujo de la versión 1 a la 2. Las próximas mejoras se beneficiarán de la información adicional sobre sesiones y ancho de banda proporcionada en los registros de flujo de la versión 2.

### <a name="if-im-using-third-party-tooling"></a>¿Y si uso herramientas de terceros?

Este cambio del formato de los registros se ha comunicado con antelación a todos los [asociados de Network Watcher](https://azure.microsoft.com/services/network-watcher). Póngase en contacto con su proveedor para obtener más información.

### <a name="if-i-have-built-a-custom-application-or-integration"></a>¿Y si he compilado una integración o aplicación personalizada?

**Sí**, deberá modificar la aplicación para procesar los registros de flujo de grupos de seguridad de red en el nuevo formato.

## <a name="what-is-the-new-flow-logging-format"></a>¿Cuál es el nuevo formato de los registros de flujo?

La versión 2 de los registros de flujo contiene tuplas de flujo con el siguiente formato:

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186".
```

En la tabla siguiente se proporcionan descripciones y nombres de propiedades de la tupla de flujo de la versión 2 del grupo de seguridad de red. Puede encontrar registros de ejemplo completos en el [evento de ejemplo de la versión 2](#version2sampleevent).

| Nombre de propiedad                        | Valor           | DESCRIPCIÓN                                                                                                                                                 |
| ------------------------------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Marca de tiempo                           | 1493763938      | Marca de tiempo correspondiente a la entrada de flujo. Se usa el formato UNIX EPOCH.                                                                                       |
| Dirección IP de origen                    | 185.170.185.105 |                                                                                                                                                             |
| Dirección IP de destino               | 10.2.0.4        |                                                                                                                                                             |
| Puerto de origen                          | 35370           |                                                                                                                                                             |
| Puerto de destino                     | 23              |                                                                                                                                                             |
| Protocolo de tráfico                     | T               | **T**: TCP y **U**: UDP.                                                                                                                                  |
| Dirección del flujo de tráfico               | I               | **I**: tráfico entrante y **O**: tráfico saliente.                                                                                                         |
| Decisión de tráfico                     | Una                | **A**: flujo permitido y **D**: flujo denegado.                                                                                                         |
| Estado del flujo                           | C               | **B**: inicio, cuando se crea el flujo. No se proporcionan las estadísticas. **C**: continuación de un flujo en curso. Las estadísticas se proporcionan a intervalos de cinco minutos. **E**: final, cuando termina el flujo. Se proporcionan las estadísticas.                                                                                                                                                        |
| Paquetes: del origen al destino      | 1               | El número total de paquetes TCP y UDP enviados desde el origen al destino desde la última actualización.                                                                  |
| Bytes enviados: del origen al destino   | 103             | El número total de bytes de paquetes TCP y UDP enviados desde el origen al destino desde la última actualización. Los bytes de paquete incluyen el encabezado y la carga del paquete.         |
| Paquetes enviados: del destino al origen | 1               | El número total de paquetes TCP y UDP enviados desde el destino al origen desde la última actualización.                                                                  |
| Bytes enviados: del destino al origen   | 186             | El número total de bytes de paquetes TCP y UDP enviados desde el destino al origen desde la última actualización. Los bytes de paquete incluyen el encabezado y la carga del paquete.             |

## <a name="how-is-a-flow-modeled-in-version-2"></a>¿Cómo se modela un flujo en la versión 2?

La versión 2 de los registros presenta el estado de flujo. El estado de flujo *B* se registra cuando se inicia un flujo. El estado de flujo *C* y el estado de flujo *E* son estados que marcan la continuación de un flujo y la terminación de este, respectivamente. Los estados *C* y *E* contienen información sobre el ancho de banda del tráfico.

**Ejemplo**: tuplas de flujo de una conversación TCP entre 185.170.185.105:35370 y 10.2.0.4:23:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,,,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

### <a name="how-does-the-format-differ-from-version-1"></a>¿En qué se diferencia con el formato de la versión 1?

En la versión 1, una tupla de flujo ["1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"] se creaba cada vez que se establece o intenta establecer un flujo (denegar caso).

### <a name="how-are-bytes-and-packets-accounted-for"></a>¿Cómo se tienen en cuenta los bytes y los paquetes?

Para los estados de continuación *C* y finalización *E*, los recuentos de paquetes y bytes son recuentos agregados desde el momento del registro de la tupla de flujo anterior. Con referencia a la conversación de ejemplo anterior, el número total de paquetes transferidos es 1021+52+8005+47 = 9125. El número total de bytes transferidos es 588096+29952+4610880+27072 = 5256000.

### <a name="if-my-application-doesnt-understand-the-traffic-bandwidth-fields-how-does-version-2-affect-the-application"></a>Si mi aplicación no reconoce los campos de ancho de banda del tráfico, ¿cómo afecta la versión 2 a la aplicación?

Las aplicaciones que usan los registros de flujo de grupos de seguridad de red de la versión 1 suelen contar el número de flujos únicos. Si no se produce ningún cambio en el análisis para tener en cuenta el estado de flujo, puede observar un aumento no preciso del número de flujos notificado. El recuento de flujos únicos puede realizarse ignorando las tuplas de flujo con los estados de flujo *C* y *E*.

## <a name="can-i-control-the-version-format-i-receive"></a>¿Puedo controlar el formato de versión que recibo?

No. Habilitar y deshabilitar los registros de flujo no afectará al formato de salida de los registros. El cambio de los registros de la versión 1 a la 2 se producirá región por región. Cuando una región actualiza de la versión 1 a la 2, puede ver registros de flujo de grupos de seguridad de red en ambos formatos. Una vez completado el lanzamiento, solo estarán disponibles los registros de la versión 2.

## <a name="while-the-change-occurs-can-i-see-both-formats-for-the-same-network-security-group"></a>Mientras se realiza el cambio, ¿puedo ver ambos formatos para el mismo grupo de seguridad de red?

Sí. Dentro de una región, la transición se producirá por cada dirección MAC. Como un grupo de seguridad de red se puede aplicar a muchas máquinas, puede que vea los registros en ambos formatos escritos en el almacenamiento. Los registros serán de la versión 1 o de la 2.

## <a name="will-i-see-duplicate-data"></a>¿Los datos aparecerán duplicados?

No se producirá la duplicación de los datos de registro de flujo en los formatos. Se registrará un flujo en el formato de la versión 1 o de la 2 mientras se realiza el cambio.

## <a name="how-can-i-test-the-new-format-ahead-of-time"></a>¿Cómo puedo probar el nuevo formato con antelación?

Sí. Puede [descargar](https://aka.ms/nsgflowlogsv2blobsample) un archivo de registro de flujo de ejemplo de la versión 2 a fin de utilizarlo para probar la solución.

## <a name="are-there-changes-to-configuration-and-management-of-network-security-group-flow-logging"></a>¿Hay algún cambio en la configuración y administración de registros de flujo de grupos de seguridad de red?

No. La compatibilidad con las cuentas de Azure Storage en las suscriptor que comparten el mismo inquilino de Azure Active Directory se [lanzó](https://azure.microsoft.com/blog/new-azure-network-watcher-integrations-and-network-security-group-flow-logging-updates/) a principios de este año. Este cambio ayuda a reducir el número de cuentas de almacenamiento necesarias para administrar los registros de flujo de grupos de seguridad de red.

## <a name="questions-or-feedback"></a>¿Tiene preguntas o comentarios?

Esperamos obtener sus comentarios sobre su experiencia con los registros de flujo de grupos de seguridad de red y Network Watcher. Puede proporcionar comentarios o sugerencias en línea o enviando un correo electrónico a AzureNetworkWatcher@microsoft.com.

## <a name="version-2-sample"></a>Ejemplo de la versión 2

```json
{

"records": [

{

"time": "2018-08-03T17:00:54.5657764Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315592,10.1.1.6,204.79.197.200,62375,80,T,O,A,B,,,,",

"1533315595,10.1.1.6,204.79.197.200,62373,80,T,O,A,E,30,1784,92,123631",

"1533315597,10.1.1.6,204.79.197.200,62376,80,T,O,A,B,,,,",

"1533315601,10.1.1.6,204.79.197.200,62374,80,T,O,A,E,13,866,87,123079",

"1533315603,10.1.1.6,204.79.197.200,62377,80,T,O,A,B,,,,",

"1533315604,10.1.1.6,204.79.197.200,62375,80,T,O,A,E,33,1946,90,123247",

"1533315608,10.1.1.6,204.79.197.200,62378,80,T,O,A,B,,,,",

"1533315610,10.1.1.6,204.79.197.200,62376,80,T,O,A,E,20,1244,92,123355",

"1533315613,10.1.1.6,204.79.197.200,62379,80,T,O,A,B,,,,",

"1533315616,10.1.1.6,204.79.197.200,62377,80,T,O,A,E,24,1460,92,123355",

"1533315619,10.1.1.6,204.79.197.200,62380,80,T,O,A,B,,,,",

"1533315622,10.1.1.6,204.79.197.200,62378,80,T,O,A,E,23,1406,93,123409",

"1533315624,10.1.1.6,204.79.197.200,62381,80,T,O,A,B,,,,",

"1533315628,10.1.1.6,204.79.197.200,62379,80,T,O,A,E,16,1028,88,123133",

"1533315630,10.1.1.6,204.79.197.200,62382,80,T,O,A,B,,,,",

"1533315631,10.1.1.6,204.79.197.200,62380,80,T,O,A,E,13,866,87,123079",

"1533315635,10.1.1.6,204.79.197.200,62384,80,T,O,A,B,,,,",

"1533315637,10.1.1.6,204.79.197.200,62381,80,T,O,A,E,15,974,86,123025",

"1533315640,10.1.1.6,204.79.197.200,62385,80,T,O,A,B,,,,",

"1533315643,10.1.1.6,204.79.197.200,62382,80,T,O,A,E,16,1028,88,123139",

"1533315646,10.1.1.6,204.79.197.200,62386,80,T,O,A,B,,,,",

"1533315649,10.1.1.6,204.79.197.200,62384,80,T,O,A,E,21,1298,92,123355",

"1533315651,10.1.1.6,204.79.197.200,62387,80,T,O,A,B,,,,"

]

}

]

}

]

}

},

{

"time": "2018-08-03T17:01:54.5668880Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_DenyAllInBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315685,80.211.83.37,10.1.1.6,45321,81,T,I,D,B,,,,"

]

}

]

},

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315655,10.1.1.6,204.79.197.200,62385,80,T,O,A,E,20,1244,87,123079",

"1533315657,10.1.1.6,204.79.197.200,62388,80,T,O,A,B,,,,",

"1533315658,10.1.1.6,204.79.197.200,62386,80,T,O,A,E,26,1568,92,123355",

"1533315662,10.1.1.6,204.79.197.200,62389,80,T,O,A,B,,,,",

"1533315664,10.1.1.6,204.79.197.200,62387,80,T,O,A,E,15,974,88,123133",

"1533315667,10.1.1.6,204.79.197.200,62390,80,T,O,A,B,,,,",

"1533315670,10.1.1.6,204.79.197.200,62388,80,T,O,A,E,18,1136,88,123139",

"1533315673,10.1.1.6,204.79.197.200,62391,80,T,O,A,B,,,,",

"1533315676,10.1.1.6,204.79.197.200,62389,80,T,O,A,E,14,920,87,123079",

"1533315678,10.1.1.6,204.79.197.200,62392,80,T,O,A,B,,,,",

"1533315679,10.1.1.6,204.79.197.200,62390,80,T,O,A,E,31,1838,94,123739",

"1533315684,10.1.1.6,204.79.197.200,62393,80,T,O,A,B,,,,",

"1533315685,10.1.1.6,204.79.197.200,62391,80,T,O,A,E,28,1676,101,141199",

"1533315689,10.1.1.6,204.79.197.200,62394,80,T,O,A,B,,,,",

"1533315691,10.1.1.6,204.79.197.200,62392,80,T,O,A,E,21,1298,93,123409",

"1533315694,10.1.1.6,204.79.197.200,62395,80,T,O,A,B,,,,",

"1533315697,10.1.1.6,204.79.197.200,62393,80,T,O,A,E,26,1568,91,123393",

"1533315700,10.1.1.6,204.79.197.200,62396,80,T,O,A,B,,,,",

"1533315703,10.1.1.6,204.79.197.200,62394,80,T,O,A,E,14,920,89,123187",

"1533315705,10.1.1.6,204.79.197.200,62397,80,T,O,A,B,,,,",

"1533315706,10.1.1.6,204.79.197.200,62395,80,T,O,A,E,13,866,87,123079",

"1533315711,10.1.1.6,204.79.197.200,62398,80,T,O,A,B,,,,"

]

}

]

}

]

}

}

]

}
```

## <a name="version-1-sample"></a>Ejemplo de la versión 1

```json
{ 

    "records": [ 

        { 

            "time": "2017-02-16T22:00:32.8950000Z", 

            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434", 

            "category": "NetworkSecurityGroupFlowEvent", 

            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG", 

            "operationName": "NetworkSecurityGroupFlowEvents", 

            "properties": { 

                "Version": 1, 

                "flows": [ 

                    { 

                        "rule": "DefaultRule_DenyAllInBound", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D" 

                                ] 

                            } 

                        ] 

                    }, 

                    { 

                        "rule": "UserRule_default-allow-rdp", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A", 

                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A", 

                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A", 

                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A" 

                                ] 

                            } 

                        ] 

                    } 

                ] 

            } 

        }, 

        { 

            "time": "2017-02-16T22:01:32.8960000Z", 

            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434", 

            "category": "NetworkSecurityGroupFlowEvent", 

            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG", 

            "operationName": "NetworkSecurityGroupFlowEvents", 

            "properties": { 

                "Version": 1, 

                "flows": [ 

                    { 

                        "rule": "DefaultRule_DenyAllInBound", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D" 

                                ] 

                            } 

                        ] 

                    }, 

                    { 

                        "rule": "UserRule_default-allow-rdp", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",

                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",

                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"

                                ] 

                            } 

                        ] 

                    } 

                ] 

            } 

        } 

    ] 
}
```