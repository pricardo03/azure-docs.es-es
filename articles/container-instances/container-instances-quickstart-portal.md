---
title: 'Guía de inicio rápido: Ejecución de una aplicación en Azure Container Instances: Portal'
description: En esta guía de inicio rápido, va a usar Azure Portal para implementar una aplicación de contenedor de Docker que se ejecutará en un contenedor aislado de Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: d6a1d442eca0cf5e433a82fb52ed54b09b56c779
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55566126"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-in-the-azure-portal"></a>Guía de inicio rápido: Ejecución de una aplicación de contenedor en Azure Container Instances con Azure Portal

Use Azure Container Instances para ejecutar contenedores de Docker en Azure con sencillez y velocidad. No es necesario implementar máquinas virtuales ni usar una plataforma de orquestación de contenedores completa como Kubernetes. En esta guía de inicio rápido, va a usar Azure Portal para crear un contenedor en Azure y hacer que su aplicación esté disponible con un nombre de dominio completo (FQDN). Después de configurar algunos valores e implementar el contenedor, puede ir a la aplicación en ejecución:

![Aplicación implementada en Azure Container Instances vista en el explorador][aci-portal-07]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

Si no tiene ninguna suscripción a Azure, cree una [cuenta gratuita][azure-free-account] antes de empezar.

## <a name="create-a-container-instance"></a>Creación de instancia de contenedor

Seleccione **Crear un recurso** > **Contenedores** > **Container Instances**.

![Comenzar a crear una instancia de contenedor nueva en Azure Portal][aci-portal-01]

Escriba los valores siguientes en los cuadros de texto **Nombre de contenedor**, **Imagen de contenedor** y **Grupo de recursos**. Deje los otros valores predeterminados y haga clic en **Aceptar**.

* Nombre de contenedor: `mycontainer`
* Imagen de contenedor: `microsoft/aci-helloworld`
* Grupo de recursos: **Crear nuevo** > `myResourceGroup`

![Configuración básica de una instancia de contenedor nueva en Azure Portal][aci-portal-03]

En este artículo de inicio rápido, deje el valor predeterminado **Público** para implementar la imagen `microsoft/aci-helloworld` del registro público de Docker Hub. Esta imagen empaqueta una pequeña aplicación web escrita en Node.js que sirve una página HTML estática.

En **Configuración**, especifique una **etiqueta de nombre DNS** para el contenedor. El nombre debe ser único dentro de la región de Azure en la que crea la instancia de contenedor. El contenedor estará públicamente accesible en `<dns-name-label>.<region>.azurecontainer.io`.

Deje los otros valores en **Configuration** en su configuración predeterminada y seleccione **Aceptar** para validar la configuración.

![Configuración de una instancia de contenedor nueva en Azure Portal][aci-portal-04]

Cuando se complete la validación, verá un resumen de la configuración del contenedor. Seleccione **Aceptar** para enviar la solicitud de implementación de contenedor.

![Resumen de la configuración de una instancia de contenedor nueva en Azure Portal][aci-portal-05]

Cuando se inicia una implementación, aparece una notificación que indica que la implementación está en curso. Aparecerá otra notificación cuando se haya implementado el grupo de contenedores.

![Progreso de la creación de una instancia de contenedor nueva en Azure Portal][aci-portal-08]

Para abrir la información general del grupo de contenedores, vaya a **Grupos de recursos** > **myResourceGroup** > **mycontainer**. Tome nota del **FQDN** (el nombre de dominio completo) de la instancia de contenedor y también de su **estado**.

![Información general del grupo de contenedores en Azure Portal][aci-portal-06]

Cuando el **Estado** sea *En ejecución*, vaya al FQDN del contenedor en el explorador.

![Aplicación implementada mediante Azure Container Instances vista en el explorador][aci-portal-07]

Felicidades. Con tan solo implementar algunos valores, ha implementado una aplicación de acceso público en Azure Container Instances.

## <a name="view-container-logs"></a>Visualización de registros de contenedores

Ver los registros de una instancia de contenedor resulta de utilidad al solucionar problemas con el contenedor o la aplicación en la que se ejecuta.

Para ver los registros del contenedor, en **Configuración**, seleccione **Contenedores** y luego **Registros**. Debería ver la solicitud GET HTTP que se genera cuando se ve la aplicación en el explorador.

![Registros de contenedor en Azure Portal][aci-portal-11]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el contenedor, seleccione **Introducción** para el contenedor *mycontainer* y luego seleccione **Eliminar**.

![Eliminación de una instancia de contenedor en Azure Portal][aci-portal-09]

Seleccione **Sí** cuando aparezca el cuadro de diálogo de confirmación.

![Eliminar la confirmación de una instancia de contenedor en Azure Portal][aci-portal-10]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una instancia de contenedor de Azure a partir de una imagen en un repositorio público de Docker Hub. Si quiere compilar una imagen de contenedor e implementarla desde un registro de contenedor privado de Azure, vaya al tutorial de Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial de Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/