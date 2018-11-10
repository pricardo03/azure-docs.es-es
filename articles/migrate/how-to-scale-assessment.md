---
title: Escalado de detección y evaluación mediante Azure Migrate | Microsoft Docs
description: Se describe cómo evaluar un número elevado de máquinas locales mediante el servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: b5685640a55e2ce52a202c341cb293fe9315ab14
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240195"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Detección y evaluación de un entorno grande de VMware

Azure Migrate tiene un límite de 1500 máquinas por proyecto. En este artículo se describe cómo evaluar un número elevado de máquinas virtuales (VM) locales mediante [Azure Migrate](migrate-overview.md).   

## <a name="prerequisites"></a>Requisitos previos

- **VMware**: las máquinas virtuales que planea migrar deben administrarse mediante vCenter Server, versión 5.5, 6.0 o 6.5. Además, necesita un host de ESXi que ejecute la versión 5.0 o posterior para implementar la máquina virtual del recopilador.
- **Cuenta de vCenter**: necesita una cuenta de solo lectura para acceder a vCenter Server. Azure Migrate usa esta cuenta para detectar las máquinas virtuales locales.
- **Permisos**: en vCenter Server, necesitará permisos para crear una máquina virtual mediante la importación de un archivo en formato OVA.
- **Configuración de estadísticas**: este requisito solo es aplicable al [modelo de detección de un solo uso](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods). Para el modelo de detección de un solo uso, la configuración de las estadísticas de vCenter Server se debe establecer en el nivel 3 antes de empezar la implementación. El nivel estadístico se establecerá en 3 para cada uno de los intervalos de colección de día, semana y mes. Si el nivel es inferior a 3 en cualquiera de los tres intervalos de colección, la valoración funcionará, pero los datos de rendimiento de almacenamiento y red no se recopilarán. Las recomendaciones de tamaño se basarán entonces en los datos de rendimiento de CPU y memoria y en los datos de configuración de discos y adaptadores de red.

### <a name="set-up-permissions"></a>Configuración de permisos

Azure Migrate necesita acceso a los servidores de VMware para detectar automáticamente las máquinas virtuales para su evaluación. La cuenta de VMware necesita los siguientes permisos:

- Tipo de usuario: al menos un usuario de solo lectura.
- Permisos: Data Center -> Propagate to Child Object, role=Read-only (Centro de datos -> Propagar a objeto secundario, rol = Solo lectura).
- Detalles: el usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.
- Para restringir el acceso, asigne el rol Sin acceso con Propagar a objetos secundarios a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).

Si va a implementar en un entorno de inquilinos, esta es una manera de configurar esta opción:

1.  Cree un usuario por inquilino y use [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) para asignar permisos de solo lectura a todas las máquinas virtuales que pertenezcan a un inquilino determinado. A continuación, use esas credenciales para la detección. RBAC garantiza que el usuario correspondiente de vCenter solo tendrá acceso a las máquinas virtuales específicas del inquilino.
2. Configure RBAC para los usuarios de otros inquilinos, como se describe en el ejemplo siguiente para el usuario n.º 1 y el usuario n.º 2:

    - En **Nombre de usuario** y **Contraseña**, especifique las credenciales de la cuenta de solo lectura que el recopilador utilizará para detectar las máquinas virtuales.
    - Centrodedatos1: asigne permisos de solo lectura al usuario n.º 1 y al usuario n.º 2. No propague esos permisos a todos los objetos secundarios, porque establecerá los permisos en las máquinas virtuales individuales.

      - VM1 (inquilino n.º 1) (permiso de solo lectura al usuario n.º 1)
      - VM2 (inquilino n.º 1) (permiso de solo lectura al usuario n.º 1)
      - VM3 (inquilino n.º 2) (permiso de solo lectura al usuario n.º 2)
      - VM4 (inquilino n.º 2) (permiso de solo lectura al usuario n.º 2)

   - Si realiza la detección con las credenciales del usuario n.º 1, solo se detectarán VM1 y VM2.

## <a name="plan-your-migration-projects-and-discoveries"></a>Planificación de los proyectos de migración y las detecciones

Un único recopilador de Azure Migrate admite la detección desde varias instancias de vCenter Server (una detrás de otra) y también admite la detección a varios proyectos de migración (uno detrás de otro).

El recopilador, en el caso de una detección de una sola vez, funciona en un modelo de disparo y olvido; una vez que se realiza una detección, puede usar el mismo recopilador para recopilar datos de otro servidor vCenter o enviarlos a otro proyecto de migración. En el caso de la detección continua, un dispositivo solo se conecta a un proyecto, por lo que no se puede usar el mismo recopilador para desencadenar una segunda detección.

Planee las detecciones y evaluaciones en función de los límites siguientes:

| **Entidad** | **Límite de máquinas** |
| ---------- | ----------------- |
| proyecto    | 1500             |
| Detección  | 1500             |
| Evaluación | 1500             |

Tenga en cuenta estas consideraciones de planeación:

- Al realizar una detección mediante Azure Migrate Collector, puede establecer el ámbito de detección en una carpeta, un centro de datos, un clúster o un host de vCenter Server.
- Para hacer más de una detección, verifique en vCenter Server si las máquinas virtuales que desea detectar están en carpetas, centros de datos, clústeres o hosts que admiten la limitación de 1500 máquinas.
- Se recomienda que, para los fines de evaluación, mantenga las máquinas con interdependencias dentro del mismo proyecto y de la misma evaluación. En vCenter Server, asegúrese de que las máquinas dependientes están en la misma carpeta, centro de datos o clúster para la evaluación.

Dependiendo del escenario, puede dividir las detecciones como se indica a continuación:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Varias instancias de vCenter Server con menos de 1500 máquinas virtuales
Si tiene varios servidores de vCenter en el entorno y el número total de máquinas virtuales es inferior a 1500, puede usar el enfoque siguiente en función del escenario:

**Detección de una sola vez:** puede usar un único recopilador y un único proyecto de migración para detectar todas las máquinas virtuales en todos los servidores vCenter. Como el recopilador de detección de una sola vez detecta los servidores vCenter de uno en uno, puede ejecutar el mismo recopilador en todos los servidores vCenter, uno detrás de otro, y apuntar el recopilador al mismo proyecto de migración. Después de completar todas las detecciones, puede crear las evaluaciones de las máquinas.

**Detección continua:** en el caso de la detección continua, un dispositivo solo puede estar conectado a un único proyecto. Por tanto, tendrá que implementar un dispositivo para cada uno de los servidores vCenter y, después, crear un proyecto para cada dispositivo y desencadenar las detecciones en consecuencia.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Varias instancias de vCenter Server con más de 1500 máquinas virtuales

Si tiene varias instancias de vCenter Server con menos de 1500 máquinas virtuales por vCenter Server, pero más de 1500 máquinas virtuales entre todas las instancias de vCenter Server, debe crear varios proyectos de migración (un proyecto de migración solamente puede contener 1500 máquinas virtuales). Puede conseguirlo creando un proyecto de migración por instancia de vCenter Server y dividir las detecciones.

**Detección de una sola vez:** puede usar un único recopilador para detectar cada servidor vCenter (uno detrás de otro). Si desea que las detecciones se inicien al mismo tiempo, también puede implementar varios dispositivos y ejecutar las detecciones en paralelo.

**Detección continua:** tendrá que crear varios dispositivos de recopilador (uno para cada servidor vCenter) y conectar cada dispositivo a un proyecto y una detección de desencadenador en consecuencia.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Más de 1500 máquinas en una sola instancia de vCenter Server

Si tiene más de 1500 máquinas virtuales en una sola instancia de vCenter Server, debe dividir la detección en varios proyectos de migración. Para dividir las detecciones, puede aprovechar el campo Ámbito en el dispositivo y especificar el host, el clúster, la carpeta o el centro de datos que quiere detectar. Por ejemplo, si tiene dos carpetas en vCenter Server, una con 1000 máquinas virtuales (Carpeta1) y otra con 800 (Carpeta2), puede usar el campo de ámbito para dividir las detecciones entre esas carpetas.

**Detección de una sola vez:** puede usar el mismo recopilador para desencadenar ambas detecciones. En la primera detección, puede especificar Carpeta1 como el ámbito y apuntarla al primer proyecto de migración; una vez completada la primera detección, puede usar el mismo recopilador, cambiar su ámbito a Carpeta2 y los detalles del proyecto de migración al segundo proyecto de migración y realizar la segunda detección.

**Detección continua:** en este caso, tendrá que crear dos dispositivos de recopilador, para el primer recopilador, especificar el ámbito como Carpeta1 y conectarlo al primer proyecto de migración. Puede iniciar en paralelo la detección de Carpeta2 mediante el segundo dispositivo de recopilador y conectarlo al segundo proyecto de migración.

### <a name="multi-tenant-environment"></a>Entorno multiinquilino

Si tiene un entorno que se comparte entre los inquilinos y no desea detectar las máquinas virtuales de un inquilino en la suscripción de otro inquilino, puede usar el campo Ámbito en el dispositivo recopilador para definir el ámbito de la detección. Si los inquilinos comparten hosts, cree una credencial que tenga acceso de solo lectura solamente a las máquinas virtuales que pertenecen al inquilino específico y, luego, use esta credencial en el dispositivo recopilador y especifique el ámbito como el host para realizar la detección.

## <a name="discover-on-premises-environment"></a>Detectar el entorno local

Cuando esté listo con el plan, puede iniciar la detección de las máquinas virtuales locales:

### <a name="create-a-project"></a>Crear un proyecto

Cree un proyecto de Azure Migrate según sus necesidades:

1. En Azure Portal, haga clic en **Crear un recurso**.
2. Busque **Azure Migrate** y seleccione el servicio **Azure Migrate** en los resultados de búsqueda. Seleccione **Crear**.
3. Especifique un nombre de proyecto y la suscripción de Azure para el proyecto.
4. Cree un nuevo grupo de recursos.
5. Especifique la ubicación en la que desea crear el proyecto y seleccione **Crear**. Tenga en cuenta que todavía puede evaluar las máquinas virtuales para una ubicación de destino diferente. La ubicación especificada para el proyecto se utiliza para almacenar los metadatos que se recopilan a partir de máquinas virtuales locales.

### <a name="set-up-the-collector-appliance"></a>Configuración del dispositivo de recopilador

Azure Migrate crea una VM local conocida como el dispositivo de recopilador. Esta máquina virtual detecta máquinas virtuales de VMware locales y envía metadatos sobre ellas al servicio Azure Migrate. Para configurar la aplicación del recopilador, descargue un archivo OVA e impórtelo a la instancia de vCenter Server local.

#### <a name="download-the-collector-appliance"></a>Descarga del dispositivo de recopilador

Si tiene varios proyectos, tiene que descargar la aplicación del recopilador solo una vez en vCenter Server. Después de descargar y configurar la aplicación, ejecútela en cada proyecto y especifique el identificador de proyecto exclusivo y la clave.

1. En el proyecto de Azure Migrate, haga clic en **Introducción** > **Detectar y evaluar** > **Detectar máquinas**.
2. En **Detectar máquinas**, hay dos opciones disponibles para el dispositivo. Haga clic en **Descargar** para descargar el dispositivo adecuado según sus preferencias.

    a. **Detección de una sola vez:** el dispositivo para este modelo se comunica con vCenter Server para recopilar metadatos sobre las máquinas virtuales. Para la recopilación de datos de rendimiento de las máquinas virtuales, se basa en los datos de rendimiento históricos almacenados en vCenter Server y recopila el historial de rendimiento del último mes. En este modelo, Azure Migrate recopila los valores medios de contador (a diferencia del valor máximo) de cada métrica; obtenga [más información](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected). Como se trata de una detección de una sola vez, no se reflejan los cambios en el entorno local una vez completada la detección. Si quiere que los cambios se reflejen, tendrá que realizar una nueva detección del mismo entorno para el mismo proyecto.

    b. **Detección continua:** el dispositivo para este modelo genera perfiles continuamente del entorno local con el fin de recopilar datos de uso en tiempo real para cada máquina virtual. En este modelo, se recopilan contadores de número máximo de cada métrica (uso de la CPU, uso de memoria etc.). Este modelo no depende de la configuración de las estadísticas de vCenter Server para la recopilación de datos de rendimiento. Puede detener la generación de perfiles continua en cualquier momento desde el dispositivo.

    > [!NOTE]
    > La función de detección continua está en versión preliminar.

3. En **Copiar las credenciales del proyecto**, copie la clave y el identificador del proyecto. Los necesitará cuando configure el recopilador.


#### <a name="verify-the-collector-appliance"></a>Comprobación del dispositivo de recopilador

Compruebe que el archivo OVA es seguro, antes de implementarlo:

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.

2. Ejecute el siguiente comando para generar el código hash para el archivo OVA:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Ejemplo de uso: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Asegúrese de que el código hash generado coincida con la configuración siguiente.

#### <a name="one-time-discovery"></a>Detección de una sola vez

Para la versión 1.0.9.15 de OVA (publicada el 23/10/2018)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

Para la versión 1.0.9.14 de OVA (publicada el 24/8/2018)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

Para la versión 1.0.9.12 de OVA

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

Para la versión 1.0.9.8 de OVA

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | b5d9f0caf15ca357ac0563468c2e6251
SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

Para la versión 1.0.9.7 de OVA

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | d5b6a03701203ff556fa78694d6d7c35
SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

#### <a name="continuous-discovery"></a>Detección continua

Para la versión 1.0.10.4 de OVA

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

### <a name="create-the-collector-vm"></a>Creación de la VM de recopilador

Importe el archivo descargado a vCenter Server:

1. En la consola de cliente de vSphere, seleccione **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).

    ![Implementación de OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. En el Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF) > **Source** (Origen), especifique la ubicación del archivo OVA.
3. En **Name** (Nombre) y **Location** (Ubicación), especifique un nombre descriptivo para la VM de recopilador y el objeto de inventario en el que se hospedará la VM.
4. En **Host/Cluster** (Host o clúster), especifique el host o clúster donde se ejecutará el recopilador de máquina virtual.
5. En el almacenamiento, especifique el destino de almacenamiento para la VM de recopilador.
6. En **Disk Format** (Formato de disco), especifique el tamaño y el tipo de disco.
7. En **Network Mapping** (Asignación de red), especifique la red a la que se conectará la VM de recopilador. La red necesita conectividad a Internet, para enviar metadatos a Azure.
8. Revise y confirme la configuración y, a continuación, seleccione **Finish** (Finalizar).

### <a name="identify-the-id-and-key-for-each-project"></a>Identificación del identificador y de la clave de cada proyecto

Si tiene varios proyectos, asegúrese de identificar el identificador y la clave de cada uno. Necesita la clave al ejecutar el recopilador para detectar las máquinas virtuales.

1. En el proyecto, seleccione **Introducción** > **Detectar y evaluar** > **Detectar máquinas**.
2. En **Copiar las credenciales del proyecto**, copie la clave y el identificador del proyecto.
    ![Copia de las credenciales del proyecto](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="set-the-vcenter-statistics-level"></a>Configuración del nivel de estadísticas de vCenter

El dispositivo de recopilación detecta los siguientes metadatos estáticos sobre las máquinas virtuales seleccionadas.

1. Nombre para mostrar de la máquina virtual (en vCenter)
2. Ruta de acceso de inventario de la máquina virtual (host o carpeta en vCenter)
3. Dirección IP
4. Dirección MAC
5. Sistema operativo
5. Número de núcleos, discos, NIC
6. Tamaño de memoria, tamaños de disco
7. Y los contadores de rendimiento de la máquina virtual, el disco y la red se muestran en la tabla siguiente.

Para la detección de una sola vez, en la tabla siguiente se enumeran los contadores de rendimiento exactos que se recopilan, y también los resultados de la valoración que se ven afectados si no se recopila un contador determinado.

Para la detección continua, se recopilan los mismos contadores en tiempo real (con un intervalo de 20 segundos), por lo que no hay ninguna dependencia en el nivel de estadísticas de vCenter. Después, el dispositivo acumula los ejemplos de 20 segundos para crear un único punto de datos para cada 15 minutos mediante la selección del valor máximo de los ejemplos de 20 segundos y lo envía a Azure.

|Contador                                  |Nivel    |Nivel por dispositivo  |Impacto en la evaluación                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |N/D                |Costo y tamaño de VM recomendados                    |
|mem.usage.average                        | 1       |N/D                |Costo y tamaño de VM recomendados                    |
|virtualDisk.read.average                 | 2       |2                 |Tamaño del disco, costo de almacenamiento y tamaño de VM         |
|virtualDisk.write.average                | 2       |2                 |Tamaño del disco, costo de almacenamiento y tamaño de VM         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Tamaño del disco, costo de almacenamiento y tamaño de VM         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Tamaño del disco, costo de almacenamiento y tamaño de VM         |
|net.received.average                     | 2       |3                 |Tamaño de VM y costo de la red                        |
|net.transmitted.average                  | 2       |3                 |Tamaño de VM y costo de la red                        |

> [!WARNING]
> Para la detección de una sola vez, si ha configurado un nivel de estadística más alto, los contadores de rendimiento pueden tardar hasta un día en generarse. Por lo tanto, se recomienda ejecutar la detección después de un día. Para el modelo de detección continua, espere al menos un día después de iniciar la detección para que el dispositivo genere el perfil del entorno y, después, cree las evaluaciones.

### <a name="run-the-collector-to-discover-vms"></a>Ejecución del recopilador para detectar VM

Para cada detección que necesite realizar, ejecute el recopilador para detectar máquinas virtuales en el ámbito requerido. Ejecute las detecciones una detrás de otra. No se admiten las detecciones simultáneas, y cada detección debe tener un ámbito distinto.

1.  En la consola del cliente de vSphere, haga clic con el botón derecho en VM > **Open Console** (Abrir consola).
2.  Proporcione el idioma, la zona horaria y las preferencias de contraseña para el dispositivo.
3.  En el escritorio, seleccione el acceso directo **Run collector** (Ejecutar recopilador).
4.  En Azure Migrate Collector, abra **Set Up Prerequisites** (Configurar requisitos previos) y luego:

    a. Acepte los términos de licencia y lea la información de terceros.

    El recopilador comprueba que la VM tenga acceso a Internet.

    b. Si la máquina virtual tiene acceso a Internet a través de un proxy, seleccione **Configuración de proxy** y especifique el puerto de escucha y la dirección del proxy. Especifique las credenciales si el proxy requiere autenticación.

    El recopilador comprueba que el servicio del recopilador se está ejecutando. El servicio se instala de forma predeterminada en la VM de recopilador.

    c. Descargue e instale VMware PowerCLI.

5.  En **Specify vCenter Server details** (Especificar detalles de vCenter Server), haga lo siguiente:
    - Especifique el nombre (FQDN) o la dirección IP de vCenter Server.
    - En **Nombre de usuario** y **Contraseña**, especifique las credenciales de cuenta de solo lectura que utilizará el recopilador para detectar las máquinas virtuales en vCenter Server.
    - En **Seleccionar ámbito**, especifique un ámbito para la detección de máquinas virtuales. El recopilador solo puede detectar máquinas virtuales dentro del ámbito especificado. El ámbito se puede establecer en una carpeta, centro de datos o clúster específico. No debe contener más de 1000 máquinas virtuales.

6.  En **Specify migration project** (Especificar proyecto de migración), especifique el identificador y la clave del proyecto. Si no los copió, abra Azure Portal desde la máquina virtual de recopilador. En la página **Introducción** del proyecto, seleccione **Detectar máquinas** y copie los valores.  
7.  En **View collection progress** (Ver progreso de recopilación), supervise el proceso de detección y compruebe que los metadatos recopilados de las máquinas virtuales se encuentran dentro del ámbito. El recopilador proporciona un tiempo de detección aproximado.


#### <a name="verify-vms-in-the-portal"></a>Comprobación de VM en el portal

Para la detección de una sola vez, el tiempo de detección depende de cuántas máquinas virtuales se están detectando. Normalmente, para 100 máquinas virtuales, cuando el recopilador finaliza la ejecución, se necesita aproximadamente una hora para completar la recopilación de los datos de configuración y rendimiento. Puede crear las evaluaciones (tanto las basadas en rendimiento como las locales) inmediatamente después de que finalice la detección.

Para la detección continua (en versión preliminar), el recopilador generará perfiles del entorno local de forma continuada y seguirá enviando los datos de rendimiento en intervalos de una hora. Puede revisar las máquinas en el portal una hora después de que se inicie la detección. Se recomienda encarecidamente esperar al menos un día antes de crear evaluaciones basadas en el rendimiento para las máquinas virtuales.

1. En el proyecto de migración, haga clic en **Administrar** > **Máquinas**.
2. Compruebe que las VM que desea detectar aparecen en el portal.


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear un grupo](how-to-create-a-group.md) para evaluación.
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
