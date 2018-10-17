---
title: Creación de Cloud Foundry en Azure
description: Aprenda a configurar los parámetros necesarios para aprovisionar un clúster PCF de Cloud Foundry en Azure.
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: Cloud Foundry
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: a0a3379a8a2579080d9b686917395feec9cf8f3d
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48250675"
---
# <a name="create-cloud-foundry-on-azure"></a>Creación de Cloud Foundry en Azure

En este tutorial se proporcionan pasos rápidos para crear y generar los parámetros necesarios para aprovisionar un clúster Pivotal Cloud Foundry (PCF) en Azure.  La solución Pivotal Cloud Foundry se puede encontrar realizando una búsqueda en Azure [MarketPlace](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

![Texto de imagen alternativo](media/deploy/pcf-marketplace.png "Búsqueda de Pivotal Cloud Foundry en Azure")


## <a name="generate-an-ssh-public-key"></a>Generación de una clave pública SSH

Hay varias maneras de generar una clave pública SSH mediante Windows, Mac o Linux.

```Bash
ssh-keygen -t rsa -b 2048
```
- Haga clic aquí para ver las [instrucciones]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) correspondientes a su entorno.

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

> [!NOTE]
>
> Para crear una entidad de servicio se requiere un permiso de propietario de cuenta.  Además, puede escribir un script para automatizar la creación de la entidad de servicio. Por ejemplo, mediante [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest) de la CLI de Azure.

1. Inicie sesión en la cuenta de Azure.

    `az login`

    ![Texto de imagen alternativo](media/deploy/az-login-output.png "Inicio de sesión de la CLI de Azure")
 
    Copie el valor de “id” como **identificador de suscripción** y el valor de **tenantId** para usarlo más adelante.

2. Establezca la suscripción predeterminada de esta configuración.

    `az account set -s {id}`

3. Cree una aplicación de AAD para su PCF y especifique una contraseña alfanumérica única.  Almacene la contraseña como **clientSecret** para usarla más adelante.

    `az ad app create --display-name "Svc Prinicipal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    A continuación, copie el valor "appId" de la salida como el valor de **ClientID** que usará más adelante.

    > [!NOTE]
    >
    > Elija su propia página principal de la aplicación y el URI de identificador.  Por ejemplo: http://www.contoso.com.

4. Cree una entidad de servicio con su nuevo "appId".

    `az ad sp create --id {appId}`

5. Establezca el rol de permiso de la entidad de seguridad como **Colaborador**.

    `az role assignment create --assignee “{enter-your-homepage}” --role “Contributor” `

    O, también puede usar…

    `az role assignment create --assignee {service-princ-name} --role “Contributor” `

    ![Texto de imagen alternativo](media/deploy/svc-princ.png "Asignación de rol de la entidad de servicio")

6. Compruebe que puede iniciar sesión correctamente en la entidad de servicio mediante los valores de appId, password y tenantId.

    `az login --service-principal -u {appId} -p {your-passward}  --tenant {tenantId}`

7. Cree un archivo .json con el siguiente formato con todos los valores de **identificador de suscripción**, **tenantId**, **clientID** y **clientSecret** que ha copiado anteriormente.  Guarde el archivo.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Obtención del token de Pivotal Network

1. Regístrese o inicie sesión en su cuenta de [Pivotal Network](https://network.pivotal.io).
2. Haga clic en el nombre del perfil en la parte superior derecha de la página y seleccione **Edit Profile" (Editar perfil).
3. Desplácese hasta la parte inferior de la página y copie el valor **LEGACY API TOKEN** (TOKEN DE API HEREDADO).  Este es el valor de **Pivotal Network Token** (Token de Pivotal Network) que se usará posteriormente.

## <a name="provision-your-cloud-foundry-on-azure"></a>Aprovisionamiento de una instancia de Cloud Foundry en Azure

1. Ahora tiene todos los parámetros necesarios para aprovisionar su clúster de [Pivotal Cloud Foundry en Azure](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).
2. Especifique los parámetros y cree el clúster PCF.

## <a name="verify-the-deployment-and-log-into-the-pivotal-ops-manager"></a>Comprobación de la implementación y el inicio de sesión en Pivotal Ops Manager

1. El clúster PCF debe mostrar un estado de implementación.

    ![Texto de imagen alternativo](media/deploy/deployment.png "Estado de implementación de Azure")

2. Haga clic en el vínculo **Deployments** (Implementaciones) en el panel de navegación izquierdo para obtener las credenciales de PCF Ops Manager y, luego, haga clic en el **nombre de la implementación** en la página siguiente.
3. En el panel de navegación izquierdo, haga clic en el vínculo **Outputs** (Salidas) para mostrar la dirección URL, el nombre de usuario y la contraseña de PCF Ops Manager.  El valor "OPSMAN-FQDN" es la dirección URL.
 
    ![Texto de imagen alternativo](media/deploy/deploy-outputs.png "Salida de la implementación de Cloud Foundry")
 
4. Vaya a la dirección URL en un explorador web y escriba las credenciales del paso anterior para iniciar sesión.

    ![Texto de imagen alternativo](media/deploy/pivotal-login.png "Página de inicio de sesión de Pivotal")
         
    > [!NOTE]
    >
    > Si se produce un error en el Explorador de Internet Explorer debido a un mensaje de advertencia de sitio no seguro, haga clic en "Más información" y "vaya a la página web.  En Firefox, haga clic en Avanzadas y agregue la certificación para continuar.

5. La instancia de PCF Ops Manager mostrará las instancias de Azure implementadas. Ahora puede empezar a implementar y administrar las aplicaciones aquí.
               
    ![Texto de imagen alternativo](media/deploy/ops-mgr.png "Instancia de Azure implementada en Pivotal")
 
