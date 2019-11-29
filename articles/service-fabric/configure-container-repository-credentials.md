---
title: Azure Service Fabric - Configuración de credenciales de repositorio de contenedor | Microsoft Docs
description: Configuración de credenciales de repositorio para descargar imágenes desde el registro de contenedor
services: service-fabric
documentationcenter: .net
author: arya
manager: gkhanna
ms.assetid: b93d31e5-9e4c-4405-b266-c0efa4643d97
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 8/1/2019
ms.author: arya
ms.openlocfilehash: c415739934e2318ea5287d5eed9f8235029b666f
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74405616"
---
# <a name="configure-repository-credentials-for-your-application-to-download-container-images"></a>Configuración de credenciales de repositorio para que la aplicación descargue imágenes de contenedor

Configure la autenticación de Container Registry mediante la adición de `RepositoryCredentials` a `ContainerHostPolicies` en el archivo ApplicationManifest.xml. Agregue la cuenta y contraseña para el registro de contenedores myregistry.azurecr.io, que permite al servicio descargar la imagen de contenedor del repositorio.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

Se recomienda cifrar la contraseña del repositorio con un certificado de cifrado que se implemente en todos los nodos del clúster. Cuando Service Fabric implementa el paquete de servicio en el clúster, el certificado de cifrado se utiliza para descifrar el texto cifrado. El cmdlet Invoke-ServiceFabricEncryptText se usa para crear el texto cifrado para la contraseña, que se agrega al archivo ApplicationManifest.xml.
Vea [Administración de secretos](service-fabric-application-secret-management.md) para más información sobre los certificados y la semántica de cifrado.

## <a name="configure-cluster-wide-credentials"></a>Configuración de las credenciales en todo el clúster

Service Fabric permite configurar credenciales en todo el clúster que las aplicaciones pueden usar como credenciales del repositorio predeterminadas.

Para habilitar o deshabilitar esta característica, hay que agregar el atributo `UseDefaultRepositoryCredentials` a `ContainerHostPolicies` en ApplicationManifest.xml con un valor de `true` o `false`.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code" UseDefaultRepositoryCredentials="true">
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

Después, Service Fabric usa las credenciales del repositorio predeterminadas, que se pueden especificar en el archivo ClusterManifest de la sección `Hosting`.  Si `UseDefaultRepositoryCredentials` es `true`, Service Fabric lee los valores siguientes de ClusterManifest:

* DefaultContainerRepositoryAccountName (cadena)
* DefaultContainerRepositoryPassword (cadena)
* IsDefaultContainerRepositoryPasswordEncrypted (valor booleano)
* DefaultContainerRepositoryPasswordType (cadena): se admite a partir de la versión 6.4 del entorno de ejecución

Este es un ejemplo de lo que se puede agregar dentro de la sección `Hosting` en el archivo ClusterManifestTemplate.json. La sección `Hosting` puede agregarse al crear el clúster o posteriormente en una actualización de la configuración. Para obtener más información, consulte [Personalización de la configuración de un clúster de Service Fabric](service-fabric-cluster-fabric-settings.md) y [Administración de los secretos en aplicaciones de Azure Service Fabric](service-fabric-application-secret-management.md).

```json
"fabricSettings": [
    ...,
    {
        "name": "Hosting",
        "parameters": [
          {
            "name": "EndpointProviderEnabled",
            "value": "true"
          },
          {
            "name": "DefaultContainerRepositoryAccountName",
            "value": "someusername"
          },
          {
            "name": "DefaultContainerRepositoryPassword",
            "value": "somepassword"
          },
          {
            "name": "IsDefaultContainerRepositoryPasswordEncrypted",
            "value": "false"
          },
          {
            "name": "DefaultContainerRepositoryPasswordType",
            "value": "PlainText"
          }
        ]
      },
]
```

## <a name="leveraging-the-managed-identity-of-the-virtual-machine-scale-set-by-using-managed-identity-service-msi"></a>Aprovechamiento de la identidad administrada del conjunto de escalado de máquinas virtuales mediante Managed Service Identity (MSI)

Service Fabric permite usar tokens como credenciales para descargar imágenes relativas a los contenedores.  Esta característica aprovecha la identidad administrada del conjunto de escalado de máquinas virtuales subyacente para autenticarse en el Registro, lo que elimina la necesidad de administrar las credenciales de usuario.  Vea [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) para más información sobre MSI.  El uso de esta característica requiere los siguientes pasos:

1.  Asegúrese de que la identidad administrada asignada por el sistema está habilitada para la máquina virtual (vea la siguiente captura de pantalla).

    ![Crear una identidad de conjunto de escalado de máquinas virtuales](./media/configure-container-repository-credentials/configure-container-repository-credentials-acr-iam.png)

2.  Después, conceda permisos al conjunto de escalado de máquinas virtuales para extraer o leer imágenes del Registro.  Vaya a Control de acceso (IAM) en ACR a través de Azure Blade y asigne los permisos correctos al conjunto de escalado de máquinas virtuales, como se muestra aquí:

    ![Agregar una entidad de seguridad de máquina virtual a ACR](./media/configure-container-repository-credentials/configure-container-repository-credentials-vmss-identity.png)

3.  Una vez completados los pasos anteriores, modifique el archivo applicationmanifest.xml.  Busque la etiqueta "ContainerHostPolicies" y agregue el atributo `‘UseTokenAuthenticationCredentials=”true”`.

    ```xml
      <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
      <Policies>
        <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" UseTokenAuthenticationCredentials="true">
          <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        </ContainerHostPolicies>
        <ResourceGovernancePolicy CodePackageRef="NodeService.Code" MemoryInMB="256"/>
      </Policies>
      </ServiceManifestImport>
    ```

    > [!NOTE]
    > Si la marca `UseDefaultRepositoryCredentials` está establecida en true cuando `UseTokenAuthenticationCredentials` es true, se producirá un error durante la implementación.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la [autenticación del registro de contenedor](/azure/container-registry/container-registry-authentication).
