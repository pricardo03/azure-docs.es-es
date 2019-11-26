---
title: Conservación del estado en Windows - Azure Event Grid IoT Edge | Microsoft Docs
description: Conservación del estado en Windows
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 485c6d4a92539a2ba67aece319c68d31649e8045
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991765"
---
# <a name="persist-state-in-windows"></a>Conservación del estado en Windows

Los temas y las suscripciones creadas en el módulo de Event Grid se almacenan de forma predeterminada en el sistema de archivos de contenedor. Sin persistencia, si se vuelve a implementar el módulo, se perderán todos los metadatos creados. Para conservar los datos en las implementaciones, debe conservar los datos fuera del sistema de archivos de contenedor. Actualmente solo se conservan los metadatos. Los eventos se almacenan en la memoria. Si el módulo de Event Grid se vuelve a implementar o se reinicia, se perderán todos los eventos no entregados.

En este artículo se proporcionan los pasos necesarios para implementar el módulo de Event Grid con persistencia en las implementaciones de Windows.

> [!NOTE]
>El módulo de Event Grid se ejecuta como un usuario con pocos privilegios **ContainerUser** en Windows.

## <a name="persistence-via-volume-mount"></a>Persistencia a través del montaje del volumen

[Los volúmenes de Docker](https://docs.docker.com/storage/volumes/) se usan para conservar los datos en las implementaciones. Para montar un volumen, debe crearlo con los comandos de Docker, conceder permisos para que el contenedor pueda leer y escribir en él y, a continuación, implementar el módulo. No hay ninguna provisión para crear automáticamente un volumen con los permisos necesarios en Windows. Debe crearse antes de la implementación.

1. Cree un volumen mediante la ejecución del siguiente comando:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume create <your-volume-name-here>
    ```

    Por ejemplo,

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume create myeventgridvol
   ```
1. Obtenga el directorio host al que se asigna el volumen mediante la ejecución del comando siguiente.

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume inspect <your-volume-name-here>
    ```

    Por ejemplo,

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume inspect myeventgridvol
   ```

   Salida de ejemplo:

   ```json
   [
          {
            "CreatedAt": "2019-07-30T21:20:59Z",
            "Driver": "local",
            "Labels": {},
            "Mountpoint": "C:\\ProgramData\\iotedge-moby\u000bolumes\\myeventgridvol\\_data",
            "Name": "myeventgridvol",
            "Options": {},
            "Scope": "local"
          }
   ]
   ```
1. Agregue el grupo **Usuarios** al valor al que apunta el **punto de montaje** como se indica a continuación:
    1. Inicie el explorador de archivos.
    1. Navegue hasta la carpeta a la que apunta el **punto de montaje**.
    1. Haga clic con el botón derecho y seleccione **Propiedades**.
    1. Seleccione **Seguridad**.
    1. En *Nombre de grupo o usuario, seleccione **Editar**.
    1. Seleccione **Agregar**, escriba `Users`, seleccione **Comprobar nombres** y, a continuación, seleccione **Aceptar**.
    1. En los *permisos para usuarios*, seleccione **Modificar** y luego **Aceptar**.
1. Use los **enlaces** para montar este volumen y volver a implementar el módulo de Event Grid desde Azure Portal.

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
                  "<your-volume-name-here>:C:\\app\\metadataDb"
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
   >No cambie la segunda parte del valor de enlace. Apunta a una ubicación concreta dentro del módulo. Para el módulo de Event Grid en Windows, debe ser **C:\\app\\metadataDb**.


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
                "myeventgridvol:C:\\app\\metadataDb"
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

## <a name="persistence-via-host-directory-mount"></a>Persistencia mediante el montaje de directorios de host

Como alternativa, puede optar por crear un directorio en el sistema host y montar ese directorio.

1. En el sistema de archivos host, cree un directorio ejecutando el comando siguiente.

   ```sh
   mkdir <your-directory-name-here>
   ```

   Por ejemplo,

   ```sh
   mkdir C:\myhostdir
   ```
1. Use los **enlaces** para montar el directorio y volver a implementar el módulo de Event Grid desde Azure Portal.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:C:\\app\\metadataDb"
             ]
         }
    }
    ```

    >[!IMPORTANT]
    >No cambie la segunda parte del valor de enlace. Apunta a una ubicación concreta dentro del módulo. Para el módulo de Event Grid en Windows, debe ser **C:\\app\\metadataDb**.

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
                "C:\\myhostdir:C:\\app\\metadataDb"
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
