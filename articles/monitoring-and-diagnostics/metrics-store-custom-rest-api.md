---
title: Envío de métricas personalizadas de un recurso de Azure al almacén de métricas de Azure Monitor mediante la API de REST
description: Envío de métricas personalizadas de un recurso de Azure al almacén de métricas de Azure Monitor mediante la API de REST
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: c01440437eae3cb076627ab9f2221e33b833f472
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977249"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-using-a-rest-api"></a>Envío de métricas personalizadas de un recurso de Azure al almacén de métricas de Azure Monitor mediante la API de REST

En este artículo muestra cómo enviar métricas personalizadas de los recursos de Azure al almacén de métricas Azure Monitor a través de una API de REST.  Una vez que las métricas se encuentran en Azure Monitor, puede realizar con ellas todas las acciones que hace con las métricas estándar, como crear gráficos y alertas con ellas, enrutarlas a otras herramientas externas, etc.  

>[!NOTE] 
>La API de REST solo permite el envío de métricas personalizadas de los recursos de Azure. Para enviar métricas de recursos en distintos entornos o de forma local, puede usar [Application Insights](../application-insights/app-insights-api-custom-events-metrics.md).    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Creación y autorización de una entidad de servicio para la emisión de métricas 

Cree una entidad de servicio en el inquilino de Azure Active Directory según las instrucciones que encontrará en [Creación de una entidad de servicio](../azure-resource-manager/resource-group-create-service-principal-portal.md). 

Tenga en cuenta lo siguiente al realizar este proceso: 

- Puede colocar cualquier dirección URL como dirección URL de inicio de sesión.  
- Cree un nuevo secreto de cliente para esta aplicación.  
- Guarde la clave y el identificador de cliente para su uso en pasos posteriores.  

Asigne a la aplicación que creó como parte del paso 1 "Supervisión del publicador de métricas" permisos para el recurso frente al cual desea emitir métricas. Si va a usar la aplicación para emitir métricas personalizadas frente a muchos recursos, puede conceder estos permisos en el nivel de suscripción o de grupo de recursos. 

## <a name="get-an-authorization-token"></a>Obtención de un token de autorización
Abra un símbolo del sistema y ejecute el comando siguiente.
```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step> " -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Guarde el token de acceso de la respuesta.

![Token de acceso](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>Emisión de la métrica a través de la API de REST 

1. Pegue el siguiente código JSON en un archivo y guárdelo como custommetric.json en el equipo local. Actualice el parámetro de tiempo del archivo JSON. 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20”, 
        "data": { 
            "baseData": { 
                "metric": "QueueDepth", 
                "namespace": "QueueProcessing", 
                "dimNames": [ 
                  "QueueName", 
                  "MessageType" 
                ], 
                "series": [ 
                  { 
                    "dimValues": [ 
                      "ImagesToProcess", 
                      "JPEG" 
                    ], 
                    "min": 3, 
                    "max": 20, 
                    "sum": 28, 
                    "count": 3 
                  } 
                ] 
            } 
        } 
    } 
    ``` 

1. En la ventana del símbolo del sistema, publique los datos de la métrica. 
    - Región de Azure: debe coincidir con la región de implementación del recurso para el que se van a emitir las métricas. 
    - ResourceID: identificador de recurso del recurso de Azure con respecto al que realiza el seguimiento de la métrica.  
    - Token de acceso: pegue el token adquirido anteriormente.

    ```Shell 
    curl -X POST curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId> /metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
    ```
1. Cambie la marca de tiempo y los valores del archivo JSON. 
1. Repita los dos pasos anteriores varias veces para tener datos durante varios minutos.

## <a name="troubleshooting"></a>solución de problemas 
Si recibe un error con alguna parte del proceso, considere lo siguiente:

1. No se pueden emitir métricas con respecto a una suscripción o grupo de recursos como el recurso de Azure. 
1. No se puede colocar en el almacén una métrica con más de 20 minutos desde su creación. El almacén de métricas está optimizado para las alertas y los gráficos en tiempo real. 
2. El número de nombres de la dimensión debe coincidir con los valores y viceversa. Compruebe los valores. 
2. Se pueden emitir métricas con respecto a una región que no es compatible con las métricas personalizadas. Consulte las [regiones compatibles con la métrica personalizada (versión preliminar)](metrics-custom-overview.md#supported-regions). 



## <a name="view-your-metrics"></a>Visualización de las métricas 

1. Iniciar sesión en Azure Portal 

1. En el menú de la izquierda, haga clic en **Monitor** (Supervisión). 

1. En la página Supervisión, haga clic en **Métricas**. 

   ![Token de acceso](./media/metrics-store-custom-rest-api/metrics.png) 

1. Cambie el período de agregación a **Últimos 30 minutos**.  

1. En la lista desplegable *Recursos*, seleccione el recurso con respecto al que emitió la métrica.  

1. En la lista desplegable *Espacios de nombres*, seleccione **QueueProcessing**. 

1. En la lista desplegable *Métricas*, seleccione **QueueDepth**.  

 
## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de las [métricas personalizadas](metrics-custom-overview.md).