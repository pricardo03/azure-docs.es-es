---
title: Integración de Azure Functions con Azure virtual network
description: Un tutorial paso a paso que se muestra cómo conectarse a una función a una red virtual de Azure
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: article
ms.date: 5/03/2019
ms.author: alkarche, glenga
ms.openlocfilehash: 07c7d7fb682708bf813820440d9c790c28b1f3e5
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834576"
---
# <a name="tutorial-integrate-functions-with-an-azure-virtual-network"></a>Tutorial: integración de funciones con una red virtual de Azure

Este tutorial muestra cómo usar Azure Functions para conectarse a recursos en una red virtual de Azure. creación de una función que tiene acceso a internet y a una máquina virtual con WordPress en la red virtual.

> [!div class="checklist"]
> * Crear una aplicación de función en el plan Premium
> * Implementar un sitio de WordPress en la máquina virtual en una red virtual
> * Conectar la aplicación de función a la red virtual
> * Crear un proxy de función para acceder a recursos de WordPress
> * Solicite un archivo de WordPress desde dentro de la red virtual

> [!NOTE]  
> Este tutorial crea una aplicación de función en el plan Premium. Este plan de hospedaje está actualmente en versión preliminar. Para obtener más información, consulte [plan Premium].

## <a name="topology"></a>Topología

El siguiente diagrama muestra la arquitectura de la solución que cree:

 ![Interfaz de usuario para la integración de red virtual](./media/functions-create-vnet/topology.png)

Funciones que se ejecutan en el plan Premium tienen las mismas capacidades de hospedaje como aplicaciones web en Azure App Service, que incluye la característica de integración con red virtual. Para obtener más información sobre la integración con red virtual, incluida la configuración avanzada y solución de problemas, consulte [integrar su aplicación con una red virtual](../app-service/web-sites-integrate-with-vnet.md).

## <a name="prerequisites"></a>Requisitos previos

Para este tutorial, es importante que comprenda el direccionamiento IP y subredes. Puede comenzar con [en este artículo que trata los aspectos básicos del direccionamiento y las subredes](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Muchos que más artículos y vídeos están disponibles en línea.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-function-app-in-a-premium-plan"></a>Crear una aplicación de función en un plan Premium

En primer lugar, creará una aplicación de función en el [plan Premium]. Este plan ofrece escala sin servidor, al tiempo que admite la integración de red virtual.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

Puede anclar la aplicación de función al panel seleccionando el icono de anclaje en la esquina superior derecha. Anclar resulta más fácil volver a esta aplicación de función después de crear la máquina virtual.

## <a name="create-a-vm-inside-a-virtual-network"></a>Crear una máquina virtual dentro de una red virtual

A continuación, cree una máquina virtual preconfigurada que se ejecute WordPress en una red virtual ([WordPress LEMP7 Max Performance](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure) por Jetware). Se usa una VM WordPress debido a su bajo costo y la comodidad. Este mismo escenario funciona con cualquier recurso en una red virtual, como las API de REST, entornos de App Service y otros servicios de Azure. 

1. En el portal, elija **+ crear un recurso** en el panel de navegación izquierdo, en el tipo de campo de búsqueda `WordPress LEMP7 Max Performance`, y presione ENTRAR.

1. Elija **Wordpress LEMP Max Performance** en los resultados de búsqueda. Seleccione un plan de software de **Wordpress LEMP Max Performance para CentOS** como el **Plan de Software** y seleccione **crear**.

1. En el **Fundamentos** , utilice la configuración de máquina virtual como se especifica en la tabla debajo de la imagen:

    ![Pestaña aspectos básicos para crear una máquina virtual](./media/functions-create-vnet/create-vm-1.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN      |
    | ------------ | ---------------- | ---------------- |
    | **Suscripción** | Su suscripción | La suscripción en la que se crean los recursos. | 
    | **[Grupo de recursos](../azure-resource-manager/resource-group-overview.md)**  | myResourceGroup | Elija `myResourceGroup`, o el grupo de recursos que creó con la aplicación de función. Con el mismo grupo de recursos para la aplicación de función, WordPress VM y el plan de hospedaje resulta más fácil limpiar los recursos cuando haya terminado con este tutorial. |
    | **Nombre de la máquina virtual** | VNET-Wordpress | El nombre de la máquina virtual debe ser único en el grupo de recursos |
    | **[Región](https://azure.microsoft.com/regions/)** | (Europa) Europa occidental | Elija una región cerca de usted o cerca de las funciones que tienen acceso a la máquina virtual. |
    | **Tamaño** | B1s | Elija **cambiar tamaño** y, a continuación, seleccione la imagen B1s estándar, que tiene 1 vCPU y 1 GB de memoria. |
    | **Tipo de autenticación** | Contraseña | Para usar la autenticación de contraseña, también debe especificar un **Username**, un seguro **contraseña**y, a continuación, **Confirmar contraseña**. Para este tutorial, no tendrá que iniciar sesión en la máquina virtual, a menos que deba solucionar el problema. |

1. Elija la **redes** pestaña y en redes virtuales de configurar Active **crear nuevo**.

1. En **crear red virtual**, use la configuración de la tabla debajo de la imagen:

    ![Creación de la pestaña red de máquina virtual](./media/functions-create-vnet/create-vm-2.png)

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | **Nombre** | myResourceGroup-vnet | Puede usar el nombre predeterminado generado para la red virtual. |
    | **Intervalo de direcciones** | 10.10.0.0/16 | Use un intervalo de dirección única para la red virtual. |
    | **Nombre de subred** | Tutorial-Net | Nombre de la subred. |
    | **Intervalo de direcciones** (subred) | 10.10.1.0/24   | El tamaño de la subred define cuántas interfaces se pueden agregar a la subred. Esta subred se usa el sitio de WordPress.  Un `/24` subred proporciona 254 direcciones de host. |

1. Seleccione **Aceptar** para crear la red virtual.

1. En el **redes** ficha, elija **ninguno** para **dirección IP pública**.

1. Elija la **administración** ficha, a continuación, en **cuenta de almacenamiento de diagnóstico**, elija la cuenta de almacenamiento que creó con la aplicación de función.

1. Seleccione **Revisar + crear**. Una vez finalizada la validación, seleccione **crear**. La máquina virtual crear proceso tarda unos minutos. La máquina virtual creada solo puede tener acceso a la red virtual.

1. Una vez creada la máquina virtual, elija **ir al recurso** para ver la página de la nueva máquina virtual, a continuación, elija **redes** en **configuración**.

1. Compruebe que no haya ningún **dirección IP pública**. Tome nota del **IP privada**, que se utiliza para conectarse a la máquina virtual desde la aplicación de función.

    ![Configuración de red en la máquina virtual](./media/functions-create-vnet/vm-networking.png)

Ahora tiene un sitio de WordPress que se implementa completamente dentro de la red virtual. Este sitio no es accesible desde internet público.

## <a name="connect-your-function-app-to-the-virtual-network"></a>Conectar la aplicación de función a la red virtual

Con un sitio de WordPress que se ejecuta en una máquina virtual en una red virtual, ahora puede conectarse la aplicación de función a la red virtual.

1. En la nueva function app, seleccione **características de la plataforma** > **redes**.

    ![Elegir una red en la aplicación de función](./media/functions-create-vnet/networking-0.png)

1. En **integración con red virtual**, seleccione **haga clic aquí para configurar**.

    ![Estado de configuración de una característica de red](./media/functions-create-vnet/Networking-1.png)

1. En la página de integración de red virtual, seleccione **agregar red virtual (versión preliminar)**.

    ![Agregar la vista previa de la integración con red virtual](./media/functions-create-vnet/networking-2.png)

1. En **estado de la característica de red**, use la configuración de la tabla debajo de la imagen:

    ![Definir la red virtual de aplicación de función](./media/functions-create-vnet/networking-3.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN      |
    | ------------ | ---------------- | ---------------- |
    | **Virtual Network** | MyResourceGroup-vnet | Esta red virtual es el que creó anteriormente. |
    | **Subred** | Crear nueva subred | Cree una subred en la red virtual para su aplicación de función. Integración con red virtual debe configurarse para usar una subred vacía. No importa que sus funciones usan una subred diferente a la máquina virtual. La red virtual enruta automáticamente el tráfico entre las dos subredes. |
    | **Nombre de subred** | Función-Net | Nombre de la nueva subred. |
    | **Bloque de direcciones de red virtual** | 10.10.0.0/16 | Elija el mismo bloque de direcciones utilizado por el sitio de WordPress. Solo debe tener un bloque de direcciones definido. |
    | **Intervalo de direcciones** | 10.10.2.0/24   | El tamaño de la subred restringe el número total de instancias de la aplicación de función del plan Premium puede escalar horizontalmente. Este ejemplo se usa un `/24` subred con 254 direcciones disponibles en el host. Esta subred es un aprovisionamiento excesivo, pero es fácil de calcular. |

1. Seleccione **Aceptar** para agregar la subred. Cierre las páginas de integración con red virtual y el estado de la característica de red para volver a la página de aplicación de función.

Ahora la aplicación de función puede tener acceso a la red virtual donde se está ejecutando el sitio de WordPress. A continuación, utilice [Azure Functions Proxies](functions-proxies.md) para devolver un archivo desde el sitio de WordPress.

## <a name="create-a-proxy-to-access-vm-resources"></a>Crear a un proxy para acceder a los recursos de máquina virtual

Integración con red virtual habilitada, puede crear a un proxy en la aplicación de función para reenviar solicitudes a la máquina virtual que se ejecuta en la red virtual.

1. En la aplicación de función, seleccione **Proxies** > **+**, a continuación, use la configuración de proxy de la tabla debajo de la imagen:

    ![Definir la configuración de proxy](./media/functions-create-vnet/create-proxy.png)

    | Configuración  | Valor sugerido  | Descripción      |
    | -------- | ---------------- | ---------------- |
    | **Nombre** | Planta | El nombre puede ser cualquier valor. Sirve para identificar al servidor proxy. |
    | **Plantilla de ruta** | /plant | Ruta que se asigna a un recurso de máquina virtual. |
    | **Dirección URL de back-end** | http://<YOUR_VM_IP>/wp-content/themes/twentyseventeen/assets/images/header.jpg | Reemplace `<YOUR_VM_IP>` con la dirección IP de la VM de WordPress que creó anteriormente. Esta asignación devuelve un único archivo desde el sitio. |

1. Seleccione **crear** para agregar el proxy para la aplicación de función.

## <a name="try-it-out"></a>Probarlo

1. En el explorador, intenta obtener acceso a la dirección URL que se usó como la **dirección URL de back-end**. Según lo previsto, la solicitud agota el tiempo. Un tiempo de espera se produce porque el sitio de WordPress se conecta únicamente a la red virtual y no por internet.

1. Copia el **dirección URL del Proxy** desde el nuevo proxy de valor y péguelo en la barra de direcciones del explorador. Es la imagen devuelta desde el sitio de WordPress que se ejecuta dentro de la red virtual.

    ![Archivo de imagen de planta devuelto desde el sitio de WordPress](./media/functions-create-vnet/plant.png)

La aplicación de función está conectada a internet y la red virtual. El proxy es recibir una solicitud a través de internet y, a continuación, que actúa como un proxy HTTP simple para reenviar esa solicitud a la red virtual conectada. El proxy, a continuación, las retransmisiones la respuesta de vuelta al público a través de internet.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, el sitio de WordPress actúa como una API que se llama mediante el uso de un servidor proxy en la aplicación de función. Este escenario hace un buen tutorial porque es fácil de configurar y visualizar. Puede usar cualquier otra API implementado dentro de una red virtual. También podría haber creado una función con el código que llama a las API implementadas dentro de la red virtual. Un escenario más realista es una función que usa las API de cliente de datos para llamar a una instancia de SQL Server implementada en la red virtual.

Las funciones que se ejecutan en un plan Premium comparten la misma infraestructura subyacente de App Service como aplicaciones web en los planes de PremiumV2. Toda la documentación de [aplicaciones en Azure App Service web](../app-service/overview.md) se aplica a las funciones del plan Premium.

> [!div class="nextstepaction"]
> [Más información sobre las opciones de red de funciones](./functions-networking-options.md)

[Plan Premium]: functions-scale.md#premium-plan-public-preview
