---
title: Tareas posteriores a la implementación de OpenShift en Azure | Documentos de Microsoft
description: Tareas adicionales posteriores a la implementación de un clúster de OpenShift.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/02/2019
ms.author: haroldw
ms.openlocfilehash: bc7a49aa143400387afcd59d5b9307d82a028486
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098668"
---
# <a name="post-deployment-tasks"></a>Tareas posteriores a la implementación

Después de implementar un clúster de OpenShift, puede configurar elementos adicionales. En este artículo se describe:

- Cómo configurar el inicio de sesión único con Azure Active Directory (Azure AD)
- Cómo configurar registros de Azure Monitor para supervisar OpenShift
- Cómo configurar las métricas y el registro
- Cómo instalar Open Service Broker para Azure (OSBA)

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Configurar el inicio de sesión único con Azure Active Directory

Para usar Azure Active Directory para la autenticación, primero debe crear un registro de la aplicación de Azure AD. Este proceso implica dos pasos: crear el registro de la aplicación y configurar los permisos.

### <a name="create-an-app-registration"></a>Crear un registro de aplicación

Estos pasos usan la CLI de Azure para crear el registro de aplicación y la GUI (portal) para definir los permisos. Para crear el registro de aplicación, se necesitan los cinco datos siguientes:

- Nombre para mostrar: Nombre de registro de la aplicación (por ejemplo, OCPAzureAD)
- Página de inicio: Dirección URL de la consola de OpenShift (por ejemplo, https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Identificador URI: Dirección URL de la consola de OpenShift (por ejemplo, https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Dirección URL de respuesta: Maestro de dirección URL pública y el nombre del registro de aplicación (por ejemplo, https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Contraseña: Contraseña segura (use una contraseña segura)

En el ejemplo siguiente, se creará un registro de aplicación con la información anterior:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com/console --password {Strong Password}
```

Si el comando se ejecuta correctamente, recibirá una salida JSON similar a la siguiente:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD"
  ]
}
```

Tome nota de la propiedad appId que ha devuelto el comando para otro paso.

En Azure Portal:

1. Seleccione **Azure Active Directory** > **Registro de aplicaciones**.
2. Busque el registro de su aplicación (p. ej.: OCPAzureAD).
3. En los resultados, haga clic en el registro de aplicación.
4. En **Configuración**, seleccione **Permisos necesarios**.
5. En **Permisos necesarios**, seleccione **Agregar**.

   ![Registro de aplicaciones](media/openshift-post-deployment/app-registration.png)

6. Haga clic en el paso 1: Seleccionar API y, a continuación, haga clic en **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Haga clic en **Seleccionar** en la parte inferior.

   ![Selección de API del registro de aplicaciones](media/openshift-post-deployment/app-registration-select-api.png)

7. En el paso 2: Seleccione los permisos, seleccione **iniciar sesión y leer el perfil de usuario** en **permisos delegados**y, a continuación, haga clic en **seleccione**.

   ![Acceso al registro de aplicaciones](media/openshift-post-deployment/app-registration-access.png)

8. Seleccione **Listo**.

### <a name="configure-openshift-for-azure-ad-authentication"></a>Configuración de OpenShift para la autenticación de Azure AD

Para configurar OpenShift para usar Azure AD como proveedor de autenticación, el archivo /etc/origin/master/master-config.yaml se debe editar en todos los nodos maestros.

Busque el identificador de inquilino mediante el siguiente comando de CLI:

```azurecli
az account show
```

En el archivo yaml, busque las siguientes líneas:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Inserte las líneas siguientes inmediatamente después de las líneas anteriores:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Asegúrese de que el texto se alinea correctamente en identityProviders. Busque el identificador de inquilino mediante el siguiente comando de CLI: ```az account show```.

Reinicie los servicios maestros de OpenShift en todos los nodos maestros:

**OpenShift Container Platform (OCP) con varios maestros**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform con un solo maestro**

```bash
sudo systemctl restart atomic-openshift-master
```

**OKD con varios maestros**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OKD con un solo maestro**

```bash
sudo systemctl restart origin-master
```

En la consola de OpenShift, ahora verá dos opciones para la autenticación: htpasswd_auth y [App Registration].

## <a name="monitor-openshift-with-azure-monitor-logs"></a>Supervisar OpenShift con registros de Azure Monitor

Hay tres maneras de agregar al agente de Log Analytics a OpenShift.
- Instalar el agente de Log Analytics para Linux directamente en cada nodo de OpenShift
- Habilitar la extensión de máquina virtual de Azure Monitor en cada nodo OpenShift
- Instalar el agente de Log Analytics como daemon-set de OpenShift

Las instrucciones completas se encuentran aquí: https://docs.microsoft.com/azure/log-analytics/log-analytics-containers#configure-a-log-analytics-agent-for-red-hat-openshift.

## <a name="configure-metrics-and-logging"></a>Configurar las métricas y el registro

En función de la rama, las plantillas de Azure Resource Manager para OpenShift Container Platform y OKD pueden proporcionar parámetros de entrada para habilitar las métricas y el registro como parte de la instalación.

La oferta de Marketplace de OpenShift Container Platform también proporciona una opción para habilitar las métricas y el registro durante la instalación del clúster.

Si las métricas o el registro no se han habilitado durante la instalación del clúster, se pueden habilitar fácilmente con posterioridad.

### <a name="ansible-inventory-pre-work"></a>Trabajo previo de inventario de Ansible

Compruebe que el archivo de inventario de Ansible (/etc/ansible/hosts) tiene las variables adecuadas para las métricas y el registro. El archivo de inventario puede encontrarse en diferentes hosts según la plantilla utilizada.

Para la plantilla de OpenShift Container y la oferta de Marketplace, el archivo de inventario se encuentra en el host de tipo bastión. Para la plantilla de OKD, el archivo de inventario se encuentra en el host master-0 o el host de tipo bastión en función de la rama en uso.

1. Edite el archivo /etc/ansible/hosts y agregue las líneas siguientes después de la sección de proveedor de identidad (# Enable HTPasswdPasswordIdentityProvider). Si ya están presentes estas líneas, no las agregue de nuevo.

   OpenShift y OKD versiones 3.9 y anteriores

   ```yaml
   # Setup metrics
   openshift_hosted_metrics_deploy=false
   openshift_metrics_cassandra_storage_type=dynamic
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"type":"infra"}
   openshift_metrics_cassandra_nodeselector={"type":"infra"}
   openshift_metrics_heapster_nodeselector={"type":"infra"}
   openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

   # Setup logging
   openshift_hosted_logging_deploy=false
   openshift_hosted_logging_storage_kind=dynamic
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"type":"infra"}
   openshift_logging_kibana_nodeselector={"type":"infra"}
   openshift_logging_curator_nodeselector={"type":"infra"}
   openshift_master_logging_public_url=https://kibana.$ROUTING
   ```

   OpenShift y OKD versiones 3.10 y posteriores

   ```yaml
   # Setup metrics
   openshift_metrics_install_metrics=false
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_metrics_cassandra_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_metrics_heapster_nodeselector={"node-role.kubernetes.io/infra":"true"}

   # Setup logging
   openshift_logging_install_logging=false
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_kibana_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_curator_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_master_public_url=https://kibana.$ROUTING
   ```

3. Reemplace $ROUTING con la cadena usada para la opción openshift_master_default_subdomain en el mismo archivo /etc/ansible/hosts.

### <a name="azure-cloud-provider-in-use"></a>Proveedor de soluciones en la nube de Azure en uso

Conéctese mediante SSH al nodo de tipo bastión o al primer nodo maestro (en función de la plantilla y la rama en uso) con las credenciales proporcionadas durante la implementación. Ejecute el siguiente comando:

**OpenShift Container Platform 3.7 y versiones anteriores**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

**OpenShift Container Platform 3.9 y versiones posteriores**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

**OKD 3.7 y versiones anteriores**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

**OKD 3.9 y versiones posteriores**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook ~/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

### <a name="azure-cloud-provider-not-in-use"></a>Proveedor de soluciones en la nube de Azure que no está en uso

Conéctese mediante SSH al nodo de tipo bastión o al primer nodo maestro (en función de la plantilla y la rama en uso) con las credenciales proporcionadas durante la implementación. Ejecute el siguiente comando:


**OpenShift Container Platform 3.7 y versiones anteriores**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True
```

**OpenShift Container Platform 3.9 y versiones posteriores**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

**OKD 3.7 y versiones anteriores**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True
```

**OKD 3.9 y versiones posteriores**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True
ansible-playbook ~/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

## <a name="install-open-service-broker-for-azure-osba"></a>Instalación de Open Service Broker para Azure (OSBA)

Open Service Broker para Azure (OSBA) le permite aprovisionar Azure Cloud Services directamente desde OpenShift. OSBA es una implementación de Open Service Broker API para Azure. Open Service Broker API es una especificación que define un lenguaje común para proveedores en la nube que las aplicaciones nativas pueden usar para administrar servicios en la nube sin dependencia del proveedor.

Para instalar OSBA en OpenShift, siga las instrucciones que encontrará aquí: https://github.com/Azure/open-service-broker-azure#openshift-project-template. 

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a OpenShift Container Platform](https://docs.openshift.com/container-platform)
- [Introducción a OKD](https://docs.okd.io/latest)
