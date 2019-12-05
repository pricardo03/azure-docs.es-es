---
title: Referencia de YAML para el grupo de contenedores
description: Referencia del archivo YAML compatible con Azure Container Instances para configurar un grupo de contenedores
ms.topic: article
ms.date: 08/12/2019
ms.openlocfilehash: 5603f2e0f63c4f83a6d3761feb540abb8b8b7d5c
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533486"
---
# <a name="yaml-reference-azure-container-instances"></a>Referencia de YAML: Azure Container Instances

En este artículo se abordan la sintaxis y las propiedades del archivo YAML compatible con Azure Container Instances para configurar un [grupo de contenedores](container-instances-container-groups.md). Use un archivo YAML para especificar la configuración del grupo en el comando [az container create][az-container-create] en la CLI de Azure. 

Un archivo YAML es una manera cómoda de configurar un grupo de contenedores para implementaciones reproducibles. Es una alternativa concisa al uso de una [plantilla de Resource Manager](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups) o los SDK de Azure Container Instances para crear o actualizar un grupo de contenedores.

> [!NOTE]
> Esta referencia se aplica a los archivos YAML para la versión de la API REST de Azure Container Instances`2018-10-01`.

## <a name="schema"></a>Schema 

A continuación se muestra el esquema para el archivo YAML, con comentarios para resaltar las principales propiedades. Para obtener una descripción de las propiedades de este esquema, vea la sección [Valores de propiedad](#property-values).

```yml
name: string  # Name of the container group
apiVersion: '2018-10-01'
location: string
tags: {}
identity: 
  type: string
  userAssignedIdentities: {}
properties: # Properties of container group
  containers: # Array of container instances in the group
  - name: string # Name of an instance
    properties: # Properties of an instance
      image: string # Container image used to create the instance
      command:
      - string
      ports: # Exposed ports on the instance
      - protocol: string
        port: integer
      environmentVariables:
      - name: string
        value: string
        secureValue: string
      resources: # Resource requirements of the instance
        requests:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
        limits:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
      volumeMounts: # Array of volume mounts for the instance
      - name: string
        mountPath: string
        readOnly: boolean
      livenessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
      readinessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
  imageRegistryCredentials: # Credentials to pull a private image
  - server: string
    username: string
    password: string
  restartPolicy: string
  ipAddress: # IP address configuration of container group
    ports:
    - protocol: string
      port: integer
    type: string
    ip: string
    dnsNameLabel: string
  osType: string
  volumes: # Array of volumes available to the instances
  - name: string
    azureFile:
      shareName: string
      readOnly: boolean
      storageAccountName: string
      storageAccountKey: string
    emptyDir: {}
    secret: {}
    gitRepo:
      directory: string
      repository: string
      revision: string
  diagnostics:
    logAnalytics:
      workspaceId: string
      workspaceKey: string
      logType: string
      metadata: {}
  networkProfile: # Virtual network profile for container group
    id: string
  dnsConfig: # DNS configuration for container group
    nameServers:
    - string
    searchDomains: string
    options: string
```

## <a name="property-values"></a>Valores de propiedad

Las tablas siguientes describen los valores que debe establecer en el esquema.

<a id="Microsoft.ContainerInstance/containerGroups" />

### <a name="microsoftcontainerinstancecontainergroups-object"></a>Objeto Microsoft.ContainerInstance/containerGroups

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Nombre | string | Sí | Nombre del grupo de contenedores. |
|  apiVersion | enum | Sí | 2018-10-01 |
|  location | string | Sin | Ubicación del recurso. |
|  etiquetas | object | Sin | Etiquetas del recurso. |
|  identity | object | Sin | Identidad del grupo de contenedores, si está configurada. - [Objeto ContainerGroupIdentity](#ContainerGroupIdentity) |
|  properties | object | Sí | [Objeto ContainerGroupProperties](#ContainerGroupProperties) |


<a id="ContainerGroupIdentity" />

### <a name="containergroupidentity-object"></a>Objeto ContainerGroupIdentity

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Tipo | enum | Sin | Tipo de identidad utilizado para el grupo de contenedores. El tipo "SystemAssigned, UserAssigned" incluye una identidad creada implícitamente y un conjunto de identidades asignadas por el usuario. El tipo "None" quitará las identidades del grupo de contenedores. - SystemAssigned, UserAssigned, SystemAssigned, UserAssigned, None |
|  userAssignedIdentities | object | Sin | Lista de identidades de usuario asociadas con el grupo de contenedores. Las referencias clave del diccionario de identidades de usuario serán los identificadores de recursos de Azure Resource Manager con el formato "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}". |


<a id="ContainerGroupProperties" />

### <a name="containergroupproperties-object"></a>Objeto ContainerGroupProperties

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  containers | array | Sí | Contenedores del grupo de contenedores. - [Objeto Container](#Container) |
|  imageRegistryCredentials | array | Sin | Credenciales del registro de imágenes por las que se crea el grupo de contenedores. - [Objeto ImageRegistryCredential](#ImageRegistryCredential) |
|  restartPolicy | enum | Sin | Directiva de reinicio para todos los contenedores del grupo de contenedores. - `Always` Reiniciar siempre- `OnFailure` Reiniciar con un error - `Never` No reiniciar nunca. - Always, OnFailure, Never |
|  ipAddress | object | Sin | Tipo de dirección IP del grupo de contenedores. - [Objeto IpAddress](#IpAddress) |
|  osType | enum | Sí | Tipo de sistema operativo requerido por los contenedores del grupo de contenedores. - Windows o Linux |
|  volumes | array | Sin | Lista de volúmenes que pueden montar los contenedores de este grupo de contenedores. - [Objeto Volume](#Volume) |
|  diagnóstico | object | Sin | Información de diagnóstico de un grupo de contenedores. - [Objeto ContainerGroupDiagnostics](#ContainerGroupDiagnostics) |
|  networkProfile | object | Sin | Información de perfil de red de un grupo de contenedores. - [Objeto ContainerGroupNetworkProfile](#ContainerGroupNetworkProfile) |
|  dnsConfig | object | Sin | Información de configuración DNS de un grupo de contenedores. - [Objeto DnsConfiguration](#DnsConfiguration) |


<a id="Container" />

### <a name="container-object"></a>Objeto Container

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Nombre | string | Sí | Nombre proporcionado por el usuario de la instancia de contenedor. |
|  properties | object | Sí | Propiedades de la instancia de contenedor. - [Objeto ContainerProperties](#ContainerProperties) |


<a id="ImageRegistryCredential" />

### <a name="imageregistrycredential-object"></a>Objeto ImageRegistryCredential

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  server | string | Sí | Servidor de registro de imagen de Docker sin un protocolo como "http" y "https". |
|  username | string | Sí | Nombre de usuario del registro privado. |
|  password | string | Sin | Contraseña del registro privado. |


<a id="IpAddress" />

### <a name="ipaddress-object"></a>Objeto IpAddress

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  ports | array | Sí | Lista de puertos expuestos en el grupo de contenedores. - [Objeto Port](#Port) |
|  Tipo | enum | Sí | Especifica si la dirección IP se expone a la red pública de Internet o una red virtual privada. - Pública o privada |
|  ip | string | Sin | Dirección IP expuesta a la red pública de Internet. |
|  dnsNameLabel | string | Sin | Etiqueta del nombre DNS para la IP. |


<a id="Volume" />

### <a name="volume-object"></a>Objeto Volume

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Nombre | string | Sí | El nombre del objeto visual. |
|  azureFile | object | Sin | Volumen de archivos de Azure. - [Objeto AzureFileVolume](#AzureFileVolume) |
|  emptyDir | object | Sin | Volumen de directorio vacío. |
|  secret | object | Sin | Volumen secreto. |
|  gitRepo | object | Sin | Volumen de repositorio GIT. - [Objeto GitRepoVolume](#GitRepoVolume) |


<a id="ContainerGroupDiagnostics" />

### <a name="containergroupdiagnostics-object"></a>Objeto ContainerGroupDiagnostics

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  logAnalytics | object | Sin | Información de Log Analytics del grupo de contenedores. - [Objeto LogAnalytics](#LogAnalytics) |


<a id="ContainerGroupNetworkProfile" />

### <a name="containergroupnetworkprofile-object"></a>Objeto ContainerGroupNetworkProfile

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  id | string | Sí | Identificador de un perfil de red. |


<a id="DnsConfiguration" />

### <a name="dnsconfiguration-object"></a>Objeto DnsConfiguration

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  nameServers | array | Sí | Servidores DNS para el grupo de contenedores. - cadena |
|  searchDomains | string | Sin | Dominios de búsqueda de DNS para la búsqueda del nombre de host en el grupo de contenedores. |
|  options | string | Sin | Opciones de DNS para el grupo de contenedores. |


<a id="ContainerProperties" />

### <a name="containerproperties-object"></a>Objeto ContainerProperties

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  imagen | string | Sí | Nombre de la imagen utilizada para crear la instancia de contenedor. |
|  command | array | Sin | Comandos para ejecutar dentro de la instancia de contenedor en el formato de ejecución. - cadena |
|  ports | array | Sin | Puertos expuestos en la instancia de contenedor. - [Objeto ContainerPort](#ContainerPort) |
|  environmentVariables | array | Sin | Variables de entorno para establecer en la instancia de contenedor. - [Objeto EnvironmentVariable](#EnvironmentVariable) |
|  resources | object | Sí | Requisitos de los recursos de la instancia de contenedor. - [Objeto ResourceRequirements](#ResourceRequirements) |
|  volumeMounts | array | Sin | Montajes de volumen disponibles para la instancia de contenedor. - [Objeto VolumeMount](#VolumeMount) |
|  livenessProbe | object | Sin | Sondeo de ejecución. - [Objeto ContainerProbe](#ContainerProbe) |
|  readinessProbe | object | Sin | Sondeo de preparación. - [Objeto ContainerProbe](#ContainerProbe) |


<a id="Port" />

### <a name="port-object"></a>Objeto Port

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  protocol | enum | Sin | Protocolo asociado al puerto. - TCP o UDP |
|  port | integer | Sí | Número del puerto. |


<a id="AzureFileVolume" />

### <a name="azurefilevolume-object"></a>Objeto AzureFileVolume

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  shareName | string | Sí | Nombre del recurso compartido de archivos de Azure que se va a montar como un volumen. |
|  readOnly | boolean | Sin | Marca que indica si el recurso compartido de archivos de Azure montado como un volumen es de solo lectura. |
|  storageAccountName | string | Sí | Nombre de la cuenta de almacenamiento que contiene el recurso compartido de archivos de Azure. |
|  storageAccountKey | string | Sin | Clave de acceso de la cuenta de almacenamiento utilizada para acceder al recurso compartido de archivos de Azure. |


<a id="GitRepoVolume" />

### <a name="gitrepovolume-object"></a>Objeto GitRepoVolume

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Directorio | string | Sin | Nombre del directorio de destino. No debe contener ".." ni empezar así.  Si se proporciona ".", el directorio de volumen será el repositorio de GIT.  De lo contrario, si se especifica, el volumen contendrá el repositorio de GIT en el subdirectorio con el nombre especificado. |
|  repository | string | Sí | Dirección URL del repositorio |
|  revision | string | Sin | Hash de confirmación para la revisión especificada. |


<a id="LogAnalytics" />

### <a name="loganalytics-object"></a>Objeto LogAnalytics

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  workspaceId | string | Sí | Identificador del área de trabajo de Log Analytics |
|  workspaceKey | string | Sí | Clave del área de trabajo de Log Analytics |
|  logType | enum | Sin | Tipo de registro que se va a usar. - ContainerInsights o ContainerInstanceLogs |
|  metadata | object | Sin | Metadatos de Log Analytics. |


<a id="ContainerPort" />

### <a name="containerport-object"></a>Objeto ContainerPort

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  protocol | enum | Sin | Protocolo asociado al puerto. - TCP o UDP |
|  port | integer | Sí | Número de puerto expuesto en el grupo de contenedores. |


<a id="EnvironmentVariable" />

### <a name="environmentvariable-object"></a>Objeto EnvironmentVariable

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Nombre | string | Sí | Nombre de la variable de entorno. |
|  value | string | Sin | Valor de la variable de entorno. |
|  secureValue | string | Sin | Valor de la variable de entorno segura. |


<a id="ResourceRequirements" />

### <a name="resourcerequirements-object"></a>Objeto ResourceRequirements

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Solicitudes | object | Sí | Solicitudes de los recursos de la instancia de contenedor. - [Objeto ResourceRequests](#ResourceRequests) |
|  límites | object | Sin | Límites de los recursos de la instancia de contenedor. - [Objeto ResourceLimits](#ResourceLimits) |


<a id="VolumeMount" />

### <a name="volumemount-object"></a>Objeto VolumeMount

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  Nombre | string | Sí | Nombre del montaje de volumen. |
|  mountPath | string | Sí | La ruta de acceso dentro del contenedor en la que se debe montar el volumen. No debe contener dos puntos (:). |
|  readOnly | boolean | Sin | Marca que indica si el montaje del volumen es de solo lectura. |


<a id="ContainerProbe" />

### <a name="containerprobe-object"></a>Objeto ContainerProbe

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  exec | object | Sin | Comando de ejecución para sondear - [objeto ContainerExec](#ContainerExec) |
|  httpGet | object | Sin | La configuración de http get para sondear - [objeto ContainerHttpGet](#ContainerHttpGet) |
|  initialDelaySeconds | integer | Sin | Segundos de retardo iniciales. |
|  periodSeconds | integer | Sin | Período de segundos. |
|  failureThreshold | integer | Sin | Umbral de error. |
|  successThreshold | integer | Sin | Umbral correcto. |
|  timeoutSeconds | integer | Sin | Tiempo de espera en segundos. |


<a id="ResourceRequests" />

### <a name="resourcerequests-object"></a>Objeto ResourceRequests

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | número | Sí | Solicitud de memoria en GB de esta instancia de contenedor. |
|  cpu | número | Sí | Solicitud de CPU de esta instancia de contenedor. |
|  gpu | object | Sin | Solicitud de GPU de esta instancia de contenedor. - [Objeto GpuResource](#GpuResource) |


<a id="ResourceLimits" />

### <a name="resourcelimits-object"></a>Objeto ResourceLimits

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | número | Sin | Límite de memoria en GB de esta instancia de contenedor. |
|  cpu | número | Sin | Límite de CPU de esta instancia de contenedor. |
|  gpu | object | Sin | Límite de GPU de esta instancia de contenedor. - [Objeto GpuResource](#GpuResource) |


<a id="ContainerExec" />

### <a name="containerexec-object"></a>Objeto ContainerExec

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  command | array | Sin | Comandos para ejecutar dentro del contenedor. - cadena |


<a id="ContainerHttpGet" />

### <a name="containerhttpget-object"></a>Objeto ContainerHttpGet

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  path | string | Sin | Ruta de acceso que se va a sondear. |
|  port | integer | Sí | Número de puerto que se va a sondear. |
|  scheme | enum | Sin | Esquema. - http o https |


<a id="GpuResource" />

### <a name="gpuresource-object"></a>Objeto GpuResource

|  NOMBRE | type | Obligatorio | Valor |
|  ---- | ---- | ---- | ---- |
|  count | integer | Sí | Recuento del recurso de GPU. |
|  sku | enum | Sí | SKU del recurso de GPU. - K80, P100, V100 |


## <a name="next-steps"></a>Pasos siguientes

Vea el tutorial [Implementación de un grupo de varios contenedores con un archivo YAML](container-instances-multi-container-yaml.md).

Vea ejemplos de uso de un archivo YAML para implementar grupos de contenedores en una [red virtual](container-instances-vnet.md) o para [montar un volumen externo](container-instances-volume-azure-files.md).

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create

