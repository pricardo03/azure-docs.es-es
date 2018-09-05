---
title: Implementación de un clúster de Kubernetes en Azure Stack | Microsoft Docs
description: Aprenda a implementar un clúster de Kubernetes en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 67e82a4809253123e15305b5f2ecd8cc5f7ee4ed
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046857"
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Implementación de un clúster de Kubernetes en Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

> [!Note]  
> Kubernetes de Azure Kubernetes Service (AKS) en Azure Stack se encuentra en versión preliminar privada. El operador de Azure Stack debe solicitar acceso al elemento Kubernetes de Marketplace necesario para seguir las instrucciones de este artículo.

En el artículo siguiente se explica cómo usar una plantilla de la solución Azure Resource Manager para implementar y aprovisionar los recursos para Kubernetes en una única operación coordinada. Necesita recopilar la información necesaria sobre la instalación de Azure Stack, generar la plantilla y, a continuación, implementar en la nube.

## <a name="kubernetes-and-containers"></a>Kubernetes y contenedores

Puede instalar Kubernetes mediante las plantillas de Azure Resource Manager generadas por el motor de Azure Kubernetes Services (AKS) en Azure Stack. [Kubernetes](https://kubernetes.io) es un sistema de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones en contenedores. Un [contenedor](https://www.docker.com/what-container) se encuentra en una imagen, similar a una VM. A diferencia de una máquina virtual, la imagen de contenedor tan solo incluye los recursos necesarios para ejecutar una aplicación, como el código, el tiempo de ejecución para ejecutar el código, bibliotecas específicas y la configuración.

Puede utilizar Kubernetes para:

- Desarrollar aplicaciones escalables de forma masiva y actualizables que se pueden implementar en segundos. 
- Simplificar el diseño de la aplicación y mejorar la fiabilidad mediante distintas aplicaciones de Helm. [Helm](https://github.com/kubernetes/helm) es una herramienta de empaquetado de código abierto que ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes.
- Supervisar y diagnosticar fácilmente el estado de las aplicaciones con una funcionalidad de escalado y actualización.

## <a name="prerequisites"></a>Requisitos previos 

Para empezar, asegúrese de tener los permisos adecuados y de que la instancia de Azure Stack está lista.

1. Compruebe que puede crear aplicaciones en el inquilino de Azure Active Directory (Azure AD). Necesitará estos permisos para la implementación de Kubernetes.

    Para obtener instrucciones sobre cómo comprobar sus permisos, consulte [Comprobación de los permisos de Azure Active Directory](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

1. Genere un par de claves SSH pública y privada para iniciar sesión en la máquina virtual de Linux en Azure Stack. Necesitará la clave pública al crear el clúster.

    Para obtener instrucciones sobre cómo generar una clave, consulte [SSH Key Generation](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation) (Generación de claves SSH).

1. Compruebe que tiene una suscripción válida en el portal del inquilino de Azure Stack y que tiene suficientes direcciones IP públicas disponibles para agregar nuevas aplicaciones.

    No se puede implementar el clúster en una suscripción de **administrador** de Azure Stack. Debe usar una suscripción de Usuario**. 

## <a name="create-a-service-principal-in-azure-ad"></a>Creación de una entidad de servicio en Azure AD

1. Inicie sesión en [Azure Portal](http://portal.azure.com) global.
1. Compruebe que inició sesión con el inquilino de Azure AD asociado con la instancia de Azure Stack.
1. Crear una aplicación de Azure.

    a. Seleccione **Azure Active Directory** > **+ Registros de aplicaciones** > **Nuevo registro de aplicaciones**.

    b. Escriba un **Nombre** para la aplicación.

    c. Seleccione **Aplicación web/API**.

    d. Escriba `http://localhost` para **URL de inicio de sesión**.

    c. Haga clic en **Create**(Crear).

1. Anote el **Id. de la aplicación**. Necesitará el identificador al crear el clúster. Se hace referencia al identificador como **Id. de cliente de entidad de servicio**.

1. Seleccione **Configuración** > **Claves**.

    a. Escriba la **Descripción**.

    b. Seleccione **Never expires** (Nunca expira) para **Expira**.

    c. Seleccione **Guardar**. Tome nota de la cadena de clave. Necesitará la cadena de clave al crear el clúster. Se hace referencia a la clave como **Secreto de cliente de la entidad de servicio**.



## <a name="give-the-service-principal-access"></a>Dar acceso a la entidad de servicio

Dé a la entidad de servicio acceso a su suscripción para que la entidad pueda crear recursos.

1.  Inicie sesión en el [portal de Azure Stack](https://portal.local.azurestack.external/).

1. Seleccione **Más servicios**  > **Suscripciones**.

1. Seleccione la suscripción que ha creado.

1. Seleccione **Control de acceso (IAM)** > seleccione **+ Agregar**.

1. Seleccione el rol **Propietario**.

1. Seleccione el nombre de aplicación creado para la entidad de servicio. Puede que tenga que escribir el nombre en el cuadro de búsqueda.

1. Haga clic en **Save**(Guardar).

## <a name="deploy-a-kubernetes-cluster"></a>Implementación de un clúster de Kubernetes

1. Abra el [portal de Azure Stack](https://portal.local.azurestack.external).

1. Seleccione **+ Nuevo** > **Proceso** > **Clúster de Kubernetes**. Haga clic en **Create**(Crear).

    ![Implementar plantilla de solución](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

1. Seleccione **Conceptos básicos** en Creación de un clúster de Kubernetes.

    ![Implementar plantilla de solución](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Escriba el **nombre de usuario del administrador de la máquina virtual Linux**. Nombre de usuario de las máquinas virtuales de Linux que forman parte del clúster de Kubernetes y DVM.

1. Escriba la **clave pública SSH** utilizada para la autorización en todas las máquinas Linux creadas como parte del clúster de Kubernetes y DVM.

1. Escriba el **prefijo del DNS del perfil maestro** que es único para la región. Debe ser un nombre único para la región, como `k8s-12345`. Como procedimiento recomendado, intente elegirlo de modo que sea el mismo que el nombre del grupo de recursos.

    > [!Note]  
    > Para cada clúster, use un prefijo de DNS del perfil principal nuevo y único.

1. Escriba el **recuento de perfiles del grupo de agentes**. El recuento contiene el número de agentes en el clúster. Puede haber de 1 a 4.

1. Escriba el **id. de cliente de la entidad de servicio**. El proveedor de nube de Azure Kubernetes usa este identificador.

1. Escriba el **secreto de cliente de la entidad de servicio** que creó al crear la aplicación de la entidad de servicio.

1. Escriba la **versión del proveedor de nube de Azure Kubernetes**. Esta es la versión para el proveedor de Kubernetes Azure. Azure Stack publica una compilación de Kubernetes personalizada para cada versión de Azure Stack.

1. Seleccione el identificador de **suscripción**.

1. Escriba el nombre del nuevo grupo de recursos o seleccione uno existente. El nombre del recurso debe ser alfanumérico y estar en minúsculas.

1. Seleccione la **ubicación** del grupo de recursos. Esta es la región que elige para la instalación de Azure Stack.

### <a name="specify-the-azure-stack-settings"></a>Especifique la configuración de Azure Stack

1. Seleccione la **configuración de marca de Azure Stack**.

    ![Implementar plantilla de solución](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings.png)

1. Escriba el **punto de conexión ARM del inquilino**. Este es el punto de conexión de Azure Resource Manager para conectarse para crear el grupo de recursos para el clúster de Kubernetes. Debe obtener el punto de conexión del operador de Azure Stack para un sistema integrado. Para obtener el Kit de desarrollo de Azure Stack (ASDK), puede usar `https://management.local.azurestack.external`.

1. Escriba el **identificador de inquilino** de este. Si necesita ayuda para encontrar este valor, consulte [Obtención del identificador de inquilino](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

## <a name="connect-to-your-cluster"></a>Conexión al clúster

Ahora está listo para conectarse al clúster. La máquina virtual maestra se puede encontrar en el grupo de recursos del clúster y se denomina `k8s-master-<sequence-of-numbers>`. Use un cliente de SSH para conectarse a la máquina virtual maestra. En ella, puede usar **kubectl**, el cliente de línea de comandos de Kubernetes, para administrar el clúster. Para obtener instrucciones, consulte [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Puede que también le resulte útil el administrador de paquetes **Helm** para instalar e implementar aplicaciones en el clúster. Para más instrucciones sobre cómo instalar y usar Helm con el clúster, consulte [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Pasos siguientes

[Agregar un clúster de Kubernetes a Marketplace (para el operador de Azure Stack)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes en Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
