---
title: 'Carga de datos con Azure Machine Learning Data Prep SDK: Python'
description: Aprenda a cargar datos con Azure Machine Learning Data Prep SDK. Puede cargar distintos tipos de datos de entrada, especificar los parámetros y tipos de archivos de datos o utilizar la funcionalidad de lectura inteligente de SDK para detectar automáticamente el tipo de archivo.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 11/20/2018
ms.openlocfilehash: 208d6958b56dafbfacc45ecb05a71c14ac024ab4
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309885"
---
# <a name="load-and-read-data-with-azure-machine-learning"></a>Carga y lectura de datos con Azure Machine Learning

En este artículo obtendrá información sobre los diferentes métodos de carga de datos mediante el [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk). El SDK es compatible con varias características de ingesta de datos, entre las que se incluyen:

* Opciones de carga desde varios tipos de archivos con inferencia de parámetros de análisis (codificación, separador, encabezados).
* Opciones de conversión de tipo usando la inferencia durante la carga de archivos.
* Compatibilidad con la conexión para MS SQL Server y Azure Data Lake Storage.

## <a name="load-text-line-data"></a>Cargar datos de línea de texto 

Para leer datos de texto simples en un flujo de datos, use `read_lines()` sin especificar los parámetros opcionales.

```python
dataflow = dprep.read_lines(path='./data/text_lines.txt')
dataflow.head(5)
```

||Línea|
|----|-----|
|0|Fecha \|\|  Temperatura mínima \|\|  Temperatura máxima|
|1|1-07-2015 \|\|  -4,1 \|\|  10,0|
|2|2-07-2015 \|\|  -0,8 \|\|  10,8|
|3|3-07-2015 \|\|  -7,0 \|\|  10,5|
|4|4-07-2015 \|\|  -5,5 \|\|  9,3|

Después de ingerir los datos, ejecute el siguiente código para convertir el objeto de flujo de datos en un marco de datos de Pandas.

```python
pandas_df = dataflow.to_pandas_dataframe()
```

## <a name="load-csv-data"></a>Cargar datos CSV

Al leer archivos delimitados, el entorno de ejecución subyacente puede inferir los parámetros de análisis (separador, codificación, si se deben usar los encabezados, etc.). Ejecute el siguiente código para intentar leer un archivo CSV especificando solo su ubicación.

```python
# SAS expires June 16th, 2019
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0||stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Hale County|10171002158| |
|2|ALABAMA|1|101710|Hale County|10171002162| |
|3|ALABAMA|1|101710|Hale County|10171002156| |
|4|ALABAMA|1|101710|Hale County|10171000588|2|

Para excluir las líneas durante la carga, defina el parámetro `skip_rows`. Este parámetro omitirá la carga de filas que descienden en el archivo CSV (usando para ello un índice basado en uno).

```python
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dataflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale County|10171002158|29|
|1|ALABAMA|1|101710|Hale County|10171002162|40 |
|2|ALABAMA|1|101710|Hale County|10171002156| 43|
|3|ALABAMA|1|101710|Hale County|10171000588|2|
|4|ALABAMA|1|101710|Hale County|10171000589|23 |

Ejecute el siguiente código para mostrar los tipos de datos de columna.

```python
dataflow.head(1).dtypes

stnam                     object
fipst                     object
leaid                     object
leanm10                   object
ncessch                   object
schnam10                  object
MAM_MTH00numvalid_1011    object
dtype: object
```

De forma predeterminada, el SDK de preparación de datos de Azure Machine Learning no cambia el tipo de datos. El origen de datos del que se lee es un archivo de texto, por lo que el SDK lee todos los valores como cadenas. En este ejemplo, las columnas numéricas se deben analizar como números. Establezca el parámetro `inference_arguments` en `InferenceArguments.current_culture()` para inferir y convertir automáticamente los tipos de columna durante la lectura del archivo.

```
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dataflow.head(1).dtypes

stnam                      object
fipst                     float64
leaid                     float64
leanm10                    object
ncessch                   float64
schnam10                   object
ALL_MTH00numvalid_1011    float64
dtype: object
```

Algunas de las columnas se detectaron correctamente como números y su tipo se establece en `float64`.

## <a name="use-excel-data"></a>Uso de datos de Excel

El SDK incluye una función `read_excel()` para cargar archivos de Excel. De forma predeterminada, la función cargará la primera hoja en el libro. Para definir una hoja específica para cargar, defina el parámetro `sheet_name` con el valor de cadena del nombre de la hoja.

```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dataflow.head(5)
```

||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|
|------|------|------|-----|------|-----|-------|----|-----|
|0|None|None|None|None|None|None|None|None|None|
|1|None|None|None|None|None|None|None|None|None|
|2|None|None|None|None|None|None|None|None|None|
|3|RANK|Título|Estudio|Worldwide|Domestic / %|Column1|Overseas / %|Column2|Year^|
|4|1|Avatar|Fox|2788|760,5|0,273|2027,5|0,727|2009^|5|

El resultado muestra que los datos en la segunda hoja tenían tres filas vacías antes de los encabezados. La función `read_excel()` contiene parámetros opcionales para omitir filas y usar encabezados. Ejecute el siguiente código para omitir las tres primeras filas y use la cuarta fila como encabezado.

```python
dataflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_header=True, skip_rows=3)
```

||RANK|Título|Estudio|Worldwide|Domestic / %|Column1|Overseas / %|Column2|Year^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760,5|0,273|2027,5|0,727|2009^|
|1|2|Titanic|Par.|2186,8|658,7|0,301|1528,1|0,699|1997^|
|2|3|Marvel's The Avengers|BV|1518,6|623,4|0,41|895,2|0,59|2012|
|3|4|Harry Potter and the Deathly Hallows Part 2|WB|1341,5|381|0,284|960,5|0,716|2011|
|4|5|Frozen|BV|1274,2|400,7|0,314|873,5|0,686|2013|

## <a name="load-fixed-width-data-files"></a>Cargar archivos de datos de ancho fijo

Para cargar archivos de ancho fijo, especifique una lista de desplazamientos de caracteres. La primera columna siempre se supone que comienza en el desplazamiento cero.

```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dataflow.head(5)
```

||010000|99999|BOGUS NORWAY|NO|NO_1|ENRS|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|BOGUS NORWAY|NO|NO|ENSO||||
|1|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|
|2|010013|99999|ROST|NO|NO|||||
|3|010014|99999|SOERSTOKKEN|NO|NO|ENSO|+59783|+005350|+00500|
|4|010015|99999|BRINGELAND|NO|NO|ENBL|+61383|+005867|+03270|

Para evitar la detección del encabezado y analizar los datos correctos, pase `PromoteHeadersMode.NONE` al parámetro `header`.

```python
dataflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)
```

||Column1|Column2|Column3|Column4|Column5|Column6|Column7|Column8|Column9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|BOGUS NORWAY|NO|NO_1|ENRS|Column7|Column8|Column9|
|1|010003|99999|BOGUS NORWAY|NO|NO|ENSO||||
|2|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|
|3|010013|99999|ROST|NO|NO|||||
|4|010014|99999|SOERSTOKKEN|NO|NO|ENSO|+59783|+005350|+00500|
|5|010015|99999|BRINGELAND|NO|NO|ENBL|+61383|+005867|+03270|

## <a name="load-sql-data"></a>Carga de datos de SQL

El SDK también puede cargar datos desde un origen de SQL. Actualmente solo se admite Microsoft SQL Server. Para leer datos de un servidor SQL server, cree un objeto `MSSQLDataSource` que contenga los parámetros de conexión. El parámetro de contraseña de `MSSQLDataSource` acepta un objeto `Secret`. Hay dos formas de compilar un objeto de secreto:

* Registre el secreto y su valor con el motor de ejecución. 
* Cree el secreto con solo un valor de `id` (lo que es útil si el valor del secreto ya está registrado en el entorno de ejecución) mediante `dprep.create_secret("[SECRET-ID]")`.

```python
secret = dprep.register_secret(value="[SECRET-PASSWORD]", id="[SECRET-ID]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=secret)
```

Después de crear un objeto de origen de datos, puede continuar y leer los datos del resultado de la consulta.

```python
dataflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dataflow.head(5)
```

||ProductID|NOMBRE|ProductNumber|Color|StandardCost|ListPrice|Tamaño|Peso|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|ThumbNailPhoto|ThumbnailPhotoFileName|rowguid|ModifiedDate|
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|0|680|HL Road Frame - Black, 58|FR-R92B-58|Negro|1059,3100|1431,50|58|1016,04|18|6|2002-06-01 00:00:00+00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461f-9069-55309d90ea7e|2008-03-11 |0:01:36.827000+00:00|
|1|706|HL Road Frame - Red, 58|FR-R92R-58|Rojo|1059,3100|1431,50|58|1016,04|18|6|2002-06-01 00:00:00+00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|2008-03-11 |10:01:36.827000+00:00|
|2|707|Sport-100 Helmet, Red|HL-U509-R|Rojo|13,0863|34,99|None|None|35|33|2005-07-01 00:00:00+00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|2008-03-11 |10:01:36.827000+00:00|
|3|708|Sport-100 Helmet, Black|HL-U509|Negro|13,0863|34,99|None|None|35|33|2005-07-01 00:00:00+00:00|None|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|a25a44fb-c2de-4268-958f-110b8d7621e2|2008-03-11 |10:01:36.827000+00:00|
|4|709|Mountain Bike Socks, M|SO-B909-M|Blanco|3,3963|9,50|M|None|27|18|2005-07-01 00:00:00+00:00|2006-06-30 00:00:00+00:00|None|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|18f95f47-1540-4e02-8f1f-cc1bcb6828d0|2008-03-11 |10:01:36.827000+00:00|

## <a name="use-azure-data-lake-storage"></a>Uso de Azure Data Lake Storage

Hay dos maneras de que el SDK pueda adquirir el token de OAuth necesario para acceder a Azure Data Lake Storage:

* Recuperar el token de acceso de una sesión reciente del inicio de sesión de la CLI de Azure del usuario
* Usar una entidad de servicio (SP) y un certificado como secreto

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Usar un token de acceso de una sesión de la CLI de Azure reciente

En la máquina local, ejecute el siguiente comando.

```azurecli
az login
az account show --query tenantId
dataflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dataflow.head(5) head
```

> [!NOTE] 
> Si su cuenta de usuario es miembro de más de un inquilino de Azure, deberá especificar el inquilino, con el formato de nombre de host de dirección URL de AAD.

### <a name="create-a-service-principal-with-the-azure-cli"></a>Creación de una entidad de servicio con la CLI de Azure

Use la CLI de Azure para crear una entidad de servicio y el certificado correspondiente. Esta entidad de servicio concreta está configurada con el rol `reader`, y su ámbito se ha reducido exclusivamente a la cuenta de Azure Data Lake Storage "dpreptestfiles".

```azurecli
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```

Este comando se emite `appId` y la ruta de acceso al archivo de certificado (que suele estar en la carpeta particular). El archivo .crt contiene el certificado público y la clave privada en formato PEM.

```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

Para configurar la lista de control de acceso del sistema de archivos de Azure Data Lake Storage, use el valor de objectId del usuario. En este ejemplo, se usa la entidad de servicio.

```azurecli
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```

Para configurar el acceso de `Read` y `Execute` para el sistema de archivos de Azure Data Lake Storage, debe configurar la lista de control de acceso de los archivos y las carpetas individualmente. Esto es así porque el modelo de la lista de control de acceso de HDFS subyacente no admite herencias. 

```azurecli
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r-x" --path /
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r--" --path /farmers-markets.csv
```

```
certThumbprint = 'C2:08:9D:9E:D1:74:FC:EB:E9:7E:63:96:37:1C:13:88:5E:B9:2C:84'
certificate = ''
with open('./data/adls-dpreptestfiles.crt', 'rt', encoding='utf-8') as crtFile:
    certificate = crtFile.read()

servicePrincipalAppId = "8dd38f34-1fcb-4ff9-accd-7cd60b757174"
```

### <a name="acquire-an-oauth-access-token"></a>Adquisición de un token de acceso de OAuth

Use el paquete `adal` (`pip install adal`) para crear un contexto de autenticación en el inquilino MSFT y adquirir un token de acceso de OAuth. En el caso de ADLS, el recurso de la solicitud del token debe ser para "https://datalake.azure.net", que es diferente de la mayoría de los demás recursos de Azure.

```python
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dataflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dataflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Website|street|city|County|
|----|------|-----|----|----|----|----|
|0|1012063|Caledonia Farmers Market Association - Danville|https://sites.google.com/site/caledoniafarmers... ||Danville|Caledonia|
|1|1011871|Stearns Homestead Farmers' Market|http://Stearnshomestead.com |6975 Ridge Road|Parma|Cuyahoga|
|2|1011878|100 Mile Market|http://www.pfcmarkets.com |507 Harrison St|Kalamazoo|Kalamazoo|
|3|1009364|106 S. Main Street Farmers Market|http://thetownofsixmile.wordpress.com/ |106 S. Main Street|Six Mile|||
|4|1010691|10th Street Community Farmers Market|http://agrimissouri.com/mo-grown/grodetail.php... |10th Street and Poplar|Lamar|Barton|
