---
title: Instalar TmaxSoft OpenFrame en máquinas virtuales de Azure
description: Rehospedar los IBM z/OS mainframe las cargas de trabajo mediante el entorno de TmaxSoft OpenFrame en Azure Virtual Machines (VM).
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: c7e6e7e2023c333207a3a17c3b6711d92de7d044
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65187790"
---
# <a name="install-tmaxsoft-openframe-on-azure"></a>Instalar TmaxSoft OpenFrame en Azure

Obtenga información sobre cómo configurar un entorno de OpenFrame en Azure adecuado para el desarrollo, demostraciones, pruebas o las cargas de trabajo de producción. Este tutorial le guiará a través de cada paso.

OpenFrame incluye varios componentes que crean el entorno de emulación de gran sistema en Azure. Por ejemplo, servicios en línea OpenFrame reemplazan el middleware de mainframe, como IBM Customer Information Control System (CICS) y lote OpenFrame, con su componente TJES, reemplaza el subsistema de entrada de trabajo (JES de mainframes de IBM).

OpenFrame funciona con cualquier base de datos relacional, incluida la base de datos de Oracle, Microsoft SQL Server, IBM Db2 y MySQL. Esta instalación de OpenFrame usa la base de datos relacional TmaxSoft Tibero. OpenFrame y Tibero se ejecutan en un sistema operativo Linux. En este tutorial instala CentOS 7.3, aunque puede usar otras distribuciones de Linux compatibles. El servidor de aplicaciones OpenFrame y la base de datos Tibero están instalados en una máquina virtual (VM).

El tutorial le guiará en la instalación de los componentes de suite OpenFrame. Algunos deben instalarse por separado.

Componentes OpenFrame Main:

- Paquetes de instalación necesarios.
- Tibero base de datos.
- Open Database Connectivity (ODBC) se utiliza por las aplicaciones de OpenFrame para comunicarse con la base de datos Tibero.
- OpenFrame Base, el middleware que administra todo el sistema.
- OpenFrame por lotes, la solución que reemplaza los sistemas de lote del gran sistema.
- TACF, un módulo de servicio que controla el acceso de usuario a los sistemas y los recursos.
- ProSort, una herramienta de ordenación para las transacciones por lotes.
- OFCOBOL, un compilador que interpreta los programas de COBOL del gran sistema.
- OFASM, un compilador que interpreta los programas de ensamblador de mainframe.
- OpenFrame Server tipo C (OSC), la solución que reemplaza el mainframe middleware y IBM CICS.
- Java Enterprise usuario solución (JEUS), un servidor de aplicaciones web que está certificado para Java Enterprise Edition 6.
- OFGW, el componente de puerta de enlace de OpenFrame que proporciona un agente de escucha 3270.
- OFManager, una solución que proporciona funciones de administración y operación de OpenFrame en el entorno web.

Otros componentes de OpenFrame necesarios:

- La solución que reemplaza el middleware de mainframe y DC de IMS de OSI.
- TJES, la solución que proporciona el entorno de JES del gran sistema.
- OFTSAM, la solución que permite que los archivos de SAM (V) que se usará en el sistema abierto.
- OFHiDB, la solución que reemplaza el mainframe's DB IMS.
- OFPLI, un compilador que interpreta el mainframe de PL / programas.
- PROTRIEVE, una solución que se ejecuta el lenguaje de mainframe Easytrieve de CA.
- OFMiner, una solución que analiza los activos de mainframes y luego migrarlos a Azure.

## <a name="architecture"></a>Arquitectura

La ilustración siguiente proporciona información general de los componentes de la arquitectura OpenFrame 7.0 en este tutorial:

![Componentes OpenFrame](media/openframe-02.png)

## <a name="azure-system-requirements"></a>Requisitos del sistema de Azure

En la tabla siguiente se enumera los requisitos para la instalación en Azure.
<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Requisito</th><th>DESCRIPCIÓN</th></tr>
</thead>
<tbody>
<tr><td>Distribuciones de Linux compatibles en Azure
</td>
<td>
Linux x86 2.6 (32 bits, 64 bits)<br/>
Red Hat 7.x<br/>
CentOS 7.x<br/>
</td>
</tr>
<tr><td>Hardware
</td>
<td>Núcleos: 2 (mínimo)<br/>
Memoria: 4 GB (mínimo)<br/>
Espacio de intercambio: 1 GB (mínimo)<br/>
Disco duro: 100 GB (mínimo)<br/>
</td>
</tr>
<tr><td>Software opcional para los usuarios de Windows
</td>
<td>PuTTY: Utilizada en esta guía para configurar las características de la máquina virtual<br/>
WinSCP: Un cliente SFTP popular y puede usar el cliente de FTP<br/>
Eclipse para Windows: Una plataforma de desarrollo compatible con TmaxSoft<br/>
(Microsoft Visual Studio no se admite en este momento)
</td>
</tr>
</tbody>
</table>

<!-- markdownlint-enable MD033 -->

## <a name="prerequisites"></a>Requisitos previos

Debe dedicar unos días para ensamblar todo el software necesario y completar todos los procesos manuales.

Antes de comenzar, realice lo siguiente:

- Obtener los medios de instalación OpenFrame desde TmaxSoft. Si es un cliente existente de TmaxSoft, póngase en contacto con su representante de TmaxSoft para una copia con licencia. En caso contrario, solicitar una versión de prueba de [TmaxSoft](https://www.tmaxsoft.com/contact/).

- Solicitar la documentación de OpenFrame enviando un correo electrónico a <support@tmaxsoft.com>.

- Obtener una suscripción de Azure si aún no tiene uno. También puede crear un [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.

- Opcional. Configurar un túnel VPN de sitio a sitio o un jumpbox que restringe el acceso a la máquina virtual de Azure a los usuarios permitidos en su organización. Este paso no es necesario, pero es una práctica recomendada.

## <a name="set-up-a-vm-on-azure-for-openframe-and-tibero"></a>Configuración de una máquina virtual en Azure para OpenFrame y Tibero

Puede configurar el entorno de OpenFrame con diversos patrones de implementación, pero el procedimiento siguiente muestra cómo implementar el servidor de aplicaciones OpenFrame y la base de datos Tibero en una máquina virtual. En entornos más grandes y para cargas de trabajo considerables, una práctica recomendada es implementar la base de datos por separado en su propia máquina virtual para mejorar el rendimiento.

**Para crear una máquina virtual**

1. Vaya al portal de Azure en <http://portal.azure.com> e inicie sesión en su cuenta.

2. Haga clic en **Máquinas virtuales**.

    ![Lista de recursos en Azure portal](media/vm-01.png)

3. Haga clic en **Agregar**.

    ![Agregue la opción en el portal de Azure](media/vm-02.png)

4. A la derecha del **sistemas operativos**, haga clic en **más**.

     ![Opción más en Azure portal](media/vm-03.png)

5. Haga clic en **basada en CentOS 7.3** para seguir este tutorial exactamente, o puede elegir otra admite la distribución de Linux.

     ![Opciones del sistema operativo en Azure portal](media/vm-04.png)

6. En el **Fundamentos** configuración, escriba **nombre**, **nombre de usuario**, **tipo de autenticación**, **suscripción** (Pago por uso es el estilo AWS de pago), y **grupo de recursos** (use uno existente o cree un grupo TmaxSoft).

7. Cuando haya terminado (incluido el par de claves pública y privada para **tipo de autenticación**), haga clic en **enviar**.

> [!NOTE]
> Si utiliza una clave pública SSH para **tipo de autenticación**, consulte los pasos descritos en la sección siguiente para generar el par de claves pública y privada y luego reanudar los pasos siguientes.

### <a name="generate-a-publicprivate-key-pair"></a>Generar un par de claves pública y privada

Si utiliza un sistema operativo de Windows, necesita PuTTYgen para generar un par de claves pública y privada.

La clave pública puede compartirse libremente, pero la clave privada debe mantenerse en secreta completamente y nunca debe compartirse con otra entidad. Después de generar las claves, debe pegar el **clave pública SSH** en la configuración, en efecto, cargarlo en la VM de Linux. Se almacena interior autorizado\_teclas dentro de la \~/.ssh directorio del directorio principal de la cuenta de usuario. La VM de Linux, a continuación, es capaz de reconocer y validar la conexión una vez que proporciona el asociado **clave privada SSH** en el cliente SSH (en nuestro caso, PuTTY).

Cuando lo que proporciona a nuevos individuos acceso la máquina virtual: 

- Cada nuevo individuo genera sus propias claves pública y privada con PuTTYgen.
- Los usuarios almacenan sus propias claves privadas de forma independiente y enviar la información de clave pública para el Administrador de la máquina virtual.
- El administrador pega el contenido de la clave pública para el \~/.ssh/authorized\_archivo de claves.
- El nuevo individuo se conecta mediante PuTTY.

**Para generar un par de claves pública y privada**

1.  Descargar PuTTYgen desde <https://www.putty.org/> e instalarlo mediante todos los valores predeterminados.

2.  Para abrir PuTTYgen, busque el directorio de instalación de PuTTY en C:\\archivos de programa\\PuTTY.

    ![Interfaz de puTTY](media/puttygen-01.png)

3.  Haga clic en **Generar**.

    ![Cuadro de diálogo de puTTY Key Generator](media/puttygen-02.png)

4.  Después de la generación, guarde la clave pública y la clave privada. Pegue el contenido de la clave pública en el **clave pública SSH** sección de la **crear máquina virtual \> Fundamentos** panel (se muestra en los pasos 6 y 7 en la sección anterior).

    ![Cuadro de diálogo de puTTY Key Generator](media/puttygen-03.png)

### <a name="configure-vm-features"></a>Configurar las características de la máquina virtual

1. En Azure portal, en la **elegir un tamaño** hoja, elija la configuración de hardware de máquina Linux que desee. El *mínimo* requisitos para instalar Tibero y OpenFrame son 2 CPU y 4 GB de RAM, tal como se muestra en esta instalación de ejemplo:

    ![Crear máquina virtual: conceptos básicos](media/create-vm-01.png)

2. Haga clic en **3 configuración** y use la configuración predeterminada para configurar características opcionales.
3. Revise los detalles de pago.

    ![Crear máquina virtual: compra](media/create-vm-02.png)

4. Envíe sus selecciones. Azure comienza a implementar la máquina virtual. Este proceso suele tarda unos minutos.

5. Cuando se implementa la máquina virtual, se muestra su panel, que muestra todos los valores que se seleccionaron durante la configuración. Tome nota de la **dirección IP pública**.

    ![tMax en panel de Azure](media/create-vm-03.png)

6. Abra PuTTY.

7. Para **nombre de Host**, escriba el nombre de usuario y la dirección IP pública se copian. Por ejemplo, **username\@publicip**.

    ![Cuadro de diálogo Configuración de puTTY](media/putty-01.png)

8. En el **categoría** cuadro, haga clic en **conexión \> SSH \> Auth**. Proporcione la ruta de acceso a su **clave privada** archivo.

    ![Cuadro de diálogo Configuración de puTTY](media/putty-02.png)

9. Haga clic en **abierto** para abrir la ventana de PuTTY. Si se realiza correctamente, se conectan a la nueva máquina virtual de CentOS en Azure.

10. Para iniciar sesión como usuario raíz, escriba **sudo bash**.

    ![Inicio de sesión de usuario de raíz en la ventana de comandos](media/putty-03.png)

## <a name="set-up-the-environment-and-packages"></a>Configurar el entorno y los paquetes

Ahora que se crea la máquina virtual y ha iniciado sesión, debe realizar algunos pasos de instalación e instalar los paquetes necesarios de preinstalación.

1. Asigne el nombre **ofdemo** a la dirección IP local mediante vi para modificar el archivo de hosts (`vi /etc/hosts`). Suponiendo que nuestra IP es 192.168.96.148 ofdemo, esto es antes del cambio:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    <IP Address>    <your hostname>
    ```

     Se trata después del cambio:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    192.168.96.148   ofdemo
    ```

2. Crear usuarios y grupos:

    ```vi
    [root@ofdemo ~]# adduser -d /home/oframe7 oframe7 
    [root@ofdemo ~]# passwd oframe7
    ```

3. Cambiar la contraseña de usuario oframe7:

    ```vi
    New password: 
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    ```

4. Actualice los parámetros de kernel en /etc/sysctl.conf:

    ```vi
    [root@ofdemo ~]# vi /etc/sysctl.conf
    kernel.shmall = 7294967296 
    kernel.sem = 10000 32000 10000 10000
    ```

5. Actualizar los parámetros de kernel dinámicamente sin reinicio:

    ```vi
    [root@ofdemo ~]# /sbin/sysctl -p
    ```

6. Obtenga los paquetes necesarios: Asegúrese de que el servidor está conectado a Internet, descargue los paquetes siguientes y, a continuación, vuelva a instalarlos:

     - dos2unix
     - glibc
     - glibc.i686 glibc.x86\_64
     - libaio
     - ncurses

          > [!NOTE]
          > Después de instalar el paquete ncurses, cree los vínculos simbólicos siguientes:
         ```
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so.2
         ```

     - gcc
     - gcc-c++
     - libaio-devel.x86\_64
     - strace
     - ltrace
     - gdb

7. En el caso de instalación de Java RPM, realice lo siguiente:

```
root@ofdemo ~]# rpm -ivh jdk-7u79-linux-x64.rpm
[root@ofdemo ~]# vi .bash_profile

# JAVA ENV
export JAVA_HOME=/usr/java/jdk1.7.0_79/
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$CLASSPATH:$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

[root@ofdemo ~]# source /etc/profile
[root@ofdemo ~]# java –version

java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

[root@ofdemo ~]# echo $JAVA_HOME /usr/java/jdk1.7.0_79/
```

## <a name="install-the-tibero-database"></a>Instalar la base de datos Tibero

Tibero proporciona la claves varias funciones en el entorno OpenFrame en Azure:

- Tibero sirve como almacén de datos interno OpenFrame para distintas funciones del sistema.
- VSAM, los archivos incluidos KSDS RRDS y ESDS, usan la base de datos Tibero internamente para almacenar los datos.
- El repositorio de datos TACF se almacena en Tibero.
- La información del catálogo OpenFrame se almacena en Tibero.
- La base de datos Tibero puede usarse como sustituto de IBM Db2 para almacenar datos de la aplicación.

**Para instalar Tibero**

1. Compruebe que el archivo de instalador binario Tibero está presente y revise el número de versión.
2. Copie el software Tibero en la cuenta de usuario Tibero (oframe). Por ejemplo: 

    ```
    [oframe7@ofdemo ~]$ tar -xzvf tibero6-bin-6_rel_FS04-linux64-121793-opt-tested.tar.gz 
    [oframe7@ofdemo ~]$ mv license.xml /opt/tmaxdb/tibero6/license/
    ```

3. Abra .bash\_perfil en vi (`vi .bash_profile`) y pegue lo siguiente:

    ```
    # Tibero6 ENV
    export TB_HOME=/opt/tmaxdb/tibero6 
    export TB_SID=TVSAM export TB_PROF_DIR=$TB_HOME/bin/prof 
    export LD_LIBRARY_PATH=$TB_HOME/lib:$TB_HOME/client/lib:$LD_LIBRARY_PATH 
    export PATH=$TB_HOME/bin:$TB_HOME/client/bin:$PATH
    ```

4. Para ejecutar el perfil de bash, en el símbolo del sistema, escriba:

    ```
    source .bash_profile
    ```

5. Generar el archivo de sugerencia (un archivo de configuración para Tibero) y luego ábralo en vi. Por ejemplo: 

    ```
    [oframe7@ofdemo ~]$ sh $TB_HOME/config/gen_tip.sh
    [oframe7@ofdemo ~]$ vi $TB_HOME/config/$TB_SID.tip
    ```

6. Modificar \$TB\_HOME/client/config/tbdsn.tbr y colocar en su lugar de 127.0.0.1 oflocalhost tal como se muestra:

    ```
    TVSAM=( 
    (INSTANCE=(HOST=127.0.0.1)
        (PT=8629)
    (DB_NAME=TVSAM)
          )
     )
    ```

7. Crear la base de datos. Aparece el siguiente resultado:

    ```
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NOMOUNT mode).
     /--------------------- newmount sql ------------------------/
    create database character set MSWIN949 national character set UTF16;
    /-----------------------------------------------------------/
    Database created.
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    /opt/tmaxdb/tibero6/bin/tbsvr
    ………………………..
    Creating agent table...
    Done.
    For details, check /opt/tmaxdb/tibero6/instance/TVSAM/log/system_init.log.
    ************************************************** 
    * Tibero Database TVSAM is created successfully on Fri Aug 12 19:10:43 UTC 2016.
    *     Tibero home directory ($TB_HOME) =
    *         /opt/tmaxdb/tibero6
    *     Tibero service ID ($TB_SID) = TVSAM
    *     Tibero binary path =
    *         /opt/tmaxdb/tibero6/bin:/opt/tmaxdb/tibero6/client/bin
    *     Initialization parameter file =
    *         /opt/tmaxdb/tibero6/config/TVSAM.tip
    * 
    * Make sure that you always set up environment variables $TB_HOME and
    * $TB_SID properly before you run Tibero.
     ******************************************************************************
    ```

8. Para reciclar Tibero, primero apagarla utilizando el `tbdown` comando. Por ejemplo: 

    ```
    [oframe7@ofdemo ~]$$ tbdown 
    Tibero instance terminated (NORMAL mode).
    ```

9. Ahora es arrancar Tibero utilizando `tbboot`. Por ejemplo: 

    ```
    [oframe7@ofdemo ~]$ tbboot
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof. Listener port = 8629

    Tibero 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    ```

10. Para crear un espacio de tabla, tener acceso a la base de datos mediante SYS usuario (sys/tmax), a continuación, cree el espacio de tabla es necesario para el volumen predeterminado y TACF:

    ```
    [oframe7@ofdemo ~]$ tbsql tibero/tmax
    tbSQL 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Connected to Tibero.
    ```

11. Ahora, escriba los siguientes comandos SQL:

    ```
    SQL> create tablespace "DEFVOL" datafile 'DEFVOL.dbf' size 500M autoextend on; create tablespace "TACF00" datafile 'TACF00.dbf' size 500M autoextend on; create tablespace "OFM_REPOSITORY" datafile 'ofm_repository.dbf' size 300M autoextend on;
    SQL> Tablespace 'DEFVOL' created.
    SQL> Tablespace 'TACF00' created.
    SQL> Tablespace ' OFM_REPOSITORY ' created.
    SQL> SQL> Disconnected.
    ```

12. Arranque Tibero y compruebe que se ejecutan los procesos de Tibero:

    ```
    [oframe7@ofdemo ~]$ tbboot 
    ps -ef | egrep tbsvr
    ```

Salida:

![Salida Tibero](media/tibero-01.png)

## <a name="install-odbc"></a>Instalar ODBC

Las aplicaciones en OpenFrame se comunican con la base de datos Tibero con la API de ODBC proporcionados por el proyecto de código abierto unixODBC.

Instalar ODBC:

1. Compruebe que el archivo de instalador de unixODBC 2.3.4.tar.gz está presente, o usar el `wget unixODBC-2.3.4.tar.gz` comando. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ wget ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.4.tar.gz
     ```

2. Descomprima el archivo binario. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ tar -zxvf unixODBC-2.3.4.tar.gz
     ```

3. Desplácese al directorio unixODBC-2.3.4 y generar el archivo MAKE utilizando la información de comprobación. Por ejemplo: 

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ ./configure --prefix=/opt/tmaxapp/unixODBC/ --sysconfdir=/opt/tmaxapp/unixODBC/etc
     ```

     De forma predeterminada, unixODBC se instala en/local/USR, por lo que `--prefix` pasa un valor para cambiar la ubicación. De forma similar, los archivos de configuración se instalan en/etc de forma predeterminada, por lo que `--sysconfdir` pasa el valor de la ubicación deseada.

4. Ejecute el archivo MAKE: `[oframe7@ofdemo unixODBC-2.3.4]$ make`

5. Copie el archivo ejecutable en el directorio del programa después de compilar. Por ejemplo: 

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ make install
     ```

6. Utilice vi para editar el perfil de bash (`vi ~/.bash_profile`) y agregue lo siguiente:

     ```
     # UNIX ODBC ENV 
     export ODBC_HOME=$HOME/unixODBC 
     export PATH=$ODBC_HOME/bin:$PATH 
     export LD_LIBRARY_PATH=$ODBC_HOME/lib:$LD_LIBRARY_PATH 
     export ODBCINI=$HOME/unixODBC/etc/odbc.ini 
     export ODBCSYSINI=$HOME
     ```

7. Se aplican a ODBC. Edite los archivos siguientes según corresponda. Por ejemplo: 

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ source ~/.bash_profile

     [oframe7@ofdemo ~]$ cd

     [oframe7@ofdemo ~]$ odbcinst -j unixODBC 2.3.4
     DRIVERS............: /home/oframe7/odbcinst.ini
     SYSTEM DATA SOURCES: /home/oframe7/odbc.ini
     FILE DATA SOURCES..: /home/oframe7/ODBCDataSources
     USER DATA SOURCES..: /home/oframe7/unixODBC/etc/odbc.ini
     SQLULEN Size.......: 8
     SQLLEN Size........: 8
     SQLSETPOSIROW Size.: 8

     [oframe7@ofdemo ~]$ vi odbcinst.ini

     [Tibero]
     Description = Tibero ODBC driver for Tibero6
     Driver = /opt/tmaxdb/tibero6/client/lib/libtbodbc.so
     Setup = 
     FileUsage = 
     CPTimeout = 
     CPReuse = 
     Driver Logging = 7

     [ODBC]
     Trace = NO 
     TraceFile = /home/oframe7/odbc.log 
     ForceTrace = Yes 
     Pooling = No 
     DEBUG = 1

     [oframe7@ofdemo ~]$ vi odbc.ini

     [TVSAM]
     Description = Tibero ODBC driver for Tibero6 
     Driver = Tibero 
     DSN = TVSAM 
     SID = TVSAM 
     User = tibero 
     password = tmax
     ```

8. Cree un vínculo simbólico y validar la conexión de base de datos Tibero:

     ```
     [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbc.so $ODBC_HOME/lib/libodbc.so.1 [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbcinst.so 
     $ODBC_HOME/lib/libodbcinst.so.1

     [oframe7@ofdemo lib]$ isql TVSAM tibero tmax
     ```

Muestra el siguiente resultado:

![Salida ODBC que muestra conectado a SQL](media/odbc-01.png)

## <a name="install-openframe-base"></a>Instalar Base OpenFrame

El servidor de Base de aplicación se instala antes que los servicios individuales que OpenFrame se usa para administrar el sistema en Azure, incluidos las procesos del servidor de control de transacciones.

**Para instalar la OpenFrame Base**

1. Asegúrese de que la instalación de Tibero se realizó correctamente y, después, compruebe que la siguiente OpenFrame\_Base7\_0\_Linux\_x86\_64. bin instalador junto base.properties archivo de configuración están presentes.

2. Actualizar el perfil de bash con la información específica de Tibero siguiente:

     ```bash
     alias ofhome='cd $OPENFRAME_HOME'
     alias ulog='cd $OPENFRAME_HOME/log/tmax/ulog'
     alias sysjcl='cd $OPENFRAME_HOME/volume_default/SYS1.JCLLIB'
     alias sysload='cd $OPENFRAME_HOME/volume_default/SYS1.LOADLIB'
     alias sysproc='cd $OPENFRAME_HOME/volume_default/SYS1.PROCLIB'
     alias oscsrc='cd $OPENFRAME_HOME/osc/oivp'
     alias osisrc='cd $OPENFRAME_HOME/osi/oivp'
     alias defvol='cd $OPENFRAME_HOME/volume_default'
     ```

3. Ejecute el perfil de bash:`[oframe7@ofdemo ~]$ . .bash_profile`
4. Asegúrese de que se están ejecutando los procesos Tibero. Por ejemplo: 

     ```linux
     [oframe7@ofdemo ~]$ ps -ef|grep tbsvr
     ```

    ![Base](media/base-01.png)

     > [!IMPORTANT]
     > Asegúrese de que iniciar Tibero antes de la instalación.

5. Generar licencias en [technet.tmaxsoft.com](https://technet.tmaxsoft.com/en/front/main/main.do) y poner la OpenFrame Base, lote, TACF, OSC licencias en la carpeta correspondiente:

     ```
     [oframe7@ofdemo ~]$ cp license.dat /opt/tmaxapp/OpenFrame/core/license/
     [oframe7@ofdemo ~]$ cp lictjes.dat lictacf.dat licosc.dat $OPENFRAME_HOME/license/
     ```

6. Descargue los archivos binarios y base.properties de OpenFrame Base:

     ```
     [oframe7@ofdemo ~]$ vi base.properties
     OPENFRAME_HOME= <appropriate location for installation> ex. /opt/tmaxapp/OpenFrame TP_HOST_NAME=<your IP Hostname> ex. ofdemo 
     TP_HOST_IP=<your IP Address> ex. 192.168.96.148 
     TP_SHMKEY=63481 
     TP_TPORTNO=6623 
     TP_UNBLOCK_PORT=6291 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     MASCAT_NAME=SYS1.MASTER.ICFCAT 
     MASCAT_CREATE=YES 
     DEFAULT_VOLSER=DEFVOL 
     VOLADD_DEFINE=YES TSAM_USERNAME=tibero 
     TSAM_PASSWORD=tmax 
     TSAM_DATABASE=oframe 
     DATASET_SHMKEY=63211 
     DSLOCK_DATA=SYS1.DSLOCK.DATA 
     DSLOCK_LOG=SYS1.DSLOCK.LOG 
     DSLOCK_SEQ=dslock_seq.dat 
     DSLOCK_CREATE=YES 
     OPENFRAME_LICENSE_PATH=/opt/tmaxapp/license/OPENFRAME TMAX_LICENSE_PATH=/opt/tmaxapp/license/TMAX
     ```

7. Ejecute el programa de instalación mediante el archivo base.properties. Por ejemplo: 

    ```
    [oframe7@ofdemo ~]$ chmod a+x OpenFrame_Base7_0_Linux_x86_64.bin 
    [oframe7@ofdemo ~]$ ./OpenFrame_Base7_0_Linux_x86_64.bin -f base.properties
    ```

    Cuando haya finalizado, el mensaje completo de instalación es mostrada.

8. Compruebe la estructura del directorio OpenFrame Base mediante el `ls -ltr` comando. Por ejemplo: 

     ```
     [oframe7@ofdemo OpenFrame]$ ls -ltr
     total 44

     drwxrwxr-x. 4 oframe7 oframe7 61 Nov 30 16:57 UninstallerData 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 bin 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 cpm drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 data 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 include 
     drwxrwxr-x. 2 oframe7 oframe7 8192 Nov 30 16:57 lib 
     drwxrwxr-x. 6 oframe7 oframe7 48 Nov 30 16:57 log 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 profile 
     drwxrwxr-x. 7 oframe7 oframe7 62 Nov 30 16:57 sample 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 schema 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 temp 
     drwxrwxr-x. 3 oframe7 oframe7 16 Nov 30 16:57 shared 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 license 
     drwxrwxr-x. 23 oframe7 oframe7 4096 Nov 30 16:58 core 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 config 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 scripts 
     drwxrwxr-x. 2 oframe7 oframe7 25 Nov 30 16:58 volume_default
     ```

9. Iniciar OpenFrame Base:

     ```
     [oframe7@ofdemo ~]$ cp /usr/lib/libtermcap.so.2 $TMAXDIR/lib
     Startup Tmax Server
     [oframe7@ofdemo ~]$ tmboot
     ```

     ![salida del comando tmboot](media/base-02.png)

10. Compruebe que el estado del proceso está listo mediante el comando tmadmin en si. RDY se muestra en el **estado** columna para cada uno de los procesos:

     ![salida del comando tmadmin](media/base-03.png)

11. Apagar OpenFrame Base:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(vtammgr:43) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLH downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLL downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TLM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-openframe-batch"></a>Instalar OpenFrame Batch

OpenFrame Batch consta de varios componentes que simulan entornos de gran sistema por lotes y se usa para ejecutar trabajos de batch en Azure.

**Para instalar el lote**

1. Asegúrese de que la instalación base se ha realizado correctamente y, después, compruebe que la OpenFrame\_Batch7\_0\_Fix2\_MVS\_Linux\_x86\_archivo instalador de 64. bin y archivo de configuración batch.Properties están presentes:

2. En el símbolo del sistema, escriba `vi batch.properties` para editar el archivo batch.properties mediante vi.

3. Modifique los parámetros de la manera siguiente:

     ```
     OPENFRAME_HOME = /opt/tmaxapp/OpenFrame
     DEFAULT_VOLSER=DEFVOL 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     RESOURCE_SHMKEY=66991 
     #JOBQ_DATASET_CREATE=YES 
     #OUTPUTQ_DATASET_CREATE=YES 
     DEFAULT_JCLLIB_CREATE=YES 
     DEFAULT_PROCLIB_CREATE=YES 
     DEFAULT_USERLIB_CREATE=YES 
     TJES_USERNAME=tibero 
     TJES_PASSWORD=tmax 
     TJES_DATABASE=oframe 
     BATCH_TABLE_CREATE=YES
     ```

4. Para ejecutar al instalador del lote, en el símbolo del sistema, escriba:

     ```
     ./OpenFrame_Batch7_0_Fix2_MVS_Linux_x86_64.bin -f batch.properties
     ```

5. Una vez completada la instalación, inicie los conjuntos de OpenFrame instalados escribiendo `tmboot` en el símbolo del sistema.

    ![salida tmboot](media/tmboot-01.png)

6. Tipo `tmadmin` en el símbolo del sistema para comprobar el proceso de OpenFrame.

    ![Pantalla tMax Admin](media/tmadmin-01.png)

7. Ejecute los comandos siguientes:

     ```
     $$2 NODE1 (tmadm): quit 
     ADM quit for node (NODE1)
     ```

8. Use el `tmdown` comando para iniciar y apagar el lote:

     ```
     [oframe7@ofdemo ~]$tmdown
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:44) downed: Wed Sep  7 16:01:46 2016
     TMDOWN: SERVER(vtammgr: 43) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:45) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:46) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjinit:55) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:49) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:51) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:52) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:53) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLL downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TLM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLH downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-tacf"></a>Instalar TACF

TACF Manager es un módulo de servicio OpenFrame que controla el acceso de usuario a los sistemas y los recursos a través de la seguridad RACF.

**Para instalar TACF**

1. Compruebe que la OpenFrame\_Tacf7\_0\_Fix2\_Linux\_x86\_64. bin instalador junto tacf.properties archivo de configuración están presentes.
2. Asegúrese de que la instalación por lotes se realizó correctamente y, después, usar vi para abrir el archivo tacf.properties (`vi tacf.properties`).
3. Modifique los parámetros TACF:

     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame 
     USE_OS_AUTH=NO 
     TACF_USERNAME=tibero 
     TACF_PASSWORD=tmax 
     TACF_DATABASE=oframe 
     TACF_TABLESPACE=TACF00 
     TACF_TABLE_CREATE=YES
     ```

4. Después de completar el instalador TACF, se aplican las variables de entorno TACF. En el símbolo del sistema, escriba:

     ```
     source \~/.bash\_profile
     ```

5. Ejecute al instalador TACF. En el símbolo del sistema, escriba:

     ```
     ./OpenFrame_Tacf7_0_Fix2_Linux_x86_64.bin -f tacf.properties
     ```

     El resultado tiene un aspecto similar al siguiente:

     ```
     Wed Dec 07 17:36:42 EDT 2016
     Free Memory: 18703 kB 
     Total Memory: 28800 kB

     4 Command Line Args: 
     0:  -f 1:  tacf.properties 
     2:  -m 
     3:  SILENT 
     java.class.path: 
     /tmp/install.dir.41422/InstallerData 
     /tmp/install.dir.41422/InstallerData/installer.zip 
     ZGUtil.CLASS_PATH: 
     /tmp/install.dir.41422/InstallerData 
     tmp/install.dir.41422/InstallerData/installer.zip 
     sun.boot.class.path: 
     /tmp/install.dir.41422/Linux/resource/jre/lib/resources.jar /tmp/install.dir.41422/Linux/resource/jre/lib/rt.jar /tmp/install.dir.41422/Linux/resource/jre/lib/sunrsasign.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jsse.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jce.jar /tmp/install.dir.41422/Linux/resource/jre/lib/charsets.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jfr.jar /tmp/install.dir.41422/Linux/resource/jre/classes
     ```

6. En el símbolo del sistema, escriba `tmboot` reiniciar OpenFrame. El resultado tiene un aspecto similar al siguiente:

     ```
     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/7 
     TMBOOT: TMM is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLL is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLH is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: TLM(tlm) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsasvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrlhsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdsedt) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrcmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofruisvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsmlog) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(vtammgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjschd) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjinit) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjhist) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjspbk) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrpmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmtsmgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(tmsvr) is starting: Wed Sep  7 17:48:53 2016
     ```

7. Compruebe que el estado del proceso está listo mediante `tmadmin` en el `si` comando. Por ejemplo: 

     ```
     [oframe7\@ofdemo \~]\$ tmadmin
     ```

     En el **estado** RDY de columna, aparece:

    ![RDY en la columna de estado](media/tmboot-02.png)

8. Ejecute los comandos siguientes:

     ```
     $$2 NODE1 (tmadm): quit 
     DM quit for node (NODE1)

     [oframe7@ofdemo ~]$ tacfmgr 
     Input USERNAME  : ROOT 
     Input PASSWORD  : SYS1

     TACFMGR: TACF MANAGER START!!!
     QUIT TACFMGR: TACF MANAGER END!!!

     [oframe7@ofdemo ~]$ tmdow
     ```

9. Cierre el servidor mediante el `tmdown` comando. El resultado tiene un aspecto similar al siguiente:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(tmsvr:60) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLL downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLH downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TLM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-prosort"></a>Instalación ProSort

ProSort es una utilidad que se usan en transacciones por lotes para ordenar los datos.

**Para instalar ProSort**

1. Asegúrese de que la instalación por lotes es correcta y, a continuación, compruebe que la **prosort prosort bin\_2sp3-linux64-2123-opt.tar.gz** archivo instalador está presente.

2. Ejecute el programa de instalación mediante el archivo de propiedades. En el símbolo del sistema, escriba:

     ```
     tar -zxvf prosort-bin-prosort\_2sp3-linux64-2123-opt.tar.gz
     ```

3. Mover el directorio prosort a la ubicación principal. En el símbolo del sistema, escriba:

     ```
     mv prosort /opt/tmaxapp/prosort
     ```

4. Cree un subdirectorio de licencia y copie allí el archivo de licencia. Por ejemplo: 

     ```
     cd /opt/tmaxapp/prosort 
     mkdir license 
     cp /opt/tmaxsw/oflicense/prosort/license.xml /opt/tmaxapp/prosort/license
     ```

5. Abra bash.profile en vi (`vi .bash_profile`) y actualícelo como se indica a continuación:

     ```bash
     #       PROSORT

     PROSORT_HOME=/opt/tmaxapp/prosort 
     PROSORT_SID=gbg 
     PATH=$PATH:$PROSORT_HOME/bin LD_LIBRARY_PATH=$PROSORT_HOME/lib:$LD_LIBRARY_PATH LIBPATH$PROSORT_HOME/lib:$LIBPATH 
     export PROSORT_HOME PROSORT_SID 
     PATH LD_LIBRARY_PATH LIBPATH 
     PATH=$PATH:$OPENFRAME_HOME/shbin 
     export PATH
     ```

6. Para ejecutar el perfil de bash, en el símbolo del sistema, escriba: `. .bash_profile`

7. Cree el archivo de configuración. Por ejemplo: 

     ```
     oframe@oframe7: cd /opt/tmaxapp/prosort/config 
     oframe@oframe7: ./gen_tip.sh 
     Using PROSORT_SID "gbg"
      /home/oframe7/prosort/config/gbg.tip generated
     ```

8. Crear el vínculo simbólico. Por ejemplo: 

     ```
     oframe@oframe7: cd /opt/tmaxapp/OpenFrame/util/ 
     oframe@oframe7home/oframe7/OpenFrame/util :  ln -s DFSORT SORT
     ```

9. Comprobar la instalación ProSort ejecutando el `prosort -h` comando. Por ejemplo: 

     ```
     oframe@oframe7: prosort -h

     Usage: prosort [options] [sort script files]
     options ------
     -h             Display this information 
     -v             Display version information 
     -s             Display state information 
     -j             Display profile information 
     -x             Use SyncSort compatible mode
     ```

## <a name="install-ofcobol"></a>Instalar OFCOBOL

OFCOBOL es el compilador OpenFrame que interpreta los programas de COBOL del gran sistema. 

**Para instalar OFCOBOL**

1. Asegúrese de que se realizó correctamente la instalación en línea por lotes y, a continuación, compruebe que la OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64. bin instalador archivo está presente.

2. Para ejecutar al instalador OFCOBOL, en el símbolo del sistema, escriba:

     ```
      ./OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64.bin
     ```

3. Lea el contrato de licencia y presione ENTRAR para continuar.

4. Acepte el contrato de licencia. Una vez completada la instalación, aparecerá el siguiente mensaje:

     ```
     Choose Install Folder 
     --------------------
     Where would you like to install?
     Default Install Folder: /home/oframe7/OFCOBOL

     ENTER AN ABSOLUTE PATH, OR PRESS <ENTER> TO ACCEPT THE DEFAULT : /opt/tmaxapp/OFCOBOL

     INSTALL FOLDER IS: /opt/tmaxapp/OFCOBOL 
     IS THIS CORRECT? (Y/N): Y[oframe7@ofdemo ~]$ vi .bash_profile

     ============================================================================ Installing... 
     ------------
     [==================|==================|==================|==================] 
     [------------------|------------------|------------------|------------------]

     =============================================================================== Installation Complete 
     --------------------
     Congratulations. OpenFrame_COBOL has been successfully installed
     PRESS <ENTER> TO EXIT THE INSTALLER
     ```

5. Abra el perfil de bash en vi (`vi .bash_profile`) y compruebe que se actualiza con las variables OFCOBOL.
6. Ejecutar el perfil de bash. En el símbolo del sistema, escriba:

     ```
      source ~/.bash_profile
     ```

7. Copie la licencia OFCOBOL en la carpeta de instalación. Por ejemplo: 
     ```
     mv licofcob.dat $OFCOB_HOME/license
     ```
8. Vaya al archivo de configuración de tjclrun.conf OpenFrame y ábralo en vi. Por ejemplo: 
     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

   Esta es la sección SYSLIB antes del cambio:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib
     ```
   Esta es la sección SYSLIB después del cambio:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib :${ODBC_HOME}/lib 
     :${OFCOB_HOME}/lib
     ```
9. Revise el OpenFrame\_COBOL\_InstallLog.log vi de archivo y compruebe que no hay ningún error. Por ejemplo: 
     ```
     [oframe7@ofdemo ~]$ vi $OFCOB_HOME/UninstallerData/log/OpenFrame_COBOL_InstallLog.log 
     …….. 
     Summary 
     ------
     Installation: Successful. 
     131 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalError
     ```
10. Use el `ofcob --version` comando y revise el número de versión para comprobar la instalación. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ ofcob --version 
     OpenFrame COBOL Compiler 3.0.54 
     CommitTag:: 645f3f6bf7fbe1c366a6557c55b96c48454f4bf
     ```

11. Reiniciar OpenFrame utilizando el `tmdown/tmboot` comando.

## <a name="install-ofasm"></a>Instalar OFASM

OFASM es el compilador OpenFrame que interpreta los programas de ensamblador de mainframe.

**Para instalar OFASM**

1. Asegúrese de que se realizó correctamente la instalación en línea por lotes y, a continuación, compruebe que la OpenFrame\_ASM3\_0\_Linux\_x86\_64. bin instalador archivo está presente.

2. Ejecute el programa de instalación. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ ./OpenFrame_ASM3_0_Linux_x86_64.bin
     ```

3. Lea el contrato de licencia y presione ENTRAR para continuar.
4. Acepte el contrato de licencia.
5. Compruebe que el perfil de bash se actualiza con las variables OFASM. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ source .bash_profile
     [oframe7@ofdemo ~]$ ofasm --version 
     # TmaxSoft OpenFrameAssembler v3 r328 
     (3ff35168d34f6e2046b96415bbe374160fcb3a34)

     [oframe7@ofdemo OFASM]$ vi .bash_profile

     # OFASM ENV 
     export OFASM_HOME=/opt/tmaxapp/OFASM 
     export OFASM_MACLIB=$OFASM_HOME/maclib/free_macro 
     export PATH="${PATH}:$OFASM_HOME/bin:" 
     export LD_LIBRARY_PATH="./:$OFASM_HOME/lib:$LD_LIBRARY_PATH"
     ```

6. Abra el archivo de configuración de tjclrun.conf OpenFrame en vi y modifíquelo como sigue:

     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

     Esta es la sección [SYSLIB] *antes* el cambio:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib
     ```

     Esta es la sección [SYSLIB] *después* el cambio:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib:${OFASM_HOME}/lib
     ```

7. Abra el OpenFrame\_ASM\_InstallLog.log vi de archivo y compruebe que no hay ningún error. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ vi 
     $OFASM_HOME/UninstallerData/log/OpenFrame_ASM_InstallLog.log 
     …….. 
     Summary 
     ------

     Installation: Successful.

     55 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalErrors
     ```

8. Reiniciar OpenFrame mediante la emisión de uno de los siguientes comandos:

     ```
     tmdown / tmboot
     ```

     — o:

     ```
     oscdown / oscboot
     ```

## <a name="install-osc"></a>Instalar OSC

OSC es el entorno de OpenFrame similar a IBM CICS que admite transacciones de OLTP a alta velocidad y otras funciones de administración.

**Para instalar OSC**

1. Asegúrese de que la instalación base se ha realizado correctamente y, después, compruebe que la OpenFrame\_OSC7\_0\_Fix2\_Linux\_x86\_archivo instalador de 64. bin y archivo de configuración osc.properties son presente.
2. Edite los parámetros siguientes en el archivo osc.properties:
     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame OSC_SYS_OSC_NCS_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_NCS OSC_APP_OSC_TC_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_TC
     ```

3. Ejecute el programa de instalación mediante el archivo de propiedades como se muestra:

     ```
     [oframe7@ofdemo ~]$ chmod a+x OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin [oframe7@ofdemo ~]$ ./OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin -f osc.properties
     ```

     Cuando termine, se muestra el mensaje "Instalación finalizada".

4. Compruebe que el perfil de bash se actualiza con variables OSC.
5. Revise el OpenFrame\_OSC7\_0\_Fix2\_InstallLog.log archivo. Debe tener un aspecto similar al siguiente:

     ```
     Summary 
     ------ 
     Installation: Successful.

     233 Successes
     0 Warnings
     0 NonFatalErrors
     0 FatalError
     ```

6. Utilice vi para abrir el archivo de configuración ofsys.seq. Por ejemplo: 

     ```
     vi $OPENFRAME_HOME/config/ofsys.seq
     ```

7. En el \#BASE y \#secciones de BATCH, edite los parámetros como se muestra.

     ```
     Before changes
     #BASE
     ofrsasvr
     ofrlhsvr
     ofrdmsvr
     ofrdsedt
     ofrcmsvr
     ofruisvr
     ofrsmlog
     vtammgr
     TPFMAGENT

     #BATCH 
     #BATCH#obmtsmgr
     #BATCH#ofrpmsvr
     #BATCH#obmjmsvr
     #BATCH#obmjschd
     #BATCH#obmjinit
     #BATCH#obmjhist
     #BATCH#obmjspbk
     #TACF #TACF#tmsvr

     After changes  #BATCH
     #BASE          obmtsmgr 
     ofrsasvr       ofrpmsvr
     ofrlhsvr       obmjmsvr
     ofrdmsvr       obmjschd
     ofrdsedt       obmjinit
     ofrcmsvr       obmjhist
     ofruisvr       obmjspbk
     ofrsmlog
     vtammgr        #TACF
     TPFMAGENT      tmsvr
    ```

8. Copie el archivo de licencia. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ cp /home/oframe7/oflicense/ofonline/licosc.dat 

     $OPENFRAME_HOME/license

     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/license 
     oframe@oframe7/OpenFrame/license / ls -l 
     -rwxr-xr-x. 1 oframe mqm 80 Sep 12 01:37 licosc.dat 
     -rwxr-xr-x. 1 oframe mqm 80 Sep  8 09:40 lictacf.dat 
     -rwxrwxr-x. 1 oframe mqm 80 Sep  3 11:54 lictjes.da
     ```

9. Para iniciar y apagar OSC, inicializar la memoria compartida de región CICS escribiendo `osctdlinit OSCOIVP1` en el símbolo del sistema.

10. Ejecute `oscboot` a OSC de arranque. El resultado tiene un aspecto similar al siguiente:

     ```
     OSCBOOT : pre-processing       [ OK ]

     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/12 
          TMBOOT: TMM is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLL is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLH is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: TLM(tlm) is starting: Mon Sep 12 01:40:25 2016 
     ```

11. Para comprobar que el estado del proceso está listo, use el `tmadmin` en si. Todos los procesos que deben mostrar RDY en el **estado** columna.

    ![Mostrar RDY de procesos](media/tmadmin-02.png)

12. Cierre OSC mediante el `oscdown` comando.

## <a name="install-jeus"></a>Instalar JEUS

JEUS (solución de usuario de Java Enterprise) proporciona la capa de presentación del servidor de aplicaciones web OpenFrame.

Antes de instalar JEUS, instale el paquete de Apache Ant, que proporciona las bibliotecas y herramientas de línea de comandos necesarias para instalar JEUS.

**Para instalar Apache Ant**

1. Descarga Ant binario mediante el `wget` comando. Por ejemplo: 

     ```
     wget http://apache.mirror.cdnetworks.com/ant/binaries/apacheant-1.9.7-bin.tar.gz
     ```

2. Use el `tar` utilidad para extraer el archivo binario y muévalo a una ubicación adecuada. Por ejemplo: 

     ```
     tar -xvzf apache-ant-1.9.7-bin.tar.gz
     ```

3. Para mejorar la eficacia, cree un vínculo simbólico:

     ```
     ln -s apache-ant-1.9.7 ant
     ```

4. Abra el perfil de bash en vi (`vi .bash_profile`) y se actualiza con las siguientes variables:

     ```
     # Ant ENV
     export ANT_HOME=$HOME/ant 
     export PATH=$HOME/ant/bin:$PATH
     ```

5.  Se aplican a la variable de entorno modificada. Por ejemplo: 

     ```
     [oframe7\@ofdemo \~]\$ source \~/.bash\_profile
     ```

**Para instalar JEUS**

1. Expanda el instalador con la `tar` utilidad. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ tar -zxvf jeus704.tar.gz
     ```

2. Crear un **jeus** carpeta (`mkdir jeus7`) y descomprima el archivo binario.
3. Cambie a la **instalación** directorio (o use el parámetro JEUS para su propio entorno). Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ cd jeus7/setup/
     ```

4. Ejecutar `ant clean-all` antes de realizar la compilación. El resultado tiene un aspecto similar al siguiente:

     ```
     Buildfile: /home/oframe7jeus7/setup/build.xml

     clean-bin:
     delete-domain:
     [echo] Deleting a domain configuration: domain = jeus_domain
     delete-nodesxml:
     clean-config:
     clean-all:
     BUILD SUCCESSFUL
     Total time: 0 seconds
     ```

5.  Realizar una copia de seguridad del archivo config-dominio-template.properties. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ cp domain-config-template.properties domain-configtemplate.properties.bkp
     ```

6. Abra el archivo de dominio-config-template.properties en vi:

     ```
     [oframe7\@ofdemo setup]\$ vi domain-config-template.properties
     ```

7. Cambie `jeus.password=jeusadmin nodename=Tmaxsoft` a `jeus.password=tmax1234 nodename=ofdemo`.

8. Ejecute el `ant install` comando para compilar JEUS.
9.  Actualizar el .bash\_archivo de perfil con las variables JEUS tal como se muestra:

     ```
     # JEUS ENV 
     export JEUS_HOME=/opt/tmaxui/jeus7 PATH="/opt/tmaxui/jeus7/bin:/opt/tmaxui/jeus7/lib/system:/opt/tmaxui/jeus7/webserver/bin:$ {PATH}" 
     export PATH
     ```

10. Ejecutar el perfil de bash. Por ejemplo: 

     ```
     [oframe7@ofdemo setup]$ . .bash_profile
     ```

11. *Opcional*. Crear un alias para el cierre fácil y el arranque de los componentes JEUS:

     ```     
     # JEUS alias

     alias dsboot='startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin'
     alias msboot='startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin' 
     alias msdown=‘jeusadmin -u administrator -p tmax1234 "stop-server server1“’ 
     alias dsdown=‘jeusadmin -domain jeus_domain -u administrator -p tmax1234 "local-shutdown“’
     ```

12. Para comprobar la instalación, inicie el servidor de administración de dominio como se muestra:

     ```
     [oframe7@ofdemo ~]$ startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin
     ```

13. Compruebe por inicio de sesión web mediante la sintaxis:

     ```
     http://<IP>:<port>/webadmin/login
     ```

     Por ejemplo, <http://192.168.92.133:9736/webadmin/login.> aparece la pantalla de inicio de sesión:
    
     ![Pantalla de inicio de sesión WebAdmin JEUS](media/jeus-01.png)

     > [!NOTE]
     > Si experimenta problemas con la seguridad del puerto, abra el puerto 9736 o deshabilite el firewall (`systemctl stop firewall`).

14. Para cambiar el nombre de host para el Servidor1, haga clic en **bloquear & Editar**, a continuación, haga clic en **server1**. En la ventana del servidor, cambie el nombre de host de la manera siguiente:

    1.  Cambio **Nodename** a **ofdemo**.
    2.  Haga clic en **Aceptar** en el lado derecho de la ventana.
    3.  Haga clic en **aplicar cambios** en la parte inferior izquierda de la ventana y para la descripción, escriba *cambio de nombre de host*.

    ![Pantalla WebAdmin JEUS](media/jeus-02.png)

15. Compruebe que la configuración es correcta en la pantalla de confirmación.

    ![pantalla de servidor jeus_domain](media/jeus-03.png)

16. Iniciar el proceso de servidor administrado "server1" con el siguiente comando:

     ```
     [oframe7@ofdemo ~]$ startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin
     ```

## <a name="install-ofgw"></a>Instalar OFGW

OFGW es la puerta de enlace OpenFrame que admite la comunicación entre el emulador de terminal 3270 y la base OSI y administra las sesiones entre el emulador de terminal y OSI.

**Para instalar OFGW**

1. Asegúrese de que JEUS se instaló correctamente y, después, compruebe que la OFGW7\_0\_1\_Generic.bin instalador archivo está presente.
2. Ejecute el programa de instalación. Por ejemplo: 

     ```
     [oframe7@ofdemo ~]$ ./OFGW7_0_1_Generic.bin
     ````

3. Utilice las siguientes ubicaciones para los mensajes correspondientes:
     -   Directorio particular JEUS
     -   Nombre de dominio JEUS
     -   Nombre del servidor JEUS
     -   Tibero Driver
     -   Id. de nodo tMax ofdemo

4. Acepte el resto de los valores predeterminados, a continuación, presione ENTRAR para salir del instalador.

5. Compruebe que la dirección URL de OFGW funciona según lo esperado:

     ```
     Type URL 
     http://192.168.92.133:8088/webterminal/ and press enter
      < IP >               :8088/webterminal/
     ```

     Aparecerá la pantalla siguiente:

    ![OpenFrame WebTerminal](media/ofgw-01.png)

## <a name="install-ofmanager"></a>Instalar OFManager

OFManager proporciona funciones de administración y operación para OpenFrame en el entorno web.

**Para instalar OFManager**

1. Compruebe que la OFManager7\_Generic.bin instalador archivo está presente.
2. Ejecute el programa de instalación. Por ejemplo: 

     ```
     OFManager7_Generic.bin
     ```

3.  Presione ENTRAR para continuar y, después, acepte el contrato de licencia.
4.  Elija la carpeta de instalación.
5.  Acepte los valores predeterminados.
6.  Elija Tibero como la base de datos.
7.  Presione ENTRAR para salir del instalador.
8.  Compruebe que la dirección URL de OFManager funciona según lo esperado:

     ```
     Type URL http://192.168.92.133:8088/ofmanager and press enter <  IP >  : < PORT >  ofmanager Enter ID:   ROOT 
     Password: SYS1
     ```

Aparecerá la pantalla de inicio:

![Pantalla de inicio de sesión tMax OpenFrame Manager](media/ofmanager-01.png)

Esto completa la instalación de los componentes OpenFrame.

## <a name="next-steps"></a>Pasos siguientes

Si está considerando una migración de mainframes, está disponible para ayudarle a nuestro ecosistema de asociados de expansión. Para más información sobre cómo elegir una solución de asociados, consulte [Platform Modernization Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

-   [Comience a usar Azure](https://docs.microsoft.com/azure/)
-   [Documentación de Host Integration Server (HIS)](https://docs.microsoft.com/host-integration-server/)
-   [Guía del centro de datos Virtual de Azure mediante Lift-and-Shift](https://blogs.msdn.microsoft.com/azurecat/2018/03/12/new-whitepaper-azure-virtual-datacenter-lift-and-shift-guide/)
