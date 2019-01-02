---
title: Introducción al control de acceso en Azure Data Lake Storage Gen2 | Microsoft Docs
description: Descripción de cómo funciona el control de acceso en Azure Data Lake Storage Gen2
services: storage
author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.openlocfilehash: 52af1a45f920139ddda1d02734de91372fe4719d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52976598"
---
# <a name="access-control-in-azure-data-lake-storage-gen2"></a>Control de acceso en Azure Data Lake Storage Gen2

Azure Data Lake Storage Gen2 implementa un modelo de control de acceso compatible con el control de acceso basado en rol (RBAC) de Azure y las listas de control de acceso (ACL) tipo POSIX. Este artículo resume los datos básicos del modelo de control de acceso de Data Lake Storage Gen2. 

## <a name="azure-role-based-access-control-rbac"></a>Control de acceso basado en rol (RBAC) de Azure

El control de acceso basado en rol (RBAC) de Azure usa las asignaciones de roles para aplicar de forma eficaz conjuntos de permisos a usuarios, grupos y entidades de servicio para los recursos de Azure. Normalmente, los recursos de Azure están limitados a los recursos de nivel superior (*por ejemplo*, cuentas de Azure Storage). En el caso de Azure Storage y, por tanto, Azure Data Lake Storage Gen2, este mecanismo se ha ampliado al recurso de sistema de archivos.

Aunque el uso de las asignaciones de roles RBAC es un mecanismo eficaz para controlar los permisos de usuario, se trata de un mecanismo mucho más detallado en relación con las ACL. La granularidad más pequeña para RBAC es a nivel de sistema de archivos y esto se evaluará con mayor prioridad que las ACL. Por lo tanto, si asigna permisos de RBAC en un sistema de archivos, ese usuario o entidad de servicio tendrá esa autorización para TODOS los directorios y archivos de ese sistema de archivos, independientemente de las asignaciones de ACL.

Azure Storage proporciona tres roles RBAC integrados para Blob Storage: 

- [Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview)
- [Colaborador de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview)
- [Lector de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview)

Cuando a un usuario o a una entidad de servicio se le conceden permisos de datos RBAC mediante alguno de estos roles integrados o con un rol personalizado, primero se evalúan estos permisos tras la autorización de una solicitud. Si la operación solicitada está autorizada por las asignaciones de RBAC del autor de la llamada, la autorización se resuelve de inmediato y no se realizan más comprobaciones de ACL. Como alternativa, si el autor de la llamada no tiene una asignación de RBAC o la operación de la solicitud no coincide con el permiso asignado, se realizan comprobaciones de ACL para determinar si el autor de la llamada está autorizado para realizar la operación solicitada.

Hay que tener en cuenta especialmente el rol integrado del propietario de datos de blobs de almacenamiento. Si el autor de la llamada tiene esta asignación de RBAC, al usuario se le considera *superusuario* y se le concede acceso total a todas las operaciones de mutación, incluida la configuración del propietario de un directorio o archivo, así como las ACL de archivos y directorios de los que no es propietario. El acceso de superusuario es la única manera autorizada para cambiar el propietario de un recurso.

## <a name="shared-key-and-shared-access-signature-authentication"></a>Autenticación con clave compartida y firma de acceso compartido

Azure Data Lake Storage Gen2 admite los métodos de autenticación con clave compartida y firma de acceso compartido. Una característica de estos métodos de autenticación es que no se asocia ninguna identidad con el autor de la llamada y, en consecuencia, no se puede realizar la autorización basada en permisos de usuario.

En el caso de la clave compartida, el autor de la llamada obtiene acceso de “superusuario” de forma eficaz, lo que conlleva un acceso total a todas las operaciones en todos los recursos, como la configuración del propietario y el cambio de las ACL.

Los tokens de SAS incluyen permisos permitidos como parte del token. Los permisos incluidos en el token de SAS se aplican con eficacia a todas las decisiones de autorización, pero no se realizan comprobaciones de ACL adicionales.

## <a name="access-control-lists-on-files-and-directories"></a>Listas de control de acceso en archivos y directorios

Hay dos tipos de listas de control de acceso (ACL): ACL de acceso y ACL predeterminadas.

* **ACL de acceso**: las ACL de acceso controlan el acceso a un objeto. Tanto archivos como directorios tienen ACL de acceso.

* **ACL predeterminadas**: una plantilla de ACL asociada con un directorio que determina las ACL de acceso de todos los elementos secundarios que se crean en ese directorio. Los archivos no tienen ACL predeterminadas.

Tanto las ACL de acceso como las ACL predeterminadas tienen la misma estructura.

> [!NOTE]
> El cambio de la ACL predeterminada en un elemento primario no afecta a la ACL de acceso o a la ACL predeterminada de los elementos secundarios que ya existen.

## <a name="permissions"></a>Permisos

Los permisos de un objeto del sistema de archivos son de **lectura**, **escritura** y **ejecución**, y se pueden usar en archivos y directorios, como se muestra en la tabla siguiente:

|            |    Archivo     |   Directorio |
|------------|-------------|----------|
| **Lectura (R)** | Puede leer el contenido de un archivo | Requiere permisos de **lectura** y **ejecución** para enumerar el contenido del directorio. |
| **Escritura (W)** | Puede escribir o anexar a un archivo | Requiere permisos de **lectura** y **ejecución** para crear elementos secundarios en un directorio. |
| **Ejecución (X)** | No significa nada en el contexto de Data Lake Storage Gen2 | Se requiere para atravesar los elementos secundarios de un directorio. |

### <a name="short-forms-for-permissions"></a>Formas abreviadas de los permisos

**RWX** se usa para indicar **Lectura + Escritura + Ejecución**. Existe un formato numérico más condensado en el que **Lectura = 4**, **Escritura = 2** y **Ejecución = 1**, y su suma representa los permisos. A continuación se muestran algunos ejemplos.

| Formato numérico | Formato abreviado |      Qué significa     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Lectura + Escritura + Ejecución |
| 5            | `R-X`        | Lectura + ejecución         |
| 4            | `R--`        | Lectura                   |
| 0            | `---`        | Sin permisos         |

### <a name="permissions-inheritance"></a>Herencia de permisos

En el modelo de estilo de POSIX usado por Data Lake Storage Gen2, los permisos de un elemento se almacenan en el propio elemento. En otras palabras, los permisos de un elemento no se pueden heredar de los elementos primarios si los permisos se establecen después de que ya se haya creado el elemento secundario. Los permisos solo se heredan si los permisos predeterminados se han establecido en los elementos primarios antes de crear los secundarios.

## <a name="common-scenarios-related-to-permissions"></a>Escenarios comunes relacionados con los permisos

En la tabla siguiente se enumeran algunos escenarios comunes para ayudarle a entender qué permisos se necesitan para realizar determinadas operaciones en una cuenta de Data Lake Storage Gen2.

|    Operación             |    /    | Oregón/ | Portland/ | Data.txt     |
|--------------------------|---------|----------|-----------|--------------|
| Leer Data.txt            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Anexar a Data.txt       |   `--X`   |   `--X`    |  `--X`      | `RW-`          |
| Eliminar Data.txt          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Crear Data.txt          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Enumerar /                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Enumerar /Oregón/           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Enumerar /Oregón/Portland/  |   `--X`   |   `--X`    |  `R-X`      | `---`          |


> [!NOTE]
> Para eliminar un archivo no es preciso tener permisos de escritura en él, siempre y cuando se cumplan las dos condiciones anteriores.
>
>

## <a name="users-and-identities"></a>Usuarios e identidades

Todos los archivos y directorios tienen permisos distintos para estas identidades:

- El usuario propietario
- El grupo propietario
- Usuarios designados
- Grupos designados
- Entidades de servicio designadas
- Los restantes usuarios

Las identidades de usuarios y grupos son las identidades de Azure Active Directory (Azure AD). Por tanto, a menos que se indique lo contrario, un *usuario*, en el contexto de Data Lake Storage Gen2, puede referirse a un usuario, a una entidad de servicio o a un grupo de seguridad de Azure AD.

### <a name="the-owning-user"></a>El usuario propietario

El usuario que creó el elemento es automáticamente el usuario propietario del elemento. Un usuario propietario puede:

* Cambiar los permisos de un archivo que le pertenece.
* Cambiar el grupo propietario de un archivo que le pertenece, siempre que el usuario propietario también sea miembro del grupo de destino.

> [!NOTE]
> El usuario propietario *no puede* cambiar el usuario propietario de un archivo o directorio. Solo los superusuarios pueden cambiar el usuario propietario de un archivo o directorio.

### <a name="the-owning-group"></a>El grupo propietario

En las ACL de POSIX, todos los usuarios están asociados a un *grupo principal*. Por ejemplo, el usuario "alice" puede pertenecer al grupo "finance". Alice puede pertenecer a varios grupos, pero uno de ellos siempre se designa como su grupo principal. En POSIX, cuando Alice crea un archivo, el grupo propietario de dicho archivo se establece como su grupo principal, que en este caso es "finance". De lo contrario, el grupo propietario se comporta de forma similar a los permisos asignados para otros usuarios o grupos.

#### <a name="assigning-the-owning-group-for-a-new-file-or-directory"></a>Asignar el grupo propietario de un nuevo archivo o directorio

* **Caso 1**: El directorio raíz "/". Este directorio se crea cuando se crea un sistema de archivos de Data Lake Storage Gen2. En este caso, el grupo propietario se establece en el usuario que creó el sistema de archivos, en caso de que se haya realizado con OAuth. Si el sistema de archivos se crea con una clave compartida, una SAS de cuenta o una SAS de servicio, el propietario y el grupo propietario se establecen en **$superuser**.
* **Caso 2** (cada dos casos): cuando se crea un nuevo elemento, se copia el grupo propietario del directorio primario.

#### <a name="changing-the-owning-group"></a>Cambiar el grupo propietario

El grupo propietario se puede cambiar por:
* Cualquier superusuario.
* El usuario propietario, si el usuario propietario también es miembro del grupo de destino.

> [!NOTE]
> El grupo propietario no puede cambiar las ACL de un archivo o directorio.  Aunque el grupo propietario se establece en el usuario que creó la cuenta en el caso del directorio raíz, **Caso 1** anterior, una única cuenta de usuario no es válida para proporcionar los permisos mediante el grupo propietario. Puede asignar este permiso a un grupo de usuarios válidos si es aplicable.

## <a name="access-check-algorithm"></a>Algoritmo de comprobación de acceso

El siguiente seudocódigo representa el algoritmo de comprobación de acceso de las cuentas de Data Lake Storage Gen2.

```
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or directory
  # Note: the "sticky bit" is not illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

# Handle the owning user. Note that mask IS NOT used.
entry = get_acl_entry( path, OWNER )
if (user == entry.identity)
    return ( (desired_perms & entry.permissions) == desired_perms )

# Handle the named users. Note that mask IS used.
entries = get_acl_entries( path, NAMED_USER )
for entry in entries:
    if (user == entry.identity ) :
        mask = get_mask( path )
        return ( (desired_perms & entry.permissions & mask) == desired_perms)

# Handle named groups and owning group
member_count = 0
perms = 0
entries = get_acl_entries( path, NAMED_GROUP | OWNING_GROUP )
for entry in entries:
if (user_is_member_of_group(user, entry.identity)) :
    member_count += 1
    perms | =  entry.permissions
if (member_count>0) :
return ((desired_perms & perms & mask ) == desired_perms)

# Handle other
perms = get_perms_for_other(path)
mask = get_mask( path )
return ( (desired_perms & perms & mask ) == desired_perms)
```

### <a name="the-mask"></a>La máscara

Como se muestra en el algoritmo de comprobación de acceso, la máscara limita el acceso a usuarios con nombre, el grupo propietario y grupos con nombre.  

> [!NOTE]
> Para un nuevo sistema de archivos de Data Lake Storage Gen2, el valor predeterminado de la máscara para la ACL de acceso del directorio raíz ("/") es 750 para los directorios y 640 para los archivos. Los archivos no reciben el bit X, ya que no es pertinente para los archivos de un sistema de solo almacenamiento.
>
> La máscara se puede especificar por cada llamada. Esto permite que diferentes sistemas de consumo, como los clústeres, tengan distintas máscaras eficaces para sus operaciones de archivo. Si se especifica una máscara en una solicitud dada, reemplaza completamente la máscara predeterminada.

### <a name="the-sticky-bit"></a>El bit persistente

El bit persistente es una característica más avanzada de un sistema de archivos POSIX. En el contexto de Data Lake Storage Gen2, es improbable que se necesite el bit persistente. En resumen, si el bit persistente está habilitado en un directorio, solo el usuario propietario del elemento secundario puede eliminarlo o cambiarle el nombre.

El bit persistente no se muestra en Azure Portal.

## <a name="default-permissions-on-new-files-and-directories"></a>Permisos predeterminados en archivos y directorios nuevos

Cuando se crea un archivo o directorio en un directorio existente, la ACL predeterminada del directorio principal determina:

- Una ACL de acceso y una ACL predeterminada del directorio secundario
- Una ACL de acceso de un archivo secundario (los archivos no tienen ninguna ACL predeterminada).

### <a name="umask"></a>umask

Al crear un archivo o directorio, se usa umask para modificar cómo se establecen las ACL predeterminadas en el elemento secundario. umask es un valor de 9 bits en los directorios principales que contiene un valor RWX para **usuario propietario**, **grupo propietario** y **otro**.

El valor de umask para Azure Data Lake Storage Gen2 es una constante establecida en 007. Este valor se convierte en:

| componente umask     | Formato numérico | Formato abreviado | Significado |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | En el caso del usuario propietario, copie la ACL predeterminada del elemento principal en la ACL de acceso del elemento secundario. | 
| umask.owning_group  |    0         |   `---`      | En el caso del grupo propietario, copie la ACL predeterminada del elemento principal en la ACL de acceso del elemento secundario. | 
| umask.other         |    7         |   `RWX`      | En el caso de otro, quite todos los permisos en la ACL de acceso del elemento secundario. |

El valor de umask usado por Azure Data Lake Storage Gen2 significa realmente que el valor de **otro** nunca se transmite de forma predeterminada en los nuevos objetos secundarios, independientemente de lo que indique la ACL predeterminada. 

El siguiente pseudocódigo muestra cómo se aplica umask al crear las ACL de un elemento secundario.

```
def set_default_acls_for_new_child(parent, child):
    child.acls = []
    for entry in parent.acls :
        new_entry = None
        if (entry.type == OWNING_USER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_user))
        elif (entry.type == OWNING_GROUP) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_group))
        elif (entry.type == OTHER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.other))
        else :
            new_entry = entry.clone(perms = entry.perms )
        child_acls.add( new_entry )
```

## <a name="common-questions-about-acls-in-data-lake-storage-gen2"></a>Preguntas frecuentes acerca de las ACL en Data Lake Storage Gen2

### <a name="do-i-have-to-enable-support-for-acls"></a>¿Es preciso habilitar la compatibilidad con las ACL?

 No. El control de acceso mediante ACL se habilita para una cuenta de Data Lake Storage Gen2 siempre que el espacio de nombres jerárquico esté ACTIVADO.

Si el espacio de nombres jerárquico está DESACTIVADO, las reglas de autorización de RBAC siguen siendo aplicables.

### <a name="what-is-the-best-way-to-apply-acls"></a>¿Cuál es la mejor manera de aplicar las ACL?

Utilice siempre grupos de seguridad de Azure AD como la entidad asignada en las ACL. Ofrece la oportunidad de asignar directamente usuarios individuales o entidades de servicio. Con esta estructura, podrá agregar y quitar usuarios o entidades de servicio sin necesidad de volver a aplicar las ACL en una estructura de directorios completa. En su lugar, basta con agregarlos o quitarlos en el grupo de seguridad de Azure AD apropiado. Tenga en cuenta que las ACL no se heredan y, por ello, para volver a aplicar las ACL es necesario actualizarlas en cada archivo y subdirectorio. 

### <a name="which-permissions-are-required-to-recursively-delete-a-directory-and-its-contents"></a>¿Qué permisos son necesarios para eliminar de forma recursiva un directorio y su contenido?

- El autor de la llamada tiene los permisos de "superusuario",

o

- El directorio principal debe tener permisos de escritura y ejecución.
- Tanto el directorio que se va a eliminar como todos los directorios que contiene requieren permisos de lectura, escritura y ejecución.

> [!NOTE]
> No necesita permisos de escritura para eliminar archivos en directorios. Además, el directorio raíz "/" nunca se puede eliminar.

### <a name="who-is-the-owner-of-a-file-or-directory"></a>¿Quién es el propietario de un archivo o directorio?

El creador de un archivo o directorio se convierte en el propietario. En el caso del directorio raíz, esta es la identidad del usuario que creó el sistema de archivos.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-directory-at-creation"></a>¿Qué grupo se establece como grupo propietario de un archivo o directorio al crearlo?

El grupo propietario se copia del directorio primario en la que se crea el archivo o directorio.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Soy el usuario propietario de un archivo, pero no tengo los permisos de RWX que necesito. ¿Qué puedo hacer?

El usuario propietario puede cambiar los permisos del archivo y concederse los permisos de RWX que necesite.

### <a name="why-do-i-sometimes-see-guids-in-acls"></a>¿Por qué a veces veo GUID en ACL?

Un GUID se muestra si la entrada representa un usuario y este ya no existe en Azure AD. Normalmente esto sucede cuando el usuario ha dejado la empresa o si se ha eliminado su cuenta de Azure AD. Además, las entidades de servicio y los grupos de seguridad no tienen un nombre principal de usuario (UPN) para identificarlos y, por tanto, se representan mediante su atributo OID (un GUID). 

### <a name="does-data-lake-storage-gen2-support-inheritance-of-acls"></a>¿Admite Data Lake Storage Gen2 la herencia de ACL?

Las asignaciones de RBAC de Azure se heredan. Las asignaciones fluyen desde las suscripciones, grupos de recursos y recursos de cuentas de almacenamiento hasta el recurso del sistema de archivos.

Las ACL no se heredan. Sin embargo, las ACL predeterminadas pueden usarse para establecer ACL para subdirectorios y archivos secundarios creados en el directorio principal. 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>¿Dónde puedo obtener más información sobre el modelo de control de acceso POSIX?

* [POSIX Access Control Lists on Linux](https://www.linux.com/news/posix-acls-linux) (Listas de control de acceso de POSIX en Linux)
* [HDFS Permission Guide](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) (Guía de permisos de HDFS)
* [POSIX FAQ (PREGUNTAS MÁS FRECUENTES SOBRE POSIX)](http://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)
* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)
* [ACL de POSIX en Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs)
* [ACL: Using Access Control Lists on Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/) (ACL: uso de listas de control de acceso en Linux)

## <a name="see-also"></a>Otras referencias

* [Introducción a Azure Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md)
