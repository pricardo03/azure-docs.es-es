---
title: Configurar Micro foco CICS BankDemo Micro foco Enterprise Developer 4.0 en Azure Virtual Machines
description: Ejecute la aplicación de Micro foco BankDemo en Azure Virtual Machines (VM) para aprender a utilizar Micro Focus Enterprise Server y Enterprise Developer.
author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 7afe29cb98a294b2a30020ad48f8b27264386746
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304698"
---
# <a name="set-up-micro-focus-cics-bankdemo-for-micro-focus-enterprise-developer-40-on-azure"></a>Configurar Micro foco CICS BankDemo Micro foco Enterprise Developer 4.0 en Azure

Al configurar Micro foco Enterprise Server 4.0 y 4.0 programador de Enterprise en Azure, puede probar las implementaciones de cargas de trabajo de IBM z/OS. En este artículo se muestra cómo configurar BankDemo CICS, una aplicación de ejemplo que se incluye con Enterprise Developer.

Significa Customer Information Control System, la plataforma de transacción utilizada por muchas de las aplicaciones en línea mainframe CICs. La aplicación BankDemo es ideal para aprender cómo funcionan los Enterprise Server y Enterprise Developer y cómo administrar e implementar una aplicación real que se completa con terminales de pantalla verde.

## <a name="prerequisites"></a>Requisitos previos

- Una máquina virtual con [Enterprise Developer](set-up-micro-focus-azure.md). Tenga en cuenta que el desarrollador empresarial tiene una instancia completa de Enterprise Server en ella para fines de desarrollo y pruebas. Esta instancia es la instancia del servidor de empresa utilizado para la demostración.

- [Edición de SQL Server 2017 Express](https://www.microsoft.com/sql-server/sql-server-editions-express). Descargar e instalarlo en la máquina virtual para desarrolladores de empresa. Enterprise Server requiere una base de datos para la administración de regiones CICS y la aplicación BankDemo también usa una base de datos de SQL Server denominada BANKDEMO. Esta demostración se asume que usa SQL Server Express para ambas bases de datos. Al instalar, seleccione la instalación básica.

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) (SSMS). SSMS se usa para administrar las bases de datos y ejecutar un script de Transact-SQL. Descargar e instalarlo en la máquina virtual para desarrolladores de empresa.

- [Visual Studio de 2019](https://azure.microsoft.com/downloads/) con el service pack más reciente o [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/), que puede descargar de forma gratuita.

- Rumba escritorio o en otro emulador 3270.

## <a name="configure-the-windows-environment"></a>Configurar el entorno de Windows

Después de instalar Enterprise Developer 4.0 en la máquina virtual, debe configurar la instancia del servidor de empresa que se incluye con él. Para ello, deberá instalar algunas características adicionales de Windows como se indica a continuación.

1. Use RDP para iniciar sesión en la VM de Enterprise Server 4.0 que creó.

2. Haga clic en el **búsqueda** situado junto a la **iniciar** botón y escriba **características de Windows**. Abre el Administrador de servidores agregar Roles y características Asistente.

3. Seleccione **rol servidor Web (IIS)** y, a continuación, compruebe las opciones siguientes:

    - Herramientas de administración Web
    - Compatibilidad con la administración de IIS 6 (seleccionar todas las características disponibles)
    - Consola de administración de IIS
    - Herramientas y Scripts de administración de IIS
    - Servicio de administración de IIS

4. Seleccione **servicios World Wide Web**y compruebe las opciones siguientes:

     Características de desarrollo de aplicaciones:
    - Extensibilidad de .NET
    - ASP.NET
    - Características HTTP comunes: Agregar todas las características disponibles
    - Estado y diagnóstico: Agregar todas las características disponibles
    - Seguridad:
        - Autenticación básica
        - Autenticación de Windows

5. Seleccione **Windows Process Activation Service** y todos sus elementos secundarios.

6. Para **características**, comprobar **Microsoft .NET framework 3.5.1**y compruebe las opciones siguientes:

    - Activación de Windows Communication Foundation HTTP
    - Activación no HTTP de Windows Communication Foundation

7. Para **características**, comprobar **Microsoft .NET framework 4.6**y compruebe las opciones siguientes:

   - Activación de canalización con nombre
   - Activación de TCP
   - Uso compartido de puertos TCP

     ![Agregar Roles y características Asistente: Servicios de rol](media/01-demo-roles.png)

8. Cuando haya seleccionado todas las opciones, haga clic en **siguiente** para instalar.

9. Después de las características de Windows, vaya a **Panel de Control \> sistema y seguridad \> herramientas administrativas**y seleccione **servicios**. Desplácese hacia abajo y asegúrese de que los siguientes servicios se ejecutan y establecido en **automática**:

    - **NetTcpPortSharing**
    - **Adaptador de escucha Net.Pipe**
    - **Adaptador de escucha Net.TCP**

10. Para configurar la compatibilidad IIS y WAS, en el menú Buscar **Micro foco Enterprise Developer símbolo del sistema (64 bits)** y se ejecutan como **administrador**.

11. Tipo **wassetup – i** y presione **ENTRAR**.

12. Después de ejecutar el script, puede cerrar la ventana.

## <a name="configure-the-local-system-account-for-sql-server"></a>Configurar la cuenta sistema local de SQL Server

Algunos procesos de servidor empresarial deben ser capaz de iniciar sesión en SQL Server y crear bases de datos y otros objetos. Estos procesos usan la cuenta sistema local, por lo que debe dar autoridad sysadmin a esa cuenta.

1. Iniciar el **SSMS** y haga clic en **Connect** para conectarse al servidor local SQLEXPRESS mediante la autenticación de Windows. Debe estar disponible en el **nombre del servidor** lista.

2. En el lado izquierdo, expanda el **seguridad** carpeta y seleccione **inicios de sesión**.

3. Seleccione **NT AUTHORITY\\sistema** y seleccione **propiedades**.

4. Seleccione **Roles de servidor** y comprobar **sysadmin**.

     ![Ventana del explorador de objetos de SSMS: Propiedades de inicio de sesión](media/02-demo-explorer.png)

## <a name="create-the-bankdemo-database-and-all-its-objects"></a>Crear la base de datos BankDemo y todos sus objetos

1. Abra **Windows Explorer** y vaya a **C:\\usuarios\\pública\\documentos\\Micro Focus\\desarrollador empresarial\\ Ejemplos de\\Mainframe\\CICS\\DotNet\\BankDemo\\SQL**.

2. Copie el contenido de **BankDemoCreateAll.SQL** archivo en el Portapapeles.

3. Abra **SSMS**. A la derecha, haga clic en **Server** y seleccione **nueva consulta**.

4. Pegue el contenido del Portapapeles en el **nueva consulta** cuadro.

5. Ejecute la instrucción SQL haciendo **Execute** desde el **comando** ficha encima de la consulta.

Debe ejecutar la consulta sin errores. Cuando se complete, tendrá la base de datos de ejemplo para la aplicación BankDemo.

![Salida SQLQuery1.sql](media/03-demo-query.png)

## <a name="verify-that-the-database-tables-and-objects-have-been-created"></a>Compruebe que se han creado las tablas de base de datos y objetos

1. Haga clic en el **BANKDEMO** de base de datos y seleccione **actualizar**.

2. Expanda el **base de datos** y seleccione **tablas**. Debería ver algo parecido a lo siguiente.

     ![Tabla BANKDEMO expandido en el Explorador de objetos](media/04-demo-explorer.png)

## <a name="build-the-application-in-enterprise-developer"></a>Compile la aplicación de Enterprise Developer

1. Abra Visual Studio e inicie sesión.

2. En el **archivo** opción de menú, seleccione **Abrir proyecto o solución**, vaya a **C:\\usuarios\\pública\\documentos\\Micro Enfoque\\Enterprise Developer\\ejemplos\\Mainframe\\CICS\\DotNet\\BankDemo**y seleccione el **sln**archivo.

3. Tardar algún tiempo para examinar los objetos. Programas COBOL que se muestran en el Explorador de soluciones con la extensión CBL junto con CopyBooks (CPY) y JCL.

4. Haga clic en el **BankDemo2** proyecto y seleccione **establecer como proyecto de inicio**.

    > [!NOTE]
    > El proyecto BankDemo hace uso de HCOSS (opción de compatibilidad de Host para SQL Server,) que no se utiliza para esta demostración.

5. En **el Explorador de soluciones**, haga clic en el **BankDemo2** proyecto y seleccione **compilar**.

    > [!NOTE]
    > Creación en el nivel de solución produce errores, como no se ha configurado HCOSS.

6. Cuando se compila el proyecto, examine el **salida** ventana. Debería parecerse a la imagen siguiente.

     ![Ventana de salida muestra las compilación correcta](media/05-demo-output.png)

## <a name="deploy-the-bankdemo-application-into-the-region-database"></a>Implementar la aplicación BankDemo en la base de datos de la región

1. Abra un símbolo del sistema de Enterprise Developer (64 bits) como administrador.

2. Navegue hasta la **pública %\\documentos\\Micro Focus\\Enterprise Developer\\ejemplos\\Mainframe\\CICS\\DotNet\\ BankDemo**.

3. En el símbolo del sistema, ejecute **bankdemodbdeploy** e incluya el parámetro para la base de datos implementar, por ejemplo:

    ```
    bankdemodbdeploy (local)/sqlexpress
    ```

> [!NOTE]
> Asegúrese de usar una barra diagonal (/) no es una barra diagonal inversa (\\). Este script se ejecuta durante un tiempo.

![Administración: Ventana de símbolo del sistema para desarrolladores empresariales](media/06-demo-cmd.png)

## <a name="create-the-bankdemo-region-in-enterprise-administrator-for-net"></a>Crear el área de BankDemo en el Administrador de empresa para .NET

1. Abra el **Enterprise Server para la administración de .NET** la interfaz de usuario.

2. Para iniciar el complemento de MMC, desde el Windows **iniciar** menú, elija **Micro foco Enterprise Developer \> configuración \> Enterprise Server para la administración de .NET**. (Para Windows Server, elija **Micro foco Enterprise Developer \> Enterprise Server para la administración de .NET**).

3. Expanda el **regiones** contenedor en el panel izquierdo y, a continuación, haga **CICS**.

4. Seleccione **definir región** para crear una nueva región de CICS denominada **BANKDEMO**, hospedado en la base de datos (local).

5. Proporcionar la instancia del servidor de base de datos, haga clic en **siguiente**y, a continuación, escriba el nombre de la región **BANKDEMO**.

     ![Definir el cuadro de diálogo de región](media/07-demo-cics.png)

6. Para seleccionar el archivo de definición de la región para la base de datos entre regiones, busque **región\_bankdemo\_db.config** en **C:\\usuarios\\pública\\ Documentos\\Micro Focus\\Enterprise Developer\\ejemplos\\Mainframe\\CICS\\DotNet\\BankDemo**.

     ![Definir la región - nombre de la región: BANKDEMO](media/08-demo-cics.png)

7. Haga clic en **Finalizar**

## <a name="create-xa-resource-definitions"></a>Crear definiciones de recursos XA

1. En el panel izquierdo de la **Enterprise Server para la administración de .NET** interfaz de usuario, expanda **sistema**y, a continuación, **definiciones de recursos XA**. Esta configuración define cómo interactúa la región con Enterprise Server y las bases de datos de aplicación.

2. Haga doble clic en **definiciones de recursos XA** y seleccione **Agregar instancia del servidor**.

3. En el cuadro de lista desplegable, seleccione **instancia de servicio de base de datos**. Será el equipo local SQLEXPRESS.

4. Seleccione la instancia en el **definiciones de recursos XA (machinename\\sqlexpress)** contenedor y haga clic en **agregar**.

5. Seleccione **definición de recursos de base de datos XA** y, a continuación, escriba **BANKDEMO** para el **nombre** y **región**.

     ![Nueva pantalla de definición de recursos XA de base de datos](media/09-demo-xa.png)

6. Haga clic en el botón de puntos suspensivos ( **...** ) para que aparezca el Asistente para la cadena de conexión. Para **nombre del servidor**, tipo **(local)\\SQLEXPRESS**. Para **inicio de sesión**, seleccione **Windows autenticación**. Nombre de la base de datos, escriba **BANKDEMO**

     ![Pantalla de la cadena de conexión de edición](media/10-demo-string.png)

7. Probar la conexión.

## <a name="start-the-bankdemo-region"></a>Inicio de la región BANKDEMO

> [!NOTE]
> El primer paso es importante: Debe establecer la región que se usa la definición de recursos XA que acaba de crear.

1. Vaya a la **BANDEMO CICS región** bajo el **regiones contenedor**y, a continuación, seleccione **Editar archivo de inicio de región** desde el **acciones** panel. Desplácese hacia abajo hasta las propiedades de SQL y escriba **bankdemo** para el **nombre del recurso XA**, o use el botón de puntos suspensivos para seleccionarlo.

2. Haga clic en el **guardar** icono para guardar los cambios.

3. Haga clic en **BANKDEMO CICS región** en el **consola** panel y seleccione **inicios y paradas región**.

4. En la parte inferior de la **inicios y paradas región** cuadro que aparece en el panel central, seleccione **iniciar**. Después de unos segundos, se inicia la región.

     ![Cuadro de inicio y detención de SQL](media/11-demo-sql.png)

     ![Región CICS BANKDEMO: pantalla de introducción](media/12-demo-cics.png)

## <a name="create-a-listener"></a>Crear un agente de escucha

Crear un agente de escucha para las sesiones de TN3270 que tienen acceso a la aplicación BankDemo.

1. En el panel izquierdo, expanda **editores de configuración** y seleccione **escucha**.

2. Haga clic en el **abrir archivo** icono y seleccione el **seelistener.exe.config** archivo. Este archivo se va a editar y se carga cada vez que se inicia el servidor de empresa.

3. Tenga en cuenta que las dos regiones definen previamente (ESDEMO y JCLDEMO).

4. Para crear una nueva región para BANKDEMO, haga clic en **regiones**y seleccione **Agregar región**.

5. Seleccione **BANKDEMO región**.

6. Agregar un canal de TN3270 con el botón secundario **BANKDEMO región** y seleccionando **Agregar canal**.

7. Para **nombre**, escriba **TN3270**. Para **puerto**, escriba **9024**. La aplicación ESDEMO utiliza el puerto 9230 por lo que deberá usar un puerto diferente.

8. Para guardar el archivo, haga clic en el **guardar** icono o elija **archivo** \> **guardar**.

9. Para iniciar el agente de escucha, haga clic en el **iniciar el agente de escucha** icono o elija **opciones** \> **iniciar el agente de escucha**.

     ![Ventanas del Editor de configuración de agente de escucha](media/13-demo-listener.png)


## <a name="configure-rumba-to-access-the-bankdemo-application"></a>Configurar Rumba para tener acceso a la aplicación BankDemo

Lo último que necesita hacer es configurar una sesión de 3270 con Rumba, un emulador 3270. Este paso permite tener acceso a la aplicación BankDemo mediante el agente de escucha que creó.

1. Desde el Windows **iniciar** menú, inicie sesión de escritorio Rumba.

2. En el **conexiones** elemento de menú, seleccione **TN3270**.

3. Haga clic en **insertar** y tipo **127.0.0.1** para la dirección IP y **9024** para el puerto definido por el usuario.

4. En la parte inferior del cuadro de diálogo, haga clic en **Connect**. Aparece una pantalla en negro CICS.

5. Tipo **bank** para mostrar la pantalla 3270 inicial para la aplicación BankDemo.

6. Para el Id. de usuario, escriba **B0001** y la contraseña, escriba nada. Se abre la primera pantalla BANK20.

![Pantalla de bienvenida de presentación de mainframe](media/14-demo.png)
![pantalla de presentación Mainframe - Rumba: demostración de subsistema](media/15-demo.png)

Felicidades. Ahora se ejecuta una aplicación CICS en Azure mediante Micro Focus Enterprise Server.

## <a name="next-steps"></a>Pasos siguientes

- [Ejecute Enterprise Server en contenedores de Docker en Azure](run-enterprise-server-container.md)
- [Migración de mainframes: Portal](https://blogs.msdn.microsoft.com/azurecat/2018/11/16/mainframe-migration-to-azure-portal/)
- [Máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/linux/overview)
- [Solución de problemas](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Desmitificación del sistema central para la migración a Azure](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)
