---
title: Integrar entornos en canalizaciones de Azure en Azure DevTest Labs | Microsoft Docs
description: Aprenda a integrar entornos de Azure DevTest Labs en su Azure DevOps, integración continua (CI) y las canalizaciones de entrega continua (CD).
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: 24391e9e8541f12c434ade837b8f4944711ae375
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887239"
---
# <a name="integrate-environments-into-your-azure-devops-cicd-pipelines"></a>Integrar entornos en las canalizaciones de CI/CD de Azure DevOps
Puede usar la extensión de Azure DevTest Labs Tasks que está instalada en los servicios de DevOps de Azure (anteriormente conocido como Visual Studio Team Services) para integrar fácilmente la integración continua (CI) / canalización de compilación y versión entrega continua (CD) con Azure DevTest Labs. Estas extensiones que sea más fácil implementar rápidamente un [entorno](devtest-lab-test-env.md) una determinada tarea de prueba y, a continuación, eliminarlo cuando finalice la prueba. 

En este artículo se muestra cómo crear, implementar un entorno y, a continuación, eliminar el entorno, todo en una canalización completa. Lo habitual sería realizar cada una de estas tareas individualmente en su propia canalización de compilación-probar-implementar personalizada. Las extensiones usadas en este artículo son además de estas [crear o eliminar tareas DTL VM](devtest-lab-integrate-ci-cd-vsts.md):

- Crear un entorno
- Eliminar un entorno

## <a name="before-you-begin"></a>Antes de empezar
Antes de integrar la canalización CI/CD con Azure DevTest Labs, instalar [Azure DevTest Labs Tasks](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) extensión de Visual Studio Marketplace. 

## <a name="create-and-configure-the-lab-for-environments"></a>Crear y configurar el laboratorio para entornos
En esta sección se describe cómo crear y configurar un laboratorio donde se implementará en el entorno de Azure.

1. [Creación de un laboratorio](devtest-lab-create-lab.md) si aún no tiene uno. 
2. Configurar el laboratorio y crear una plantilla de entorno siguiendo las instrucciones de este artículo: [Creación de entornos de varias máquinas virtuales y los recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md).
3. En este ejemplo, usar una plantilla de inicio rápido de Azure existente [ https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/ ](https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/).
4. Copia el **201-web-app-redis-cache-sql-database** carpeta en el **ArmTemplate** carpeta en el repositorio que configuró en el paso 2.

## <a name="create-a-release-definition"></a>Creación de una definición de versión
Para crear la definición de la versión, siga estos pasos:

1.  En el **versiones** pestaña de la **concentrador de compilaciones y versiones**, seleccione el **signo más (+)** botón.
2.  En la ventana **Crear definición de versión**, seleccione la plantilla **Vacía** y, luego, seleccione **Siguiente**.
3.  Seleccione **Elegir más tarde** y, luego, seleccione **Crear** para crear una nueva definición de versión con un entorno predeterminado y ningún artefacto vinculado.
4.  Para abrir el menú contextual, en la nueva definición de versión, seleccione el **puntos suspensivos (...)**  junto al nombre de entorno y, a continuación, seleccione **configurar variables**.
5.  En la ventana de **configuración del entorno**, escriba los siguientes valores para las variables que se usan en las tareas de definición de versión:
1.  Para **administratorLogin**, escriba el nombre de inicio de sesión de administrador de SQL.
2.  Para **administratorLoginPassword**, escriba la contraseña que va a usar el inicio de sesión de administrador de SQL. Use el icono de "candado" para ocultar y proteger la contraseña.
3.  Para **databaseName**, escriba el nombre de la base de datos SQL.
4.  Estas variables son específicas para los entornos de ejemplo, entornos diferentes pueden tener distintas variables.

## <a name="create-an-environment"></a>Creación de un entorno
La siguiente fase de la implementación es crear el entorno que se usará para el desarrollo o con fines de prueba.

1. En la definición de versión, seleccione **Agregar tareas**.
2. En el **tareas** pestaña, agregue una tarea de crear entorno de Azure DevTest Labs. Configure la tarea de la siguiente manera:
    1. En **Suscripción de Azure Resource Manager**, seleccione una conexión en la lista **Conexiones a servicios de Azure disponibles**, o cree una conexión con permisos más restrictivos a la suscripción de Azure. Para más información, consulte [Azure Resource Manager service endpoint](/devops/pipelines/library/service-endpoints) (Punto de conexión de servicio de Azure Resource Manager).
2. Para **nombre de laboratorio**, seleccione el nombre de la instancia que creó anteriormente *.
3. Para **nombre del repositorio**, seleccione el repositorio donde se ha insertado la plantilla de Resource Manager (201) en *.
4. Para **nombre de la plantilla**, seleccione el nombre del entorno que guardó en el repositorio de código fuente *. 
5. El **nombre de laboratorio**, **nombre del repositorio**, y **nombre de la plantilla** son las representaciones descriptivas de los identificadores de recursos de Azure. Escribir manualmente el nombre descriptivo provoca errores, use las listas desplegables para seleccionar la información.
6. Para **nombre del entorno**, escriba un nombre para identificar de forma única la instancia del entorno del laboratorio.  Debe ser único dentro del laboratorio.
7. El **archivo de parámetros** y **parámetros**, permitir que los parámetros personalizados que se pasarán al entorno. Uno o ambos pueden usarse para establecer los valores de parámetro. En este ejemplo, se usará la sección de parámetros. Use los nombres de las variables que definió en el entorno, por ejemplo: `-administratorLogin “$(administratorLogin)” -administratorLoginPassword “$(administratorLoginPassword)” -databaseName “$(databaseName)” -cacheSKUCapacity 1`
8. Información dentro de la plantilla de entorno se puede pasar a través en la sección de salida de la plantilla. Comprobar **crear variables de salida basadas en la salida de la plantilla de entorno** por lo que pueden usar los datos de otras tareas. `$(Reference name.Output Name)` es el modelo a seguir. Por ejemplo, si el nombre de referencia se DTL y el nombre de salida en la plantilla era la ubicación de la variable sería `$(DTL.location)`.

## <a name="delete-the-environment"></a>Eliminar el entorno
La fase final consiste en eliminar el entorno que implementó en la instancia de Azure DevTest Labs. Lo habitual es eliminar el entorno después de ejecutar las tareas de desarrollo o ejecutar las pruebas que necesita en los recursos implementados.

En la definición de versión, seleccione **agregar tareas**y, a continuación, en el **implementar** pestaña, agregue un **eliminar entorno de Azure DevTest Labs** tarea. Configúrelo de la siguiente forma:

1. Para eliminar la máquina virtual, consulte [Azure DevTest Labs Tasks](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks):
    1. En **Suscripción de Azure Resource Manager**, seleccione una conexión en la lista **Conexiones a servicios de Azure disponibles**, o cree una conexión con permisos más restrictivos a la suscripción de Azure. Para más información, consulte [Azure Resource Manager service endpoint](/devops/pipelines/library/service-endpoints) (Punto de conexión de servicio de Azure Resource Manager).
    2. Para **nombre de laboratorio**, seleccione aquel donde existe el entorno.
    3. Para **nombre del entorno**, escriba el nombre del entorno que se va a quitar.
2. Escriba un nombre para la definición de versión y, a continuación, guárdelo.

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 
- [Crear entornos de varias máquinas virtuales con plantillas de Resource Manager](devtest-lab-create-environment-from-arm.md).
- Plantillas de inicio rápido de Resource Manager para la automatización de DevTest Labs desde la [repositorio de GitHub de DevTest Labs](https://github.com/Azure/azure-quickstart-templates).
- [Página de solución de problemas de VSTS](/devops/pipelines/troubleshooting)

