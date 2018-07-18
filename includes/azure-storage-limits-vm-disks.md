Una máquina virtual de Azure admite la conexión de varios discos de datos. En este artículo, se describen los objetivos de escalabilidad y rendimiento de los discos de datos de la máquina virtual. Use estos destinos para decidir el número y tipo de disco que necesita para satisfacer sus necesidades de capacidad y rendimiento. 

> [!IMPORTANT]
> Para obtener un rendimiento óptimo, limite el número de discos muy usados que se conectan a la máquina virtual para evitar una posible limitación. Si todos los discos conectados no se usan mucho al mismo tiempo, la máquina virtual puede admitir un mayor número de discos.

* **Para Azure Managed Disks:** 

> | Recurso | Límite predeterminado | Límite máximo |
> | --- | --- | --- |
> | Discos administrados estándar | 10.000 | 50.000 |
> | Managed Disks SSD Estándar | 10.000 | 50.000 |
> | Managed Disks Premium | 10.000 | 50.000 |
> | Instantáneas Standard_LRS | 10.000 | 50.000 |
> | Instantáneas Standard_ZRS | 10.000 | 50.000 |
> | Instantáneas Premium_LRS | 10.000 | 50.000 |
> | Imagen administrada | 10.000 | 50.000 |

* **En el caso de cuentas de almacenamiento estándar:** una cuenta de almacenamiento estándar tiene una tasa total máxima de solicitudes de 20 000 E/S por segundo. El número total de E/S por segundo en todos los discos de máquina virtual de una cuenta de almacenamiento estándar no debe superar este límite.
  
    Puede calcular aproximadamente el número de discos muy usados que admite una sola cuenta de almacenamiento estándar en función del límite de tasa de solicitudes. Por ejemplo, en el caso de una máquina virtual de nivel básico, el número máximo de discos muy usados está alrededor de 66 (20 000/300 IOPS por disco) y, en el caso de una máquina virtual de nivel estándar, aproximadamente de 40 (20 000/500 IOPS por disco). 

* **En el caso de cuentas de almacenamiento premium:** una cuenta de almacenamiento premium tiene una capacidad total máxima de proceso de 50 Gbps. La capacidad total de proceso en todos los discos de la máquina virtual no debe superar este límite.

