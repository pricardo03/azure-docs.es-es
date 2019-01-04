---
title: Configuración de parámetros de servidor en Azure Database for PostgreSQL en Azure Portal
description: En este artículo se explica cómo configurar los parámetros del servidor en Azure Database for PostgreSQL mediante Azure Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 0d0626c48ecebdead604aab93ab0602c698d0d77
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540544"
---
# <a name="configure-server-parameters-in-azure-portal"></a>Configurar parámetros del servidor en Azure Portal
Puede enumerar, mostrar y actualizar los parámetros de configuración de un servidor de Azure Database for PostgreSQL a través de Azure Portal.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- [Un servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Ver y editar parámetros
1. Abra [Azure Portal](https://portal.azure.com).

2. Seleccione su servidor de Azure Database for PostgreSQL.

3. En la sección **CONFIGURACIÓN**, seleccione **Parámetros del servidor**. La página muestra una lista de parámetros, sus valores y descripciones.
![Página de información general de parámetros](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Seleccione el botón **desplegable** para ver los posibles valores para los parámetros de tipo enumerado, como client_min_messages.
![Menú desplegable de enumeración](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Seleccione o mantenga el puntero sobre el botón **i** (información) para ver el rango de valores posibles para los parámetros numéricos, como cpu_index_tuple_cost.
![botón de información](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Si es necesario, utilice el **cuadro de búsqueda** para reducir a un parámetro específico. Se busca en el nombre y la descripción de los parámetros.
![Resultados de la búsqueda](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Cambie los valores de los parámetros que le gustaría ajustar. Todos los cambios que realiza en una sesión se resaltan en color púrpura. Una vez que haya cambiado los valores, puede seleccionar **Guardar**. O bien puede **descartar** los cambios.
![Guardar o descartar cambios](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Si ha guardado los nuevos valores para los parámetros, siempre puede revertir todos los elementos a los valores predeterminados; para ello, seleccione **Restablecer todos los valores predeterminados**.
![Restablecer todos los valores predeterminados](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Pasos siguientes
Más información sobre lo siguiente:
- [Información general de los parámetros del servidor en Azure Database for PostgreSQL](concepts-servers.md)
- [Configuración de parámetros con la CLI de Azure](howto-configure-server-parameters-using-cli.md)
