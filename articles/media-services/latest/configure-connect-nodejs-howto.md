---
title: Conectarse a Azure Media Services v3 API - Node.js
description: Obtenga información sobre cómo conectarse a la API de Media Services v3 con Node.js.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.openlocfilehash: 40880a2c28ce28a671930ef8837082247e61e24b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733137"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Conectarse a la API de Media Services v3 - Node.js

Este artículo muestra cómo conectar con el SDK del método mediante el inicio de sesión entidad de servicio de node.js de Azure Media Services v3.

## <a name="prerequisites"></a>Requisitos previos

- [Instale Node.js](https://nodejs.org/en/download/).
- [Cree una cuenta de Media Services](create-account-cli-how-to.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de cuenta de Media Services.

## <a name="create-packagejson"></a>Crear package.json

1. Cree un archivo package.json con el editor que prefiera.
1. Abra el archivo y pegue el código siguiente:

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.js",
  "dependencies": {
    "azure-arm-mediaservices": "^4.1.0",
    "azure-storage": "^2.8.0",
    "ms-rest": "^2.3.3",
    "ms-rest-azure": "^2.5.5"
  }
}
```

Se deben especificar los siguientes paquetes:

|Paquete|DESCRIPCIÓN|
|---|---|
|`azure-arm-mediaservices`|SDK de Azure Media Services. <br/>Para asegurarse de que usa el paquete más reciente de Azure Media Services, consulte [NPM install azure-arm-mediaservices](https://www.npmjs.com/package/azure-arm-mediaservices/).|
|`azure-storage`|SDK de Storage. Se utiliza cuando se cargan archivos en recursos.|
|`ms-rest-azure`| Se usa para iniciar sesión.|

Puede ejecutar el comando siguiente para asegurarse de que usa el paquete más reciente:

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>Conectarse al cliente de Node.js

1. Cree un archivo .js con su editor favorito.
1. Abra el archivo y pegue el siguiente código.
1. Establezca los valores en la sección "configuración de punto de conexión" para los valores que se obtuvo al [acceder a las API](access-api-cli-how-to.md).

```js
'use strict';

const MediaServices = require('azure-arm-mediaservices');
const msRestAzure = require('ms-rest-azure');
const msRest = require('ms-rest');
const azureStorage = require('azure-storage');

// endpoint config
// make sure your URL values end with '/'
const armAadAudience = "";
const aadEndpoint = "";
const armEndpoint = "";
const subscriptionId = "";
const accountName = "";
const region = "";
const aadClientId = "";
const aadSecret = "";
const aadTenantId = "";
const resourceGroup = "";

let azureMediaServicesClient;

///////////////////////////////////////////
//     Entrypoint for sample script      //
///////////////////////////////////////////

msRestAzure.loginWithServicePrincipalSecret(aadClientId, aadSecret, aadTenantId, {
  environment: {
    activeDirectoryResourceId: armAadAudience,
    resourceManagerEndpointUrl: armEndpoint,
    activeDirectoryEndpointUrl: aadEndpoint
  }
}, async function(err, credentials, subscriptions) {
    if (err) return console.log(err);
    azureMediaServicesClient = new MediaServices(credentials, subscriptionId, armEndpoint, { noRetryPolicy: true });
    
    console.log("connected");

});
```

## <a name="run-your-app"></a>Ejecutar la aplicación

Abra el símbolo del sistema. Vaya al directorio del ejemplo y ejecute los comandos siguientes:

```
npm install 
node index.js
```

## <a name="see-also"></a>Vea también

- [Conceptos de Azure Media Services](concepts-overview.md)
- [Instalación de azure-arm-mediaservices para NPM](https://www.npmjs.com/package/azure-arm-mediaservices/)

## <a name="next-steps"></a>Pasos siguientes

Explore la documentación de [referencia de Node.js](https://aka.ms/ams-v3-nodejs-ref) y los [ejemplos](https://github.com/Azure-Samples/media-services-v3-node-tutorials) que muestran cómo usar Media Services API con node.js.

