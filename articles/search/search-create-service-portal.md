---
title: 'Creación de un servicio Azure Search en Azure Portal: Azure Search'
description: Aprovisione un recurso de Azure Search en Azure Portal. Elija grupos de recursos, regiones, SKU o planes de tarifa.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: fd81115f4e811366b6b115d1c73a9be7ca26698b
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485617"
---
# <a name="create-an-azure-search-service-in-the-portal"></a>Creación de un servicio Azure Search en el portal

Azure Search es un recurso independiente que se usa para agregar una experiencia de búsqueda a las aplicaciones personalizadas. Aunque Azure Search se integra fácilmente con otros servicios de Azure, también puede usarlo solo, con aplicaciones de servidores de red o con software que se ejecuta en otras plataformas de nube.

En este artículo, aprenderá a crear un recurso de Azure Search en [Azure Portal](https://portal.azure.com/).

[![GIF animado](./media/search-create-service-portal/AnimatedGif-AzureSearch-small.gif)](./media/search-create-service-portal/AnimatedGif-AzureSearch.gif#lightbox)

¿Prefiere PowerShell? Use la [plantilla de servicio](https://azure.microsoft.com/resources/templates/101-azure-search-create/) de Azure Resource Manager. Para obtener ayuda para empezar, consulte [Administración de Azure Search con PowerShell](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Suscripción (gratuita o de pago)

[Abra una cuenta gratuita de Azure ](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) y use créditos gratuitos para probar servicios de pago de Azure. Cuando se consuman los créditos, mantenga la cuenta y siga usando servicios de Azure gratuitos, como Websites. No se le realizará ningún cargo en su tarjeta de crédito a menos que cambie explícitamente la configuración y lo solicite.

Como alternativa, [active las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Una suscripción a MSDN le proporciona créditos todos los meses que puede usar para servicios de Azure de pago. 

## <a name="find-azure-search"></a>Búsqueda de Azure Search

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Haga clic en el signo más ("+ Crear recurso") en la esquina superior izquierda.
3. Use la barra de búsqueda para buscar "Azure Search" o vaya al recurso a través de **Web** > **Azure Search**.

![Desplazamiento a un recurso de Azure Search](./media/search-create-service-portal/find-search3.png "Navigation path to Azure Search")

## <a name="name-the-service-and-url-endpoint"></a>Asignación de un nombre al servicio y al punto de conexión de dirección URL

Un nombre de servicio es parte del punto de conexión de dirección URL con que se emiten llamadas a la API: `https://your-service-name.search.windows.net`. Escriba el nombre del servicio en el campo **Dirección URL**.

Por ejemplo, si quiere que el punto de conexión sea `https://my-app-name-01.search.windows.net`, debe escribir `my-app-name-01`.

Requisitos de nombre de servicio:

* Debe ser único dentro del espacio de nombres search.windows.net.
* Entre 2 y 60 caracteres de longitud.
* Deben usarse letras minúsculas, números o guiones ("-").
* No deben usarse guiones ("-") en los dos primeros caracteres ni en el último carácter.
* No deben usarse guiones consecutivos ("--").

## <a name="select-a-subscription"></a>Selección de una suscripción

Si tiene más de una suscripción, elija una que también tenga servicios de almacenamiento de datos o archivos. Azure Search puede detectar automáticamente Azure Table y Blob Storage, SQL Database y Azure Cosmos DB para indexarlos mediante [*indexadores*](search-indexer-overview.md), pero solo para servicios de la misma suscripción.

## <a name="select-a-resource-group"></a>Selección de un grupo de recursos

Un grupo de recursos es una colección de servicios y recursos de Azure que se usan juntos. Por ejemplo, si usa Azure Search para indexar una base de datos SQL, ambos servicios deben formar parte del mismo grupo de recursos.

Si no combina recursos en un solo grupo o si los grupos de recursos existentes se rellenan con los recursos usados en soluciones no relacionadas, cree un grupo de recursos solo para su recurso de Azure Search.

> [!TIP]
> Al eliminar un grupo de recursos también se eliminan los servicios que contiene. En el caso de proyectos de prototipo que usan muchos servicios, si se ponen todos ellos en el mismo grupo de recursos, la limpieza resulta más fácil después de que el proyecto ha finalizado.

## <a name="select-a-location"></a>Seleccionar una ubicación

En cuanto servicio de Azure, Azure Search se puede hospedar en centros de datos de todo el mundo. Se puede encontrar la lista de regiones admitidas en la [página de precios](https://azure.microsoft.com/pricing/details/search/). 

Si va a indexar los datos proporcionados por otro servicio de Azure (Azure Storage, Azure Cosmos DB, Azure SQL Database), se recomienda crear un servicio Azure Search en la misma región para evitar cargos de ancho de banda. No hay ningún cargo por datos salientes si los servicios están en la misma región.

Si usa los enriquecimientos de la inteligencia artificial de búsqueda cognitiva, cree su servicio en la misma región que el recurso de Cognitive Services. La coubicación de servicios es un requisito para el enriquecimiento de la inteligencia artificial.

> [!Note]
> Los nuevos servicios no están disponibles actualmente en India central. En el caso de los servicios que ya están en India central, puede escalar verticalmente sin restricciones, y el servicio se admite completamente en dicha región. La restricción en esta región es temporal y se quitará esta nota cuando deje de aplicarse.

## <a name="select-a-pricing-tier-sku"></a>Selección de un plan de tarifa (SKU)

[Azure Search se ofrece actualmente en varios planes de tarifa](https://azure.microsoft.com/pricing/details/search/): Gratis, Básico o Estándar. Cada plan tiene su propia [capacidad y sus propios límites](search-limits-quotas-capacity.md). Consulte [Selección SKU o plan de tarifa](search-sku-tier.md) para obtener instrucciones.

Normalmente se elige el tamaño estándar para cargas de trabajo de producción, pero la mayoría de los clientes empieza por el servicio gratis.

Un plan de tarifa no se puede cambiar una vez creado el servicio. Si necesita un plan superior o inferior más adelante, deberá volver a crear el servicio.

## <a name="create-your-service"></a>Creación del servicio

No olvide anclar su servicio en el panel para facilitar el acceso siempre que inicie sesión.

![Anclar al panel](./media/search-create-service-portal/new-service3.png "Anclar el recurso al panel para facilitar el acceso")

## <a name="get-a-key-and-url-endpoint"></a>Obtención de una clave y un punto de conexión de dirección URL

Con algunas excepciones, para usar el nuevo servicio hace falta proporcionar el punto de conexión de dirección URL y una clave de API de autorización. Los inicios rápidos, los tutoriales, como [Exploración de las API REST de Azure Search (Postman)](search-get-started-postman.md) y [Cómo usar Azure Search desde .NET](search-howto-dotnet-sdk.md), los ejemplos y el código personalizado necesitan todos ellos un punto de conexión y una clave para ejecutarse en su recurso en particular.

1. En la página de información general del servicio, busque y copie el punto de conexión de dirección URL en el lado derecho de la página.

   ![Página de información general del servicio con el punto de conexión de dirección URL](./media/search-create-service-portal/url-endpoint.png "Punto de conexión de dirección URL y otros detalles del servicio")

2. En el panel de navegación izquierdo, seleccione **Claves** y, luego, copie una de las claves de administrador (son equivalentes). Las claves de API de administrador son necesarias para crear, actualizar y eliminar objetos en el servicio.

   ![Página de claves que muestra las claves principal y secundaria](./media/search-create-service-portal/admin-api-keys.png "Claves de API de administrador para autorización")

Para las tareas basadas en el portal, no se necesita un punto de conexión y una clave. El portal ya está vinculado a un recurso de Azure Search con derechos de administrador. Para ver un tutorial del portal, comience con [Tutorial: Importación, indexación y consulta en Azure Search](search-get-started-portal.md).

## <a name="scale-your-service"></a>Escalar el servicio

La creación de un servicio puede llevarle unos minutos (15 o más, en función del nivel). Después de aprovisionado el servicio, se puede escalar para satisfacer sus necesidades. Dado que ha elegido el nivel Estándar para el servicio Azure Search, puede escalar el servicio en dos dimensiones: réplicas y particiones. Si ha elegido el nivel Básico, solo puede agregar réplicas. Si ha aprovisionado el servicio gratuito, el escalado no está disponible.

Las ***particiones*** permiten que el servicio almacene y busque en más documentos.

Las ***réplicas*** permiten al servicio administrar una carga más elevada de consultas de búsqueda.

La incorporación de recursos aumenta la factura mensual. La [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) puede ayudarle a entender cómo repercute la incorporación de recursos en la facturación. Recuerde que puede ajustar los recursos en base a la carga. Por ejemplo, puede aumentar los recursos para crear un índice inicial completo y luego reducir los recursos más adelante a un nivel más adecuado para la indexación incremental.

> [!Important]
> Un servicio debe tener [2 réplicas para SLA de solo lectura y 3 réplicas para SLA de lectura y escritura](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Vaya a la página del servicio de búsqueda de Azure Portal.
2. En el panel de navegación de la izquierda, seleccione **Configuración** > **Escala**.
3. Use la barra deslizante para agregar recursos de cualquier tipo.

![Agregar capacidad](./media/search-create-service-portal/settings-scale.png "Agregar capacidad mediante réplicas y particiones")

> [!Note]
> Cada nivel tiene distintos [límites](search-limits-quotas-capacity.md) en el número total de unidades de búsqueda que se permiten en un único servicio (réplicas * particiones = total de unidades de búsqueda).

## <a name="when-to-add-a-second-service"></a>Cuándo se debe agregar un segundo servicio

La mayoría de los clientes usan un solo servicio aprovisionado en un nivel que proporciona el [equilibrio de recursos adecuado](search-sku-tier.md). Un servicio puede hospedar varios índices en función de los [límites máximos de la capa que seleccione](search-capacity-planning.md), con cada índice aislado del otro. En Azure Search, solo se podrán dirigir las solicitudes a un índice, lo que reduce al máximo la posibilidad de que se recuperen datos accidentales o intencionados desde otros índices en el mismo servicio.

Aunque la mayoría de los clientes usan un solo servicio, la redundancia de servicios puede ser necesaria si los requisitos operativos son los siguientes:

* Recuperación ante desastres (interrupción del centro de datos). Azure Search no proporciona conmutación por error instantánea si se produce una interrupción del servicio. Para obtener recomendaciones e instrucciones, consulte el artículo sobre [administración de servicios](search-manage.md).
* La investigación del modelado de la arquitectura multiempresa ha determinado que los servicios adicionales es el diseño óptimo. Para obtener más información, consulte el artículo sobre [diseño para multiempresa](search-modeling-multitenant-saas-applications.md).
* Para las aplicaciones implementadas globalmente, es posible que necesite una instancia de Azure Search en varias regiones para minimizar la latencia de tráfico internacional de la aplicación.

> [!NOTE]
> En Azure Search, no puede separar las cargas de trabajo de indización y consultas; por lo tanto, nunca podría crear varios servicios para las cargas de trabajo segregadas. Un índice siempre se consulta en el servicio donde se creó (no se puede crear un índice en un servicio y copiar en otro).

No se requiere un segundo servicio para lograr alta disponibilidad. La alta disponibilidad en las consultas se logra al usar 2 o más réplicas en el mismo servicio. Las actualizaciones de réplicas son secuenciales, lo que significa que, al menos, una es operativa cuando se implementa una actualización de servicio. Para obtener más información, consulte [Acuerdos de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Pasos siguientes

Después de aprovisionar un servicio de Azure Search, puede continuar en el portal para crear el primer índice.

> [!div class="nextstepaction"]
> [Tutorial: Importación de datos, indexado y ejecución de consultas en el portal](search-get-started-portal.md)
