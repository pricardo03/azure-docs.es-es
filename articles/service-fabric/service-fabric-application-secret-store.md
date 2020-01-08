---
title: Almacén de secretos de Service Fabric
description: En este artículo se indica cómo usar el almacén de secretos de Service Bus.
ms.topic: conceptual
ms.date: 07/25/2019
ms.openlocfilehash: 16608d9eaf12fc9abc535ef316d7b5e8b74a8b37
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75457511"
---
#  <a name="service-fabric-secrets-store"></a>Almacén de secretos de Service Fabric
En este artículo se describe cómo crear y usar secretos en aplicaciones de Service Fabric mediante el almacén de secretos de Service Fabric (CSS). CSS es una caché local de almacén de secretos que se usa para almacenar datos confidenciales como contraseñas, tokens y claves cifradas en memoria.

## <a name="enabling-secrets-store"></a>Habilitación del almacén de secretos
 Agregue lo siguiente a la configuración del clúster en `fabricSettings` para habilitar CSS. Se recomienda usar un certificado diferente al certificado del clúster para CSS. Asegúrese de que el certificado de cifrado esté instalado en todos los nodos y de que `NetworkService` tenga permiso de lectura sobre la clave privada del certificado.
  ```json
    "fabricSettings": 
    [
        ...
    {
        "name":  "CentralSecretService",
        "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "3"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                },
                 {
                    "name" : "EncryptionCertificateThumbprint",
                    "value": "<thumbprint>"
                 }
                ,
                ],
            },
            ]
     }
        ...
     ]
```
## <a name="declare-secret-resource"></a>Declaración de un recurso como secreto
Puede crear un recurso secreto mediante la plantilla de Resource Manager o mediante la API REST.

* Uso de la plantilla de Resource Manager
```json
   "resources": [
      {
        "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
          }
        }
      ]
```
La plantilla anterior ha creado un recurso secreto `supersecret`, pero aún no se ha establecido ningún valor para este.

* Uso de la API de REST

Para crear un recurso secreto, `supersecret` realiza una solicitud PUT en `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview`. Necesita el certificado del clúster o el certificado de cliente de administrador para crear un secreto.

```powershell
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint>
```

## <a name="set-secret-value"></a>Establecimiento del valor del secreto
* Uso de la plantilla de Resource Manager

La plantilla siguiente de Resource Manager permite crear y establecer el valor del secreto `supersecret` con una versión `ver1`.
```json
  {
  "parameters": {
  "supersecret": {
      "type": "string",
      "metadata": {
        "description": "supersecret value"
      }
   }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
        }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "supersecret/ver1",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/supersecret"
      ],
      "properties": {
        "value": "[parameters('supersecret')]"
      }
    }
  ],
  ```
* Uso de la API de REST

```powershell
$Params = @{"properties": {"value": "mysecretpassword"}}
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
## <a name="using-the-secret-in-your-application"></a>Uso del secreto en la aplicación

1.  Agregue una sección al archivo settings.xml con el siguiente contenido. Tenga en cuenta que aquí el valor tiene el formato {`secretname:version`}

```xml
  <Section Name="testsecrets">
   <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
  </Section>
```
2. A continuación, importe la sección en ApplicationManifest.xml.
```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ConfigPackagePolicies CodePackageRef="Code">
        <ConfigPackage Name="Config" SectionName="testsecrets" EnvironmentVariableName="SecretPath" />
        </ConfigPackagePolicies>
    </Policies>
  </ServiceManifestImport>
```

La variable de entorno "SecretPath" apuntará al directorio donde se almacenan todos los secretos. Cada parámetro incluido en la sección `testsecrets` se almacenará en un archivo independiente. La aplicación puede ya usar el secreto como se muestra a continuación
```C#
secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
```
3. Montaje de secretos en un contenedor

El único cambio necesario para que los secretos pasen a estar disponibles en el contenedor es especificar un parámetro MountPoint en `<ConfigPackage>`.
Este es el archivo ApplicationManifest.xml modificado  

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ConfigPackagePolicies CodePackageRef="Code">
        <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="C:\secrets" EnvironmentVariableName="SecretPath" />
        <!-- Linux Container
         <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="/mnt/secrets" EnvironmentVariableName="SecretPath" />
        -->
      </ConfigPackagePolicies>
    </Policies>
  </ServiceManifestImport>
```
Los secretos estarán disponibles en el punto de montaje dentro del contenedor.

4. Enlace de un secreto a una variable de entorno 

Puede enlazar un secreto a una variable de entorno de proceso especificando Type="SecretsStoreRef". Este es un ejemplo de cómo enlazar la versión `ver1` de `supersecret` a la variable de entorno `MySuperSecret` en ServiceManifest.xml.

```xml
<EnvironmentVariables>
  <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
</EnvironmentVariables>
```
## <a name="next-steps"></a>Pasos siguientes
Más información sobre la [seguridad de aplicaciones y servicios](service-fabric-application-and-service-security.md)
