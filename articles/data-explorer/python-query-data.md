---
title: 'Inicio rápido: consulta de datos mediante la biblioteca de Python del Explorador de datos de Azure'
description: Con esta guía de inicio rápido, obtendrá información sobre cómo utilizar Python para consultar datos en el Explorador de datos de Azure.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: fee982812456548ed6d1e15d86151df88532389f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956105"
---
# <a name="quickstart-query-data-using-the-azure-data-explorer-python-library"></a>Inicio rápido: consulta de datos mediante la biblioteca de Python del Explorador de datos de Azure

El Explorador de datos de Azure es un servicio de exploración de datos altamente escalable y rápido para datos de telemetría y registro. El Explorador de datos de Azure proporciona un [biblioteca de cliente de datos para Python](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data). Esta biblioteca permite consultar los datos desde el código. En esta guía de inicio rápido, se conecta a una tabla en el *clúster de ayuda* que hemos configurado para facilitar el aprendizaje. A continuación, consulta una tabla en ese clúster, que devuelve los resultados.

Esta guía de inicio rápido también está disponible como un [Azure Notebook](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueryKusto.ipynb).

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de correo electrónico organizativa que sea miembro de Azure Active Directory (AAD)

* [Python](https://www.python.org/downloads/) instalado en el equipo de desarrollo

## <a name="install-the-data-library"></a>Instalar la biblioteca de datos

Instale *azure-kusto-data*.

```python
pip install azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Agregar instrucciones y constantes de importación

Importe clases de la biblioteca, así como *Pandas*, una biblioteca de análisis de datos.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
import pandas as pd
```

Para autenticar una aplicación, el Explorador de datos de Azure usa el identificador del inquilino AAD. Para buscar el identificador de inquilino, utilice la dirección URL siguiente y sustituya su dominio por *SuDominio*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Por ejemplo, si el dominio es *contoso.com*, la dirección URL es: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Haga clic en esta dirección URL para ver los resultados; la primera línea es como se indica a continuación. 

```
`"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"`
```

En este caso, el identificador de inquilino es `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. Establezca el valor de AAD_TENANT_ID antes de ejecutar este código.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_CLUSTER = "https://help.kusto.windows.net/"
KUSTO_DATABASE  = "Samples"
```

Ahora, cree la cadena de conexión. En este ejemplo se utiliza la autenticación de dispositivos para acceder al clúster. También puede usar el certificado de la aplicación de AAD, la clave de aplicación de AAD y el usuario y la contraseña de AAD.

```python
KCSB = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_CLUSTER)
KCSB.authority_id = AAD_TENANT_ID
```

## <a name="connect-to-azure-data-explorer-and-execute-a-query"></a>Conectarse al Explorador de datos de Azure y ejecutar una consulta

Ejecute una consulta en el clúster y almacene el resultado en un marco de datos. Cuando se ejecuta este código, devuelve un mensaje similar al siguiente: *Para iniciar sesión, use un explorador web para abrir la página https://microsoft.com/devicelogin y escriba el código F3W4VWZDM para autenticarse*. Siga los pasos para iniciar sesión y después vuelva a ejecutar el siguiente bloque de código.

```python
KUSTO_CLIENT  = KustoClient(KCSB)
KUSTO_QUERY  = "StormEvents | sort by StartTime desc | take 10"

df = KUSTO_CLIENT.execute_query(KUSTO_DATABASE, KUSTO_QUERY).primary_results[0].to_dataframe()
```

## <a name="explore-data-in-dataframe"></a>Exploración de los datos en DataFrame

Después de especificar un inicio de sesión, la consulta devuelve resultados, que se almacenan en un marco de datos. Puede trabajar con los resultados como hace con cualquier otro marco de datos.

```python
df
```

Debe ver los diez primeros resultados de la tabla StormEvents.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: introducción de datos mediante la biblioteca de Python del Explorador de datos de Azure](python-ingest-data.md)