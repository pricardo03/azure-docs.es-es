---
title: Instalación de Micro Focus Enterprise Server 4.0 y desarrollador empresarial 4.0 en Azure | Microsoft Docs
description: Rehospedar los IBM z/OS mainframe las cargas de trabajo mediante el desarrollo de Micro Focus y probar el entorno en Azure virtual machines (VM).
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 45d6f8606c665d78783f987c2f2b49a77801639c
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304605"
---
# <a name="install-micro-focus-enterprise-server-40-and-enterprise-developer-40-on-azure"></a>Instalación de Micro Focus Enterprise Server 4.0 y desarrollador empresarial 4.0 en Azure

Este artículo muestra cómo configurar [Micro foco Enterprise Server 4.0](https://www.microfocus.com/documentation/enterprise-developer/es30/) y [Micro foco Enterprise Developer 4.0](https://www.microfocus.com/documentation/enterprise-developer/ed_30/) en Azure.

Carga de trabajo comunes en Azure es un entorno de desarrollo y pruebas. Este escenario es común porque es tan rentable y fácil de implementar y anular. Con Enterprise Server, Micro Focus ha creado una de las mayores mainframe rehospedaje plataformas disponibles. Puede ejecutar las cargas de trabajo de z/OS en un económico x86 platform en Azure con máquinas virtuales de Windows o Linux (VM).

Esta configuración usa máquinas virtuales de Azure que ejecuta la imagen de Windows Server 2016 desde Azure Marketplace con Microsoft SQL Server 2017 ya instalado. Esta configuración también se aplica a Azure Stack.

El entorno de desarrollo correspondientes para Enterprise Server es desarrollador empresarial, que se ejecuta en cualquier equipo con Microsoft Visual Studio 2017 o versiones posteriores, Visual Studio Community (gratis descargar), o Eclipse. En este artículo muestra cómo se implementa mediante una máquina virtual de Windows Server 2016 que viene con Visual Studio 2017 o posterior instalado.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, revise estos requisitos previos:

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- El software de Micro Focus y una licencia válida (o licencia de prueba). Si es un cliente existente de Micro Focus, póngase en contacto con su representante de Micro Focus. En caso contrario, [solicite una evaluación](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

- Obtenga la documentación para [Enterprise Server y Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#").

> [!NOTE]
> Una práctica recomendada es configurar un túnel de red privada virtual (VPN) de sitio a sitio o un jumpbox para que pueda controlar el acceso a las máquinas virtuales de Azure.

## <a name="install-enterprise-server"></a>Instalación de Enterprise Server

1. Para obtener una mejor seguridad y facilidad de uso, considere la posibilidad de crear un nuevo grupo de recursos solo para este proyecto, por ejemplo, **RGMicroFocusEntServer**. Use la primera parte del nombre en Azure para elegir el tipo de recurso que resulte más fácil de detectar en una lista.

2. Cree una máquina virtual. En Azure Marketplace, seleccione la máquina virtual y el sistema operativo que desee. Este es un programa de instalación recomendado:

    - **Enterprise Server**: Seleccione la máquina virtual de v3 de ES2 (con 2 vCPU y 16 GB de memoria) con Windows Server 2016 y SQL Server 2017 instalado. Esta imagen está disponible en Azure Marketplace. Enterprise Server puede usar también Azure SQL Database.

    - **Desarrollador de la empresa**: Seleccionar máquina virtual B2ms (con 2 vCPU y 8 GB de memoria) con Windows 10 y Visual Studio instalado. Esta imagen está disponible en Azure Marketplace.

3. En el **Fundamentos** sección, escriba el nombre de usuario y la contraseña. Seleccione el **suscripción** y **ubicación o región** que le gustaría usar para las máquinas virtuales. Seleccione **RGMicroFocusEntServer** para el grupo de recursos.

4. Coloque ambas máquinas virtuales en la misma red virtual para que pueden comunicarse entre sí.

5. Acepte los valores predeterminados para el resto de la configuración. Recuerde el nombre de usuario y contraseña que ha creado para el Administrador de estas máquinas virtuales.

6. Cuando se han creado las máquinas virtuales, abra los puertos de entrada 9003, 86 y 80 para HTTP y 3389 para RDP en la máquina del servidor de empresa y 3389 en el equipo del desarrollador.

7. Para iniciar sesión en la máquina virtual del servidor de empresa, en Azure portal, seleccione la máquina virtual v3 de ES2. Vaya a la **Introducción** sección y seleccione **Connect** para iniciar una sesión RDP. Inicie sesión con las credenciales que creó para la máquina virtual.

8. Desde la sesión RDP, cargue los dos archivos siguientes. Dado que usa Windows, puede arrastrar y colocar los archivos en la sesión RDP:

    - **es\_40.anexo exe**, el archivo de instalación del servidor de empresa.

    - **mflic**, el archivo de licencia correspondiente, no se pudo cargar el servidor de empresa sin él.

9. Haga doble clic en el archivo para iniciar la instalación. En la primera ventana, seleccione la ubicación de instalación y acepte el contrato de licencia del usuario final.

     ![Pantalla de instalación de Server Enterprise foco Micro](media/01-enterprise-server.png)

     Cuando se completa la instalación, aparece el mensaje siguiente:

     ![Pantalla de instalación de Server Enterprise foco Micro](media/02-enterprise-server.png)

### <a name="check-for-updates"></a>Compruebe si hay actualizaciones

Después de la instalación, asegúrese de comprobar si hay actualizaciones adicionales desde una serie de requisitos previos, como el paquete redistribuible de Microsoft C++ y .NET Framework se instalan junto con el servidor de empresa.

### <a name="upload-the-license"></a>Cargar la licencia

1. Inicie la administración de licencias de Micro Focus.

2. Haga clic en **iniciar** \> **Micro foco License Manager** \> **administración de licencias**y, a continuación, haga clic en el **instalar** ficha. Elija el tipo de formato de la licencia para cargar: un archivo de licencia o un código de licencia de 16 caracteres. Por ejemplo, para un archivo, en **archivo de licencia**, vaya a la **mflic** archivo cargado previamente en la máquina virtual y seleccione **instalar licencias**.

     ![Cuadro de diálogo de administración de licencias de foco Micro](media/03-enterprise-server.png)

3. Compruebe que se cargan Enterprise Server. Intente iniciar el sitio de administración de servidor empresarial desde un explorador utilizando esta dirección URL <http://localhost:86/> . Se muestra la página de administración de servidor empresarial como se muestra.

     ![Página de administración de servidor de empresa](media/04-enterprise-admin.png)

## <a name="install-enterprise-developer-on-the-developer-machine"></a>Instalar Enterprise Developer en el equipo del desarrollador

1. Seleccione el grupo de recursos que creó anteriormente (por ejemplo, **RGMicroFocusEntServer**), a continuación, seleccione la imagen de desarrollador.

2. Para iniciar sesión la máquina virtual, vaya a la **Introducción** sección y seleccione **Connect**. Este inicio de sesión inicia una sesión RDP. Inicie sesión con las credenciales que creó para la máquina virtual.

3. En la sesión RDP, cargar los dos archivos siguientes (arrastrar y colocar si le gusta):

    - **edvs2017.exe**, el archivo de instalación del servidor de empresa.

    - **mflic**, el archivo de licencia correspondiente (no se cargará el desarrollador de la empresa sin él).

4. Haga doble clic en el **edvs2017.exe** archivo para iniciar la instalación. En la primera ventana, seleccione la ubicación de instalación y acepte el contrato de licencia del usuario final. Si lo desea, elija **instalar Rumba 9.5** para instalar este emulador de terminal, probablemente necesitará.

     ![Micro desarrollador empresarial de foco para el cuadro de diálogo de instalación de Visual Studio 2017](media/04-enterprise-server.png)

5. Una vez completada la instalación, aparece el mensaje siguiente:

     ![Mensaje de instalación correcta](media/05-enterprise-server.png)

6. Inicie el Administrador de licencias de foco Micro tal como hizo para Enterprise Server. Elija **iniciar** \> **Micro foco License Manager** \> **administración de licencias**y haga clic en el **instalar**ficha.

7. Elija el tipo de formato de la licencia para cargar: un archivo de licencia o un código de licencia de 16 caracteres. Por ejemplo, para un archivo, en **archivo de licencia**, vaya a la **mflic** archivo cargado previamente en la máquina virtual y seleccione **instalar licencias**.

     ![Cuadro de diálogo de administración de licencias de foco Micro](media/07-enterprise-server.png)

Cuando se carga el desarrollador de la empresa, la implementación de un entorno de desarrollo y pruebas de Micro Focus en Azure es completa.

## <a name="next-steps"></a>Pasos siguientes

- [Configurar la aplicación de demostración de banco](./demo.md)
- [Enterprise Server se ejecutan en contenedores de Docker](./run-enterprise-server-container.md)
- [Migrar la aplicación del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
