

---

## 🐚 Instalacion de debian servers

Cuando instalamos el SO desde la BIOS del sistema hay varias opciones para hacerlo. Según cuál seleccionamos, la configuración del SO será más específica o menos en la fase de instalación. Por norma general, las cosas que hay que configurar en esta fase, independientemente del tipo de instalación, son:

1. **Localización:** Elige tu idioma, ubicación y distribución del teclado para que el sistema esté correctamente configurado en español.
2. **Red:** Asigna un nombre a tu equipo (hostname). Si estás en una red doméstica, puedes dejar el nombre de dominio en blanco.
3. **Cuentas de Usuario:**
	- Configura la contraseña de `root` (administrador).
	- Crea un usuario personal y su contraseña.
4. **Particionado de Disco:**
	- **Guiado** (recomendado):** El asistente de Debian se encargará de crear las particiones necesarias automáticamente. Es la opción más sencilla y permite opciones como LVM, cifrado, RAID y normal sin complicaciones.
	- **Manual:** Si deseas separar carpetas como `/home` (para tus archivos personales) o `/var` (común en servidores).
5. **Selección de Software:** Debian te preguntará qué escritorio deseas (GNOME, KDE Plasma, XFCE, etc.). Si es para un servidor, selecciona solo "SSH Server" y "Standard system utilities".
6. **Gestor de Arranque (GRUB):** Confirma la instalación de GRUB en tu unidad principal para que el sistema pueda iniciar correctamente.


## Tipos de instalación de debian server
![Instalación Debian](assets/instacion-manual-debian-server/image_00.png)

## 🔴 Instalación manual de Debian server

Guia paso a paso con capturas para la instalacion manual desde el menu del instalador.

### 🧭 Vista general del menu de instalación
![](assets/instacion-manual-debian-server/image_01.png)

### 📩 Ver pasos desde el menu de instalación
![](assets/instacion-manual-debian-server/image_02.png)

### 1. Idioma y localizacion 🗣️🌎
- **Eleccion de idioma para el sistema:** Selecciona el idioma principal.
![](assets/instacion-manual-debian-server/image_03.png)
- **Eleccion de localizacion geografica:** Define el pais o region.
![](assets/instacion-manual-debian-server/image_04.png)

### 2. Teclado ⌨️
![](assets/instacion-manual-debian-server/image_05.png)

### 3. Red 🌐
Por defecto el instalador detecta interfaces, configura IPv6 y puede usar DHCP.
- **Nombre de la maquina (hostname):** Se guarda en `/etc/hostname`.
![](assets/instacion-manual-debian-server/image_06.png)
- **Dominio:** Se puede ver con `hostname -d` o `domainname` y en `/etc/hosts`.
![](assets/instacion-manual-debian-server/image_07.png)

### 4. Usuarios 👮🏻🫂
- **Contraseña de `root`:** Define la clave del superusuario.
![](assets/instacion-manual-debian-server/image_08.png)
- **Crear usuario sin privilegios:** Usuario normal para el dia a dia.
![](assets/instacion-manual-debian-server/image_09.png)
- **Contraseña del usuario normal:** Confirma la clave del usuario.
![](assets/instacion-manual-debian-server/image_10.png)

### 5. Zona horaria ⏱️
![](assets/instacion-manual-debian-server/image_11.png)

### 6. Detección de discos
Proceso automático; en otros modos se puede ajustar manualmente.

### 7. Particionado de discos 💿💔
- **Seleccion del tipo de particionado:** Define el modo de particionado.
![](assets/instacion-manual-debian-server/image_12.png)

#### 7.1 Particionado guiado
Es la opción "automática" y la más recomendada para la mayoría de escenarios.


> ⚠️ **Critico en servidores de produccion:** En producción se debe usar **LVM (Logical Volume Manager)** porque permite crear grupos de volúmenes dinámicos. Esto es esencial porque los servidores necesitan flexibilidad para crecer: puedes asignar más memoria a una partición cuando se llena sin reiniciar el servicio, conectar discos nuevos y asignar su espacio a las particiones existentes sin interrupción, o quitar memoria de una partición que no esté llena para dársela a otra. Es buena práctica en servidores con alto nivel de carga. Además, existe la opción de usar **LVM cifrado con LUKS** para mayor seguridad: proporciona cifrado a nivel de disco, protegiendo todo el almacenamiento criptográficamente. Sin la contraseña, los datos son inaccesibles aunque alguien tenga acceso físico al disco. Se usa en laptops corporativos y servidores con datos sensibles.

> ℹ️ **Mas información sobre LVM:** Puedes leer más detalles técnicos sobre LVM en:
> [LVM - Logical Volume Manager](https://www.notion.so/LVM-330b80eb3d88803da2b6ffd1d7b1c361)

> ℹ️ **Opciones de particionado guiado:** Información concreta sobre las distintas opciones de esta fase:
> [Configuraciones/modos de particionado de disco](https://www.notion.so/Esquema-de-particionados-guiados-de-Disco-segun-utilidad-y-complejidad-DEBIAN-330b80eb3d888058a98bd7c6959ce581)

- **Elegir tipo de particionado guiado:** Selecciona el asistente deseado.
![](assets/instacion-manual-debian-server/image_13.png)
- **Seleccionar disco de destino:** El disco donde se aplicaran los cambios.
![](assets/instacion-manual-debian-server/image_14.png)

#### 7.2 Esquema de particiones
> ⚠️ **Critico en servidores:** Aqui decides como se particionan los discos logicos. En produccion esto es clave porque los servicios generan logs y datos en carpetas concretas (por ejemplo `/var`). Los logs son registros tecnicos de eventos, errores, auditoria y rendimiento que el servicio y el sistema escriben de forma continua. En produccion los servidores ejecutan funciones de forma iterativa y nos apoyamos en logs y herramientas de observabilidad para monitorizar el comportamiento. Al ser codigo iterativo, el uso de memoria y almacenamiento puede descontrolarse cuando el servidor escala por las peticiones de usuarios u otros factores. Si no separas particiones, un crecimiento de logs puede llenar el disco completo y tumbar el sistema. Con particiones separadas, solo se satura la particion de logs y el servicio puede seguir funcionando. El objetivo es que los limites del sistema no sean el cuello de botella y, si lo son, que el impacto quede aislado y minimo para los usuarios.

> ℹ️ **Guia de esquemas:** Puedes ver los tipos de particiones y sus usos en esta guia de esquemas para servidores:
> [Configuraciones de particiones separadas para servidores](https://www.notion.so/Configuraciones-de-particiones-separadas-para-servidores-330b80eb3d88805a966ff666dca53d01)
- **Elegir el esquema de particiones:** En todos los tipos de particionado guiado hay que pasar por esta sección para elegir el esquema de particiones del disco duro.
![](assets/instacion-manual-debian-server/image_15.png)
- **Confirmar el esquema:** Se escribe la tabla de particiones en el disco.
![](assets/instacion-manual-debian-server/image_16.png)

#### 7.3 LUKS (si aplica)
- **Contrasena de cifrado:** En LUKS hay que elegir la contrasena y confirmarla. Si es muy poco segura, se debe volver a poner.
![](assets/instacion-manual-debian-server/image_17.png)

#### 7.4 LVM (si aplica)
- **Parte del disco reservada:** Se recomienda que sea menor que el valor por defecto para que LVM pueda ir añadiendo el disco dinamicamente.
![](assets/instacion-manual-debian-server/image_18.png)

#### 7.5 Revisar y aplicar cambios
- **Resumen de particiones:** Pantalla con las particiones creadas y la opcion de seguir configurando (RAID/LVM) o volver al modo manual. Aqui se confirma el resultado del asistente antes de aplicar cambios.
![](assets/instacion-manual-debian-server/image_19.png)
- **Confirmar escritura de particiones:** Se aplican los cambios al disco.
![](assets/instacion-manual-debian-server/image_20.png)

#### 7.6 iSCSI (discos en red)
Opción avanzada de red.
- **Uso:** Permite conectar el sistema a discos remotos expuestos vía red.
- **Motivo:** En servidores de alto rendimiento, el sistema operativo puede residir en una cabina de almacenamiento externa.
- **Cuando conviene:** Solo en entornos empresariales o servidores muy específicos. Para una instalacion normal, se puede ignorar.
- **Entorno:** Centros de datos con redes SAN (Storage Area Network).
- **Ventaja profesional:** Si un servidor fisico falla, el disco virtual vía iSCSI puede conectarse a otro servidor en segundos sin perder datos, clave para alta disponibilidad.

#### 7.7 Particionado manual (SCSI / sda)
- **Para que sirve:** Entrar al particionado manual para crear particiones a medida (por ejemplo, una para el sistema y otra para `/home`).
- **Motivo:** Separa el sistema en "cajones" fisicos. Ejemplos:
	- `/var`: separar logs para que no llenen el disco principal.
	- `/home`: mantener datos de usuarios intactos al actualizar.
	- `swap`: calcular según RAM para picos de carga.
- **Cuando conviene:** Usuarios avanzados o si se quiere conservar datos de otro sistema operativo.
- **Entorno:** Servidores web, correo o sistemas criticos expuestos a Internet.

### 8. Gestor de paquetes y descarga de imagen 💽⚙️
- **Anadir medio USB adicional:** No recomendado salvo necesidad.
![](assets/instacion-manual-debian-server/image_21.png)
- **Seleccionar region para mirrors de `apt`:** Elige el pais o zona.
![](assets/instacion-manual-debian-server/image_22.png)
- **Elegir servidor espejo en la region:** Selecciona el mirror mas cercano.
![](assets/instacion-manual-debian-server/image_23.png)
- **Proxy (opcional):** Permite enrutar la conexion al dominio a traves del proxy por razones de seguridad. Se usa mucho en empresas cuando el hostname, el dominio y la red ya estan configurados. Dejar en blanco si no se usa.
![](assets/instacion-manual-debian-server/image_24.png)

### 9. Selección de paquetes 📦⬇️
**Seleccion de paquetes:** Para marcar o desmarcar usa la tecla espacio. Segun los paquetes que se descarguen, el sistema tendra unas configuraciones u otras. Todo lo que no se descargue aqui se puede instalar luego desde el sistema.
[Paquetes apt en instalacion de debian](https://www.notion.so/330b80eb3d8880d7a400f72a366a3d07)
![](assets/instacion-manual-debian-server/image_25.png)

### 10. Instalación de sistema
![](assets/instacion-manual-debian-server/image_26.png)

### 11. GRUB 🧩
- **Instalar GRUB:** Importante para gestionar el arranque en VMs o sistemas fisicos. Se puede configurar despues desde el sistema.
![](assets/instacion-manual-debian-server/image_27.png)
- **Seleccionar disco para el arranque:** Define el disco donde se instala el cargador.
![](assets/instacion-manual-debian-server/image_28.png)

### 12. Instalación finalizada 🥳🥳🥳
![](assets/instacion-manual-debian-server/image_29.png)
![](assets/instacion-manual-debian-server/image_30.png)

---

## 🦝 Máquinas Virtuales (VMs)


Un hipervisor es una capa de abstracción que virtualiza los recursos del hardware (CPU, memoria, almacenamiento, dispositivos de red) del host, permitiendo crear máquinas virtuales independientes con sus propios kernels y sistemas operativos, aisladas entre sí a nivel de software.

### Usos principales

- **Desarrollo:** Crear entornos aislados (**hardering**) que nos permitan testear software en distintos SO. También para probar software de terceros de manera segura mediante **sandboxing**, de forma que si algo pasa no se contamina nuestro ordenador.

- **Producción:** En entornos de producción, los IAAS y los servicios de hosting usan máquinas virtuales para alojar muchos servidores distintos sin riesgo de que el servidor principal que las aloja sufra daños. Además, gracias a la escalabilidad y flexibilidad, se pueden **asignar recursos de manera dinámica** (infraestructura) y **automatizar despliegues** (DevOps automation).

- **Servicios públicos:** Exponer servicios al público con riesgo aislado.

- **Laboratorio y múltiples SO:** Creación de laboratorios de red o simplemente para tener múltiples sistemas operativos dentro de un ordenador sin necesidad de hacer particiones de disco duro.

> ℹ️ **Más información sobre VMs e hipervisores:** Si quieres profundizar en los usos específicos, tecnologías de hipervisores de escritorio y servicios remotos, puedes consultar:
> [¿Qué es una VM, el hipervisor y principales tecnologías?](https://www.notion.so/Que-es-una-Vm-el-hipervisor-y-proncipales-tecnologias-357b80eb3d8880948fafdfbbbd22fe1d)

## 🖥️ Creación y configuración de la VM desde el hipervisor

### Crear VMs a través del hipervisor (manual)

A la hora de crear la máquina virtual de VirtualBox desde la interfaz del programa tenemos varias opciones para hacerlo. Lo primero es seleccionar la opción de crear máquina. Una vez seleccionada se abrirá una nueva pestaña donde podremos elegir:

### 1. Configurar nombre de la máquina, localización en el host e ISO

- **Nombre de la máquina**
- **El directorio del sistema** donde se encontrará alojada
- **La ISO del sistema operativo** que va a usar la máquina (tendremos que tenerla descargada en nuestro sistema)

![Configuración: Nombre, Localización e ISO](assets/vms-creation-config/vm_01_name_location_iso.png)

### 2. Configurar especificaciones de arquitectura de sistema operativo

> ⚠️ Estos campos solo hay que configurarlos manualmente si se elige la instalación manual (desatendida)

- **Elección de sistema operativo (OS)**
- **Elección de distribución OS**
- **Elección de versión de sistema (arquitectura)**

![Especificaciones de Arquitectura OS](assets/vms-creation-config/vm_02_arch_os.png)

### 3. Configurar especificaciones de hardware virtualizado para la máquina

> ⚠️ Estas configuraciones hay que configurarlas independientemente del tipo de instalación (atendida o desatendida)

Son las configuraciones de los núcleos de CPU, la RAM y la memoria en el disco que va a asignar el hipervisor de nuestro sistema a la máquina cuando esté encendida (funcionando):

![Configuración de CPU y RAM](assets/vms-creation-config/vm_03_cpu_ram.png)

![Configuración de Disco](assets/vms-creation-config/vm_04_disk.png)

---
## Tipos de instalación manual

### 1. Instalación desatendida

Tendremos que configurar varias cosas desde el hipervisor. La instalación manual se tendría que hacer en el proceso de instalación del sistema:
- **Nombre de usuario principal**
- **Contraseña de usuario**
- **Claves de sistema operativo** (para Windows)

![Configuración: Instalación Desatendida](assets/vms-creation-config/vm_05_unattended_install.png)

Por otra parte, el resto de la instalación y configuración del sistema se hace automáticamente. Le damos a "finish" y comienza: descarga de paquetes, elección de software, etc. Cuando acabe la instalación solo tendremos que loguearnos y ya estaremos dentro de un sistema operativo preconfigurado.

### 2. Instalación manual

Después de los pasos previos, le damos a "finish" y comienza la instalación del sistema operativo manualmente. Según el proceso de instalación de cada sistema se hará de una manera u otra. Este modo nos da mucho más manejo sobre cómo queremos que esté instalado nuestro sistema.

---

## 🌐​ Configuración de red de máquinas virtuales desde el hipervisor

Cuando creas una maquina virtual desde el hipervisor puedes elegir que modo de Red quieres usar para concestar las Vms con la Red. Segun el modo de de seleccionado el comportamiento interno de la Red, el alcanace, la forma de conctar con el host (servidor) y con elxterior que est ajecutando el hipervisor es distinta y cada uno esta destinado a un uso diferente. Por defecto las maquinas virtuales cuando se crean desde el hipervisor manualmete se crean en modo NAT.

<img src="assets/vms-net-config/Net_types.png" alt="Configuración de red: Modo NAT" width="100%" height="50%">

---

### Modo NAT (Network Address Translation)

El hipervisor gestiona su propia red interna a través de sus **tablas de NAT internas** y sus **tablas de PAT** (Port Address Translation). Además, actúa como **servidor DHCP**, asignando dinámicamente direcciones IP a las máquinas virtuales y proporcionando la puerta de enlace (gateway) por defecto para poder conectarse a redes fuera de su LAN

> ⚠️ El trafico de red siempre sale con la IP del host. Por lo que no la maquina virtual no es visibles desde el host ni desde fuera del host.

> ⚠️ **No se recomienda este modo para servidores en produccion:** Si quieres crear un servidor en la VM, como no es visible desde fuera (comparte IP con el host), debes configurar **port forwarding en el hipervisor** para que el host o clientes externos puedan conectarse a la VM, redirigiendo un endpoint del host (IP + puerto) a un endpoint de la VM (IP + puerto). Por lo que publicamente tendrias que usar la direccion del host para que los clientes se conecten a tu servidor, por eso no se recomienda este modo para servidores en producción.
 

<details>
<summary><strong>Explicación del modo NAT y port forwarding</strong></summary>


El hipervisor gestiona su propia red interna a través de sus **tablas de NAT internas** y sus **tablas de PAT** (Port Address Translation). Además, actúa como **servidor DHCP**, asignando dinámicamente direcciones IP a las máquinas virtuales y proporcionando la puerta de enlace (gateway) por defecto para poder conectarse a redes fuera de su LAN.

<img src="assets/vms-net-config/DHCP.png" alt="Configuración de red: Modo NAT" width="50%" height="50%">

Por defecto, todo el tráfico que sale de la VM hacia Internet **sale con la IP del host** (servidor) y el puerto que el hipervisor reserva dentro del host para abrir un socket y comunicarse con el exterior. Esto se hace gracias al **NAT interno** del hipervisor que, cuando recibe una respuesta de Internet, traduce la IP del host y el puerto del host al puerto y la IP de la VM que hizo la petición. Las entradas de la tabla de traducción NAT son efímeras y solo sirven para comunicarse como cliente con el exterior, no para exponer servicios al exterior.

La manera de poder exponer servicios al exterior con este modo de red es a través de la **configuración de reglas de PAT** (Port Address Translation) en el hipervisor. Como la VM no tiene IP visible desde el host ni desde fuera del host, no podemos generar endpoints para que los clientes de un servidor alojado en ella se conecten. Con estas reglas lo solucionamos asociando un puerto endpoint del host (IP + puerto) a un endpoint de la VM (IP + puerto). Con esto podemos redirigir y controlar quién puede conectarse con la VM, actuando también como firewall para controlar el acceso a los servicios alojados en la VM.

<img src="assets/vms-net-config/protfeorwarding_hypervisor.png" alt="Configuración de red: Modo NAT" height="50%" width="50%%">

> ℹ️ **Más información sobre modo NAT en hipervisores:** Si quieres profundizar en el funcionamiento de estas tecnologías en el contexto de los hipervisores, puedes consultar:
> [NAT, PAT y DHCP en hipervisores](https://broken-snowdrop-f03.notion.site/Modo-NAT-y-Port-forwarding-32db80eb3d8880dc922fea99a4f7a1e0)

</details>

---

### Modo Bridge (Adaptador Puente)


En modo Bridge, la VM tiene entidad propia de red (IP y MAC). Esto permite que sea visible desde fuera del host y desde el propio host como un dispositivo independiente.

> ⚠️ **Uso para servidores en producción**: Si quieres crear un servidor en la VM, este modo es el recomendado porque la VM es visible desde fuera y no necesitas configurar port forwarding para que los clientes se conecten a tu servidor. Al tener un endpoint propio es visible y accesible por otros dispositivos de la red local y de Internet, lo que es ideal para servidores en producción. Además, el router puede asignar direcciones IP dinámicamente a la VM mediante DHCP.

> ⚠️ **Necesita de medidas de seguridad perimetrales fuera del host** :(firewalls, IDS/IPS, etc.) para proteger la VM, ya que al ser visible en la red, es susceptible a ataques externos. Debido a que lo paquetes no pasan por la pila de red del host y este no puede controlar el trafico por lo que es vulnerable a ataques externos.VM.


<details>
<summary><strong>Explicacion de modo Bridge</strong></summary>

En modo Bridge, la VM tiene entidad propia de red y de dirección MAC. Esto permite que sea visible desde fuera del host y desde el propio host como un dispositivo independiente. El hipervisor crea una interfaz virtual en modo bridge que se incrusta en la tarjeta de red (NIC) del host, permitiendo que reciba tramas dirigidas a su propia MAC, además de la del host. Así, los paquetes de red no tienen que ser desempaquetados por la pila de red del host ni redirigidos mediante port forwarding.

**Esto tiene ventajas e inconvenientes:**
 - La **ventaja** es que, al ser visible como un dispositivo independiente, la VM puede comunicarse directamente con otros dispositivos en la red local y con Internet sin necesidad de configurar reglas de port forwarding y que el tráfico no tenga que pasar por la pila de red del host y se enrute por la MAC de la VMS **disminuye la latencia**. Esto es importante porque ciertos protocolos de comunicación, como VoIP o protocolos de descubrimiento, no funcionan bien con la latencia añadida por NAT o port forwarding.  Ademas se puede configurar la VM para que reciba una dirección **IP dinámica del router mediante DHCP**.

- El **inconveniente** es que, al estar en modo bridge, el tráfico se enruta directamente desde la MAC de la VM sin pasar por la pila de red del host, por lo que n**o se le pueden aplicar reglas de firewall desde el host**. Esto implica que la máquina queda totalmente expuesta a Internet si se crea un servicio, sin ningún tipo de protección, lo que la hace **vulnerable**.

> ℹ️ **Más información sobre modo Bridge en hipervisores:** Si quieres profundizar en el funcionamiento, ventajas, riesgos y configuraciones avanzadas del modo Bridge, consulta:  
> [Modo Bridge en hipervisores](https://broken-snowdrop-f03.notion.site/Modo-Bridge-32db80eb3d8880799d97db06c5ca2eb8?pvs=74)


</details>

<details>
<summary><strong>Explicacion breve de aqrquitecturas con modo Bridge</strong></summary>

Como hemos dicho antes el modo bridge tiene la ventaja de que la VM es visible desde fuera del host y no necesita reglas de port forwarding para exponer servicios al exterior, pero tiene la desventaja de que el tráfico no pasa por la pila de red del host y este no puede controlar el tráfico, lo que hace a la VM vulnerable a ataques externos.


Por ellos simpre se tiene que configurar con **memedias de seguridasd perimetrales** fuera del host **(firewalls, IDS/IPS, etc.)**. Se suele pasar el tráfico antes por **una DMZ (zona desmilitarizada)**, suele ser un servidor o maquina que hace de router con reglas de firewall para controlar el acceso a la VM y protegerla de ataques externos.

En infraestructuras de **IaaS (cloud), hosting y servidores de alta disponibilidad** se suele usar este modo para alojar los servidores en producción. Además del servidor con el enrutamiento, se suele aprovechar y **se acompaña de VLANs** para segmentar la red y aislar los servidores entre sí, de **balanceadores de carga** para distribuir el tráfico entre varios servidores y mejorar la disponibilidad y el rendimiento Y de mecanismos de **micro-segmentación** para controlar el tráfico entre servidores y limitar el alcance de posibles ataques internos.

> 💻 **Repositorio con explicaciones y referencias sobre este modo:**  
> [Inception - Ejemplo de infraestructura y automatización con VMs (GitHub)](https://github.com/alcarril/Inception)



</details>


</details>

<details>
<summary><strong>Extra modo promiscuo en hipervisores (IDS/IPS, sniffer, Monitorización)</strong></summary>

Normalmente, una tarjeta de red es "educada": si llega un paquete cuya dirección MAC de destino no es la suya, lo descarta inmediatamente a nivel de hardware para no molestar a la CPU. Al activar el modo promiscuo, le dices a la tarjeta: "Pásame absolutamente todo lo que pase por el cable (o el switch virtual), sea para mí o no". Esto es útil para:

- **IDS/IPS (Intrusion Detection/Prevention Systems):** Para detectar ataques o comportamientos sospechosos en la red, el IDS necesita ver todo el tráfico. En modo promiscuo, puede analizar cada paquete que pasa por la red, incluso si no está dirigido a la VM, lo que es esencial para detectar amenazas.
- **Sniffer de red:** Herramientas como Wireshark o tcpdump necesitan el modo promiscuo para capturar todo el tráfico de la red y analizarlo, lo que es crucial para troubleshooting, análisis forense o auditorías de seguridad.
- **Monitorización de red:** Para monitorizar el rendimiento, detectar cuellos de botella o analizar patrones de tráfico, el modo promiscuo permite a la VM recoger datos de toda la red, no solo de su propio tráfico, lo que proporciona una visión completa del entorno de red.


> ℹ️ **Más información sobre configuración de modo promiscuo en hipervisores:** Consulta detalles técnicos, ejemplos y usos avanzados aquí:  
> [Configuración de modo promiscuo en hipervisores](https://broken-snowdrop-f03.notion.site/Configuracion-de-modo-promiscuo-357b80eb3d888080a57fe84461f70488?pvs=74)



</details>

---

### Modo Host-Only


En modo Host-Only, la VM solo puede comunicarse con el host y con otras VMs en la misma red Host-Only. No tiene salida directa a Internet (salvo que se combine con un segundo adaptador de red).

> ⚠️ **Solo para pruebas cerradas y entornos controlados (sandbox):** Este modo es ideal para entornos de pruebas cerrados, análisis de malware en sandbox controlado o laboratorios donde no se necesita conexión a Internet. Al no tener acceso a la red externa, las VMs están completamente aisladas del mundo exterior, lo que las hace seguras para probar software potencialmente peligroso sin riesgo de comprometer el host o la red real.

<details>
<summary><strong>Como funciona el modo Host-Only</strong></summary>

El hipervisor crea una interfaz de red virtual (por ejemplo, ***vboxnet0*** en VirtualBox o ***VMnet1*** en VMware) que solo conecta el host y las VMs configuradas en modo Host-Only. Esta interfaz tiene su propia subred y actúa como un switch virtual aislado. Las VMs pueden comunicarse entre sí y con el host, pero no pueden acceder a Internet ni a la red física del host.

**Como el hipervisor para aislar la interfaz de red de internet**
- No hay Gateway configurado: la VM no sabe a dónde enviar el tráfico externo.
- El switch virtual está aislado: solo conecta VMs y host, no sale a Internet.
- El host no reenvía tráfico (IP Forwarding desactivado): aunque pongas su IP como gateway, no actúa como router.


> ℹ️ **Más información sobre modo Host-Only en hipervisores:** Consulta detalles técnicos, ejemplos y usos avanzados aquí:  
> [Modo Host-Only en hipervisores](https://broken-snowdrop-f03.notion.site/Modo-Host-only-32db80eb3d8880368af2f084afd5ee80?pvs=74)
</details>

---

### Modo Internal Network (Red Interna)


En modo Internal Network, las VMs solo se comunican entre ellas dentro de una red virtual interna del hipervisor. Ni el host ni la red física tienen acceso directo a esa red.

> ⚠️ **Máximo aislamiento para escenarios multi-VM (netlabs, microservicios en testing):** Este modo es ideal para simular topologías de red complejas (cliente-servidor, DMZ, segmentación por subredes y laboratorios de red) sin ningún impacto en la red real. 

<details>
<summary><strong>Como funciona el modo Internal Network</strong></summary>
El hipervisor crea una red virtual completamente aislada (por ejemplo, ***intnet*** en VirtualBox) a la que solo pueden conectarse las VMs configuradas en modo Internal Network. El hipervisot hace DHCP interno para asignar IPs a las VMs y DNS interno para resolver nombres entre ellas.

> 	 **Más información sobre modo Internal Network en hipervisores:** Consulta detalles técnicos, ejemplos y usos avanzados aquí:  
> [Modo Internal Network en hipervisores](https://broken-snowdrop-f03.notion.site/Modo-Internal-Network-32db80eb3d88806eaeffcd0836cdade9?pvs=74)

</details>


## Configuraciones extras de Hipervisores para VMs

A continuacion tienes utilidades practicas de VirtualBox en formato desplegable.


<details>
<summary><strong>Localizacion de maquinas en VirtualBox</strong></summary>

Explica donde se guardan las VMs y como cambiar la ruta por defecto desde la configuracion global.

<img src="assets/vms-extras/vm-location-preferences.png" alt="Preferencias de ubicacion de VMs" width="70%" height="auto">

> ℹ️ **Mas informacion sobre ubicacion de VMs:**
> [Localizacion de maquinas de VirtualBox](https://www.notion.so/330b80eb3d8880e0a545f401bd38d079)

</details>

<details>
<summary><strong>Snapshots en VirtualBox</strong></summary>

Las instantaneas permiten volver a un estado anterior de la VM para pruebas, actualizaciones o recuperacion rapida. Congelan:
- El **estado del disco** (todos los archivos, sistema operativo, programas...)
- El **estado de la memoria RAM** (si la VM esta encendida)
- La **configuracion de la maquina virtual**

Se pueden crear desde la interfaz grafica (imagen inferior).

<img src="assets/vms-extras/snapshot-take.png" alt="Crear snapshot en VirtualBox" width="70%" height="auto">

**Cambiar ubicacion por defecto y consultar donde se guardan**
Desde la configuracion de la VM puedes definir el directorio por defecto de snapshots y ver su ruta actual.

<img src="assets/vms-extras/snapshot-location.png" alt="Cambiar ubicacion de snapshots" width="70%" height="auto">

> ℹ️ **Mas informacion sobre snapshots:**
> [Snapshots en VirtualBox](https://www.notion.so/330b80eb3d8880e29f9dfebe17bb8a96)

</details>

<details>
<summary><strong>Clonar maquinas en VirtualBox</strong></summary>

Sirve para duplicar una VM ya configurada y crear entornos paralelos (testing, desarrollo o backup) sin reinstalar.

> ℹ️ **Mas informacion sobre clonacion:**
> [Clonar maquinas en VirtualBox](https://www.notion.so/330b80eb3d8880788fcede88c738506f)

</details>

<details>
<summary><strong>Como exportar maquinas de VirtualBox</strong></summary>

Muestra el flujo recomendado para exportar VMs a **OVA**, moverlas entre equipos y mantener una **copia portable** del sistema. Es ideal para **backup**, **migraciones** o **compartir entornos** sin reinstalar.

<table>
<tr>
<td><img src="assets/vms-extras/export-ova-1.png" alt="Exportar VM a OVA" width="100%" height="auto"></td>
<td><img src="assets/vms-extras/export-ova-2.png" alt="Opciones de exportacion OVA" width="100%" height="auto"></td>
</tr>
</table>

**Como importar desde la OVA**
Usa la opcion de importacion para registrar la VM en otro equipo y recrear su configuracion.

<img src="assets/vms-extras/import-ova.png" alt="Importar VM desde OVA" width="70%" height="auto">

> ℹ️ **Mas informacion sobre exportar VMs:**
> [Como exportar maquinas de VirtualBox](https://www.notion.so/331b80eb3d8880a79857e346b1f8fbcd)

</details>

<details>
<summary><strong>Grupos de maquinas en VirtualBox</strong></summary>

Organiza VMs por proyecto y permite encender o apagar entornos completos de una sola vez.

<img src="assets/vms-extras/vm-groups.png" alt="Crear y gestionar grupos de VMs" width="70%" height="auto">

> ℹ️ **Mas informacion sobre grupos de VMs:**
> [Para que sirve crear grupos de maquinas en VirtualBox](https://www.notion.so/330b80eb3d8880c2b53cc07ab82ce3cc)

</details>

<details>
<summary><strong>Acceder a la documentacion de VirtualBox</strong></summary>

Indica como abrir la documentacion desde el propio cliente de VirtualBox y consultar recursos oficiales.

<img src="assets/vms-extras/vbox-docs.png" alt="Acceso a documentacion de VirtualBox" width="70%" height="auto">

> ℹ️ **Mas informacion sobre documentacion:**
> [Acceder a la documentacion de VirtualBox](https://www.notion.so/331b80eb3d88807f9c10cfe84712a6d3)

</details>

<details>
<summary><strong>Carpetas compartidas entre VMs y host</strong></summary>

Permiten compartir archivos entre el host y la VM sin usar red o USB. Son clave para provision scripts, transferencias rapidas y desarrollo con auto-montaje.
Herramientas como **Vagrant** usan estas carpetas para inyectar scripts de provision y automatizar la creacion de VMs en local, dejando el SO configurado y listo para desplegar entornos completos (backend, laboratorios, testing y mas).

> ℹ️ **Enlace a repositorio con ejemplos de uso de carpetas compartidas en Vagrant y documentacion mas extensa:**
> [Inception (GitHub)](https://github.com/alcarril/Inception)

**Carpetas compartidas globales (todas las VMs del hipervisor)**
Se configuran para que cualquier VM pueda acceder a la misma carpeta.

<img src="assets/vms-extras/shared-folder-make-global.png" alt="Carpeta compartida global" width="70%" height="auto">

**Carpetas compartidas por maquina (solo una VM)**
Se asocian a una VM concreta para aislar acceso y permisos.

<img src="assets/vms-extras/shared-folder-create.png" alt="Carpeta compartida por maquina" width="70%" height="auto">

> ℹ️ **Mas informacion sobre carpetas compartidas:**
> [Crear carpetas compartidas entre VMs y sistema host](https://www.notion.so/331b80eb3d888023858fde4fd5fd3add)

</details>

