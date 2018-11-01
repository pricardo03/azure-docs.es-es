---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: c9df9e0812d5c4b5a581d4b7da677623f45f8053
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227095"
---
1. Mientras está conectado a la máquina virtual con Escritorio remoto, busque **Administrador de configuración**:

    ![Abrir SSCM](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. En el panel de la consola del Administrador de configuración de SQL Server, expanda **Configuración de red de SQL Server**.

1. En el panel de la consola, haga clic en **Protocolos para MSSQLSERVER** (el nombre de instancia predeterminado). En el panel de detalles, haga clic con el botón derecho en **TCP** y, después, haga clic en **Habilitar** si no está habilitado aún.

    ![Habilitar TCP](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. En el panel de la consola, haga clic en **Servicios de SQL Server**. En el panel de detalles, haga clic con el botón derecho en **SQL Server (*nombre de la instancia*)** (la instancia predeterminada es **SQL Server (MSSQLSERVER)**) y, después, haga clic en **Reiniciar** para detener y reiniciar la instancia de SQL Server.

    ![Reiniciar el motor de base de datos](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Cierre el Administrador de configuración de SQL Server.

Para obtener más información acerca de la habilitación de protocolos para el motor de base de datos de SQL Server, consulte [Habilitar o deshabilitar un protocolo de red de servidor](http://msdn.microsoft.com/library/ms191294.aspx).
