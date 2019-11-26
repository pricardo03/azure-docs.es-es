---
title: Conservación del estado en Linux - Azure Event Grid IoT Edge | Microsoft Docs
description: Persistencia de los metadatos en Linux
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 3506399537fe2cb16014ceb3429bce5aeee8cb69
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73100330"
---
# <a name="persist-state-in-linux"></a>Conservación del estado en Linux

Los temas y las suscripciones creadas en el módulo de Event Grid se almacenan de forma predeterminada en el sistema de archivos de contenedor. Sin persistencia, si se vuelve a implementar el módulo, se perderán todos los metadatos creados. Actualmente solo se conservan los metadatos. Los eventos se almacenan en memoria. Si el módulo de Event Grid se vuelve a implementar o se reinicia, se perderán todos los eventos no entregados.

En este artículo se proporcionan los pasos para implementar el módulo de Event Grid con persistencia en las implementaciones de Linux.

> [!NOTE]
>El módulo de Event Grid se ejecuta como un usuario con pocos privilegios con UID `2000` y nombre `eventgriduser`.

## <a name="persistence-via-volume-mount"></a>Persistencia a través del montaje del volumen

 [Los volúmenes de Docker](https://docs.docker.com/storage/volumes/) se usan para conservar los datos en las implementaciones. Puede permitir que Docker cree automáticamente un volumen con nombre como parte de la implementación del módulo de Event Grid. Esta opción es la más sencilla. Puede especificar el nombre del volumen que se va a crear en la sección **Binds** de la siguiente manera:

```json
  {
     "HostConfig": {
          "Binds": [
                 "<your-volume-name-here>:/app/metadataDb"
           ]
      }
   }
```

>[!IMPORTANT]
>No cambie la segunda parte del valor de enlace. Apunta a una ubicación concreta dentro del módulo. Para el módulo de Event Grid en Linux, debe ser **/app/metadataDb**.

Por ejemplo, la siguiente configuración producirá la creación del volumen **egmetadataDbVol** donde se conservarán los metadatos.

```json
 {
  "Env": [
    "inbound:serverAuth:tlsPolicy=strict",
    "inbound:serverAuth:serverCert:source=IoTEdge",
    "inbound:clientAuth:sasKeys:enabled=false",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=true",
    "outbound:clientAuth:clientCert:enabled=true",
    "outbound:clientAuth:clientCert:source=IoTEdge",
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=true"
  ],
  "HostConfig": {
    "Binds": [
      "egmetadataDbVol:/app/metadataDb"
    ],
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
```

También puede crear un volumen de Docker mediante comandos de cliente de Docker. 

## <a name="persistence-via-host-directory-mount"></a>Persistencia mediante el montaje de directorios de host

En lugar de un volumen de Docker, también tiene la opción de montar una carpeta de host.

1. En primer lugar, cree un usuario con el nombre **eventgriduser** y el identificador **2000** en el equipo host mediante la ejecución del siguiente comando:

    ```sh
    sudo useradd -u 2000 eventgriduser
    ```
1. En el sistema de archivos host, cree un directorio ejecutando el comando siguiente.

   ```sh
   md <your-directory-name-here>
   ```

    Por ejemplo, al ejecutar el siguiente comando, se creará un directorio llamado **myhostdir**.

    ```sh
    md /myhostdir
    ```
1. A continuación, cree el propietario **eventgriduser** de esta carpeta ejecutando el siguiente comando.

   ```sh
   sudo chown eventgriduser:eventgriduser -hR <your-directory-name-here>
   ```

    Por ejemplo,

    ```sh
    sudo chown eventgriduser:eventgriduser -hR /myhostdir
    ```
1. Use **Binds** para montar el directorio y volver a implementar el módulo de Event Grid desde Azure Portal.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:/app/metadataDb"
             ]
         }
    }
    ```

    Por ejemplo,

    ```json
    {
          "Env": [
            "inbound:serverAuth:tlsPolicy=strict",
            "inbound:serverAuth:serverCert:source=IoTEdge",
            "inbound:clientAuth:sasKeys:enabled=false",
            "inbound:clientAuth:clientCert:enabled=true",
            "inbound:clientAuth:clientCert:source=IoTEdge",
            "inbound:clientAuth:clientCert:allowUnknownCA=true",
            "outbound:clientAuth:clientCert:enabled=true",
            "outbound:clientAuth:clientCert:source=IoTEdge",
            "outbound:webhook:httpsOnly=true",
            "outbound:webhook:skipServerCertValidation=false",
            "outbound:webhook:allowUnknownCA=true"
          ],
          "HostConfig": {
                "Binds": [
                  "/myhostdir:/app/metadataDb"
                ],
                "PortBindings": {
                      "4438/tcp": [
                        {
                          "HostPort": "4438"
                        }
                      ]
                }
          }
    }
    ```

    >[!IMPORTANT]
    >No cambie la segunda parte del valor de enlace. Apunta a una ubicación concreta dentro del módulo. Para el módulo de Event Grid en Linux, debe ser **/app/metadata**.
