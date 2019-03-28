---
title: Inicio rápido de Striim con Azure SQL Data Warehouse | Microsoft Docs
description: Empiece a trabajar rápidamente con Striim y Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: a835fbd427408afae9ea6ae1b1bc2a84c18b0c34
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540464"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Guía de instalación de la oferta de Marketplace Azure SQL DW para Striim

En este inicio rápido se da por supuesto que ya tiene una instancia preexistente de SQL Data Warehouse.

Busque Striim en Azure Marketplace y seleccione la opción Striim for Data Integration to SQL Data Warehouse (Staged). 

![Instalación de Striim][install]

Configure la máquina virtual de Striim con las propiedades especificadas y anote el nombre del clúster, la contraseña y la contraseña de administrador de Striim.

![Configuración de Striim][configure]

Una vez implementado, haga clic en \<nombre de máquina virtual >-masternode en Azure portal, haga clic en conectar y copiar el inicio de sesión con cuenta de máquina virtual local 

![Conexión de Striim a SQL Data Warehouse][connect]

Descargue el archivo sqljdbc42.jar desde <https://www.microsoft.com/en-us/download/details.aspx?id=54671> en la máquina local. 

Abra una ventana de línea de comandos y cambie los directorios a donde descargó el archivo .jar de JDBC. Transfiera mediante SCP el archivo .jar a su máquina virtual de Striim y obtenga la dirección y la contraseña de Azure Portal.

![Copie el archivo .jar en la máquina virtual.][copy-jar]

Abra otra ventana de línea de comandos o use una utilidad de SSH para conectarse mediante SSH al clúster de Striim.

![Conexión al clúster mediante SSH][ssh]

Ejecute los siguientes comandos para mover el archivo .jar de JDBC al directorio lib de Striim e iniciar y detener el servidor.

   1. unidad de búsqueda de sudo
   2. cd /tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl stop striim-node
   5. systemctl stop striim-dbms
   6. systemctl start striim-dbms
   7. systemctl start striim-node

![Inicio del clúster de Striim][start-striim]

Ahora, abra su explorador favorito y vaya a \<nombre DNS >: 9080

![Vaya a la pantalla de inicio de sesión.][navigate]

Inicie sesión con el nombre de usuario y la contraseña que configuró en Azure Portal y seleccione su asistente preferido para comenzar; o vaya a la página de aplicaciones para comenzar a usar la interfaz de usuario de arrastrar y colocar.

![Inicio de sesión con credenciales de servidor][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
