---
title: Enrutamiento de vehículos eléctricos mediante Azure Notebooks (Python) | Microsoft Docs
description: Enrutamiento de vehículos eléctricos mediante las API de enrutamiento de Azure Maps y Azure Notebooks.
author: walsehgal
ms.author: v-musehg
ms.date: 10/01/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: c4b46bc952782fc7c9b56d6f0c049fe17b63d0f2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71836354"
---
# <a name="electric-vehicle-routing-using-azure-notebooks-python"></a>Enrutamiento de vehículos eléctricos mediante Azure Notebooks (Python)

Azure Maps es una cartera de API de servicios geoespaciales que se integran de forma nativa en Azure y que permiten a los desarrolladores, las empresas y los ISV crear aplicaciones con reconocimiento de ubicación y soluciones de seguimiento de recursos, IoT, movilidad y logística. Se puede llamar a las API REST de Azure Maps desde lenguajes como Python y R para habilitar el análisis de datos geoespaciales y los escenarios de aprendizaje automático. Azure Maps ofrece un sólido conjunto de [API de enrutamiento](https://docs.microsoft.com/rest/api/maps/route) que permite a los usuarios calcular rutas entre varios puntos de datos en función de diversas condiciones, como el tipo de vehículo o el área accesible. En este tutorial, le guiaremos por un escenario que ayudará al conductor de un vehículo eléctrico que tiene poca carga de batería a encontrar la estación de carga más cercana posible en función del tiempo de conducción.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Creación y ejecución de un cuaderno de Jupyter Notebook en [Azure Notebooks](https://docs.microsoft.com/azure/notebooks) en la nube
> * Llamada a varias API REST de Azure Maps en Python
> * Búsqueda de un intervalo accesible en función del modelo de consumo del vehículo eléctrico
> * Búsqueda de estaciones de carga de vehículos eléctricos en el intervalo accesible (o isócrono)
> * Representación del límite del intervalo accesible y de las estaciones de carga en un mapa
> * Búsqueda y visualización de la ruta a la estación de carga de vehículos eléctricos más cercana en función del tiempo


## <a name="prerequisites"></a>Requisitos previos 

Para realizar los pasos de este tutorial, primero debe crear una cuenta de Azure Maps y obtener la clave principal (clave de suscripción). Siga las instrucciones de [Administrar cuentas](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account) para crear una suscripción de cuenta de Azure Maps con el plan de tarifa S1 y siga los pasos de [Obtención de la clave principal](./tutorial-search-location.md#getkey) para obtener la clave de suscripción principal de la cuenta.

## <a name="create-an-azure-notebook"></a>Creación de un proyecto de Azure Notebook

Para seguir este tutorial, tendrá que crear un proyecto de Azure Notebook y descargar y ejecutar el archivo de cuaderno de Jupyter Notebook. El archivo de cuaderno contiene código Python que implementa el escenario de este tutorial. Siga los pasos que se indican a continuación para crear un proyecto de Azure Notebook y cargar el documento de Jupyter Notebook en él.

1. Vaya a [Azure Notebooks](https://notebooks.azure.com) e inicie sesión. Para más información, consulte [Inicio rápido](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks).
2. En la página del perfil público, seleccione **My Projects** (Mis proyectos) en la parte superior de la página.

    ![mi proyecto](./media/tutorial-ev-routing/myproject.png)

3. En la página **My Projects** (Mis proyectos), seleccione **New Project** (Nuevo proyecto).
 
   ![nuevo proyecto](./media/tutorial-ev-routing/create-project.png)

4. En el menú emergente **Create New Project** (Crear proyecto) que aparece, escriba la siguiente información y haga clic en **Create** (Crear):
    * Nombre de proyecto
    * Identificador del proyecto
 
    ![creación de proyecto](./media/tutorial-ev-routing/create-project-window.png)

5. Una vez creado el proyecto, descargue el [archivo de documento de Jupyter Notebook](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/EVrouting.ipynb) del [repositorio de Jupyter Notebook para Azure Maps](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook). 

6. Seleccione el proyecto en la lista de proyectos de la página **My Projects** (Mis proyectos) y haga clic en **Upload** (Cargar) para cargar el archivo de documento de Jupyter Notebook. Cargue el archivo del equipo y haga clic en **Done** (Listo).

    ![carga de cuaderno](./media/tutorial-ev-routing/upload-notebook.png)

7. Tras una carga correcta, verá el archivo en la página del proyecto. Haga clic en el archivo de cuaderno para abrirlo como un documento de Jupyter Notebook.

Para comprender mejor la funcionalidad implementada en el archivo de cuaderno, se recomienda ejecutar el código del cuaderno celda a celda. Para ejecutar el código de cada celda, haga clic en el botón **Run** (Ejecutar) situado en la parte superior de la aplicación Notebook.

  ![Ejecutar](./media/tutorial-ev-routing/run.png)

## <a name="install-project-level-packages"></a>Instalación de los paquetes de nivel de proyecto

Para ejecutar el código del cuaderno, tendrá que instalar paquetes en el nivel de proyecto. Siga los pasos que se indican a continuación para instalar los paquetes necesarios:

1. Descargue el archivo ["requirements.txt"](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/requirements.txt) del [repositorio de Jupyter Notebook para Azure Maps](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) y cárguelo en el proyecto.
2. En el panel del proyecto, seleccione **Project Settings** (Configuración del proyecto). 
3. En la ventana emergente que aparece, seleccione la pestaña **Environment** (Entorno) y, después, **Add** (Agregar).
4. En **Environment Setup Steps** (Pasos para la configuración del entorno): 
    * En el primer control desplegable, elija **Requirements.txt**.
    * En el segundo control desplegable, elija el archivo "requirements.txt".
    * En el tercer control desplegable, elija Python Version 3.6 (Python versión 3.6) como versión de Python.
7. Seleccione **Guardar**.

    ![instalación de paquetes](./media/tutorial-ev-routing/install-packages.png)

## <a name="load-required-modules-and-frameworks"></a>Carga de los módulos y marcos necesarios

Ejecute el siguiente script para cargar todos los módulos y marcos necesarios.

```python
import time
import aiohttp
import urllib.parse
from IPython.display import Image, display
```

## <a name="request-for-reachable-range-boundary"></a>Solicitud de límite del intervalo alcanzable

En nuestro escenario, una empresa de entrega de paquetes tiene algunos vehículos eléctricos en su flota. Durante el día, los vehículos eléctricos deben recargarse sin tener que volver al almacén. Cada vez que la carga restante actual del vehículo eléctrico alcanza para menos de una hora (el vehículo eléctrico tiene poca carga), hay que buscar un conjunto de estaciones de carga que estén dentro del intervalo accesible y obtener la información de límite para ese intervalo. Dado que la compañía prefiere el uso de rutas equilibradas por economía y velocidad, el valor de routeType solicitado es 'eco'. El script siguiente llama a [Get Route Range API](https://docs.microsoft.com/rest/api/maps/route/getrouterange) del servicio de enrutamiento de Azure Maps con los parámetros necesarios para indicar el modelo de consumo del vehículo y analiza la respuesta para crear un objeto poligonal con formato GeoJSON que representa el intervalo máximo accesible del automóvil.

Ejecute el script de la celda siguiente para obtener los límites del intervalo accesible del vehículo eléctrico.

```python
subscriptionKey = "Your Azure Maps primary subscription key"
currentLocation = [34.028115,-118.5184279]
session = aiohttp.ClientSession()

# Parameters for the vehicle consumption model 
travelMode = "car"
vehicleEngineType = "electric"
currentChargeInkWh=45
maxChargeInkWh=80
timeBudgetInSec=550
routeType="eco"
constantSpeedConsumptionInkWhPerHundredkm="50,8.2:130,21.3"


# Get bounds for the electric vehicle's reachable range.
routeRangeResponse = await (await session.get("https://atlas.microsoft.com/route/range/json?subscription-key={}&api-version=1.0&query={}&travelMode={}&vehicleEngineType={}&currentChargeInkWh={}&maxChargeInkWh={}&timeBudgetInSec={}&routeType={}&constantSpeedConsumptionInkWhPerHundredkm={}"
                                              .format(subscriptionKey,str(currentLocation[0])+","+str(currentLocation[1]),travelMode, vehicleEngineType, currentChargeInkWh, maxChargeInkWh, timeBudgetInSec, routeType, constantSpeedConsumptionInkWhPerHundredkm))).json()

polyBounds = routeRangeResponse["reachableRange"]["boundary"]

for i in range(len(polyBounds)):
    coordList = list(polyBounds[i].values())
    coordList[0], coordList[1] = coordList[1], coordList[0]
    polyBounds[i] = coordList

polyBounds.pop()
polyBounds.append(polyBounds[0])

boundsData = {
               "geometry": {
                 "type": "Polygon",
                 "coordinates": 
                   [
                      polyBounds
                   ]
                }
             }
```

## <a name="search-electric-vehicle-charging-stations-within-reachable-range"></a>Búsqueda de estaciones de carga de vehículos eléctricos en el intervalo accesible

Una vez que tenemos el intervalo accesible (isócrono) del vehículo eléctrico, podemos buscar las estaciones de carga en ese intervalo. El siguiente script llama a [Post Search Inside Geometry API](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) de Azure Maps para buscar estaciones de carga de vehículos eléctricos en los límites del intervalo máximo accesible del coche y, a continuación, analiza la respuesta en una matriz de ubicaciones accesibles.

Ejecute el siguiente script para buscar estaciones de carga de vehículos eléctricos dentro del intervalo accesible.

```python
# Search for EV stations within reachable range.
searchPolyResponse = await (await session.post(url = "https://atlas.microsoft.com/search/geometry/json?subscription-key={}&api-version=1.0&query=electric vehicle station&idxSet=POI&limit=50".format(subscriptionKey), json = boundsData)).json() 

reachableLocations = []
for loc in range(len(searchPolyResponse["results"])):
                location = list(searchPolyResponse["results"][loc]["position"].values())
                location[0], location[1] = location[1], location[0]
                reachableLocations.append(location)
```

## <a name="upload-reachable-range-and-charging-points-to-azure-maps-data-service"></a>Carga del intervalo accesible y de los puntos de carga en el servicio de datos de Azure Maps

Con el fin de visualizar las estaciones de carga y el límite para el intervalo máximo accesible del vehículo eléctrico en el mapa, es necesario cargar los datos del límite y de las estaciones de carga como objetos GeoJSON en el servicio de datos de Azure Maps, mediante [Data Upload API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview). 

Ejecute las dos celdas siguientes para cargar los datos del límite y los datos de puntos de carga en el servicio de datos de Azure Maps.

```python
rangeData = {
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          polyBounds
        ]
      }
    }
  ]
}

# Upload range data to Azure Maps data service.
uploadRangeResponse = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = rangeData)

rangeUdidRequest = uploadRangeResponse.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    getRangeUdid = await (await session.get(rangeUdidRequest)).json()
    if 'udid' in getRangeUdid:
        break
    else:
        time.sleep(0.2)
rangeUdid = getRangeUdid["udid"]
```

```python
poiData = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "properties": {},
        "geometry": {
            "type": "MultiPoint",
            "coordinates": reachableLocations
        }
    }
  ]
}

# Upload EV charging stations data to Azure Maps data service.
uploadPOIsResponse = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = poiData)

poiUdidRequest = uploadPOIsResponse.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    getPoiUdid = await (await session.get(poiUdidRequest)).json()
    if 'udid' in getPoiUdid:
        break
    else:
        time.sleep(0.2)
poiUdid = getPoiUdid["udid"]
```

## <a name="render-charging-stations-and-reachable-range-on-map"></a>Representación de estaciones de carga e intervalo accesible en el mapa

Una vez que se han cargado los datos en el servicio de datos, se ejecutará el siguiente script para llamar al [servicio Get Map Image](https://docs.microsoft.com/rest/api/maps/render/getmapimage) de Azure Maps y representar los puntos de carga y el límite máximo alcanzable en la imagen estática de mapa.

```python
# Get bounds for bounding box.
def getBounds(polyBounds):
    maxLon = max(map(lambda x: x[0], polyBounds))
    minLon = min(map(lambda x: x[0], polyBounds))

    maxLat = max(map(lambda x: x[1], polyBounds))
    minLat = min(map(lambda x: x[1], polyBounds))
    
    # Buffer the bounding box by 10% to account for the pixel size of pins at the ends of the route.
    lonBuffer = (maxLon-minLon)*0.1
    minLon -= lonBuffer
    maxLon += lonBuffer

    latBuffer = (maxLat-minLat)*0.1
    minLat -= latBuffer
    maxLat += latBuffer
    
    return [minLon, maxLon, minLat, maxLat]

minLon, maxLon, minLat, maxLat = getBounds(polyBounds)

path = "lcff3333|lw3|la0.80|fa0.35||udid-{}".format(rangeUdid)
pins = "custom|an15 53||udid-{}||https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/images/icons/ev_pin.png".format(poiUdid)

encodedPins = urllib.parse.quote(pins, safe='')

# Render range and EV charging points on the map.
staticMapResponse =  await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&pins={}&path={}&bbox={}&zoom=12".format(subscriptionKey,encodedPins,path,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

poiRangeMap = await staticMapResponse.content.read()

display(Image(poiRangeMap))
```

![intervalo de ubicaciones](./media/tutorial-ev-routing/location-range.png)


## <a name="find-the-optimal-charging-station-to-stop"></a>Búsqueda de la estación de carga óptima para detenerse

Una vez que tenemos todas las estaciones de carga posibles en el intervalo accesible, queremos saber cuál es la estación que se puede alcanzar en la mínima cantidad de tiempo. El siguiente script llama a [Matrix Routing API](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) de Azure Maps, que devuelve, para la ubicación indicada del vehículo, el tiempo de desplazamiento y la distancia a cada ubicación de estación de carga determinada. El script de la siguiente celda analiza la respuesta para obtener la ubicación de la estación de carga accesible más cercana en función del tiempo.

Ejecute la siguiente celda para encontrar la estación de carga accesible más cercana en la mínima cantidad de tiempo.

```python
locationData = {
            "origins": {
              "type": "MultiPoint",
              "coordinates": [[currentLocation[1],currentLocation[0]]]
            },
            "destinations": {
              "type": "MultiPoint",
              "coordinates": reachableLocations
            }
         }

# Get the travel time and distance to every given charging station location.
searchPolyRes = await (await session.post(url = "https://atlas.microsoft.com/route/matrix/json?subscription-key={}&api-version=1.0&routeType=shortest&waitForResults=true".format(subscriptionKey), json = locationData)).json()

distances = []
for dist in range(len(reachableLocations)):
    distances.append(searchPolyRes["matrix"][0][dist]["response"]["routeSummary"]["travelTimeInSeconds"])

minDistLoc = []
minDistIndex = distances.index(min(distances))
minDistLoc.extend([reachableLocations[minDistIndex][1], reachableLocations[minDistIndex][0]])
closestChargeLoc = ",".join(str(i) for i in minDistLoc)
```

## <a name="calculate-route-to-the-closest-charging-station"></a>Cálculo de la ruta a la estación de carga más cercana

Ahora que hemos encontrado la estación de carga más cercana, llamaremos a [Get Route Directions API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) para solicitar la ruta detallada desde la ubicación actual del vehículo eléctrico a la estación de carga.

Ejecute el script de la siguiente celda, que obtiene la ruta y analiza la respuesta para crear un objeto GeoJSON que representa esa ruta.

```python
# Get route from current location to the closest charging station. 
routeResponse = await (await session.get("https://atlas.microsoft.com/route/directions/json?subscription-key={}&api-version=1.0&query={}:{}".format(subscriptionKey, str(currentLocation[0])+","+str(currentLocation[1]), closestChargeLoc))).json()

route = []
for loc in range(len(routeResponse["routes"][0]["legs"][0]["points"])):
                location = list(routeResponse["routes"][0]["legs"][0]["points"][loc].values())
                location[0], location[1] = location[1], location[0]
                route.append(location)

routeData = {
         "type": "LineString",
         "coordinates": route
     }
```

## <a name="visualize-the-route"></a>Visualización de la ruta

Para visualizar la ruta, primero cargaremos los datos de ruta como un objeto GeoJSON en el servicio de datos de Azure Maps mediante [Data Upload API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) de Azure Maps. A continuación, llamaremos al servicio de representación, [Get Map Image API](https://docs.microsoft.com/rest/api/maps/render/getmapimage), para representar la ruta en el mapa y visualizarla.

Ejecute el siguiente script para obtener una imagen de la ruta representada en el mapa.

```python
# Upload route data to Azure data service.
routeUploadRequest = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = routeData)

udidRequestURI = routeUploadRequest.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    udidRequest = await (await session.get(udidRequestURI)).json()
    if 'udid' in udidRequest:
        break
    else:
        time.sleep(0.2)

udid = udidRequest["udid"]

destination = route[-1]

destination[1], destination[0] = destination[0], destination[1]

path = "lc0f6dd9|lw6||udid-{}".format(udid)
pins = "default|codb1818||{} {}|{} {}".format(str(currentLocation[1]),str(currentLocation[0]),destination[1],destination[0])


# Get bounds for bounding box.
minLat, maxLat = (float(destination[0]),currentLocation[0]) if float(destination[0])<currentLocation[0] else (currentLocation[0], float(destination[0]))
minLon, maxLon = (float(destination[1]),currentLocation[1]) if float(destination[1])<currentLocation[1] else (currentLocation[1], float(destination[1]))

#Buffer the bounding box by 10% to account for the pixel size of pins at the ends of the route.
lonBuffer = (maxLon-minLon)*0.1
minLon -= lonBuffer
maxLon += lonBuffer

latBuffer = (maxLat-minLat)*0.1
minLat -= latBuffer
maxLat += latBuffer

# Render route on the map.
staticMapResponse = await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&&path={}&pins={}&bbox={}&zoom=16".format(subscriptionKey,path,pins,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

staticMapImage = await staticMapResponse.content.read()

await session.close()
display(Image(staticMapImage))
```

![ruta](./media/tutorial-ev-routing/route.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a llamar directamente a varias API REST de Azure Maps y a visualizar datos de Azure Maps con Python.

Para explorar las API de Azure Maps que se usan en este tutorial, consulte:

* [Get Route Range](https://docs.microsoft.com/rest/api/maps/route/getrouterange)
* [Post Search Inside Geometry](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)
* [Data Upload](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)
* [Render - Get Map Image](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Post Route Matrix](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview)
* [Get Route Directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)

Para obtener una lista completa de las API REST de Azure Maps, consulte:

* [API REST de Azure Maps](https://docs.microsoft.com/azure/azure-maps/#reference)

Para más información sobre Azure Notebooks, consulte:

* [Azure Notebooks](https://docs.microsoft.com/azure/notebooks)