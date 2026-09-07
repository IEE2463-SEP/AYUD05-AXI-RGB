# AYUD05 · AXI RGB

> Protocolo AXI y modulación PWM: un IP-Core propio con puerto AXI esclavo, el `RGB_driver`, que recibe ciclos de trabajo desde un *AXI Traffic Generator* y con ellos regula la intensidad del LED RGB de la ZyboZ7, validando las transacciones con un ILA.

Ayudante a cargo: **Ernesto Ferrante** — ernesto.ferrante@uc.cl

Esta ayudantía se divide en dos partes bien distintas:

| | Qué es | Cuándo se hace |
| :--- | :--- | :--- |
| **Actividades previas** | Crear el IP-Core `RGB_driver` con puerto AXI, alimentarlo desde un ATG con los archivos `.COE` y validar la transacción con un ILA. | **Antes**, en su casa. |
| **Ejercicio propuesto** | Extender el driver a los tres canales del LED para generar los 16 colores de una tabla RGB, seleccionables con los 4 switches, más un parpadeo periódico. | **Durante** la ayudantía. |

---

## 🎥 Antes de la ayudantía

Debes llegar a la sesión con las **actividades previas ya desarrolladas y funcionando en la tarjeta**: el IP-Core `RGB_driver` con puerto AXI esclavo empaquetado e incorporado al *Block Design*, un *AXI Traffic Generator* en **Test Mode** con protocolo **AXI Lite** cargado con los cuatro archivos `.COE`, un bloque ILA de tipo *MonitorType AXI* entre el ATG y el driver, y el *bitstream* generado desde el *HDL Wrapper*. El driver compara cuatro constantes contra un contador diente de sierra y, mediante el ciclo de trabajo de la señal PWM resultante, varía la intensidad lumínica del LED RGB. Todo el detalle está en el [enunciado](https://github.com/IEE2463-SEP/AYUD05-AXI-RGB/blob/HEAD/AYUD05-AXI-RGB.pdf).

Estas actividades están resueltas paso a paso en este video, grabado el año **2023 por el ex ayudante del curso Reimundo Alcalde**:

[![Video de la ayudantía 05](https://img.youtube.com/vi/6MO-9q7fUwE/hqdefault.jpg)](https://youtu.be/6MO-9q7fUwE)

**Apoyarse en el video es opcional.** Puede seguirlo completo, usarlo sólo para destrabar un punto puntual, o resolver las actividades por su cuenta: eso lo decide usted. Lo que no es opcional es llegar con el diseño andando, porque el tiempo de la ayudantía se destinará por completo al ejercicio propuesto.

---

## 📂 Material

| Archivo | Descripción |
| :--- | :--- |
| [AYUD05-AXI-RGB.pdf](https://github.com/IEE2463-SEP/AYUD05-AXI-RGB/blob/HEAD/AYUD05-AXI-RGB.pdf) | Enunciado de la ayudantía: actividades previas, ejercicio propuesto, tabla de valores RGB y diagrama de la señal PWM. |
| [RGB_driver_v1_0.vhd](https://github.com/IEE2463-SEP/AYUD05-AXI-RGB/blob/HEAD/RGB_driver_v1_0.vhd) | Fuente de mayor jerarquía del IP-Core: expone `clk`, la entrada `sw` y la salida `RGB_B`, e instancia la interfaz AXI. |
| [RGB_driver_v1_0_S00_AXI.vhd](https://github.com/IEE2463-SEP/AYUD05-AXI-RGB/blob/HEAD/RGB_driver_v1_0_S00_AXI.vhd) | Fuente de menor jerarquía: la interfaz AXI Lite con sus cuatro registros esclavos (`slv_reg0` a `slv_reg3`) y la generación de la señal PWM. |
| [Ay5_coe_files.rar](https://github.com/IEE2463-SEP/AYUD05-AXI-RGB/blob/HEAD/Ay5_coe_files.rar) | Los cuatro archivos `.COE` que carga el ATG: `addr.coe`, `ctrl.coe`, `data.coe` y `mask.coe` (dirección, control, datos y máscara). |
| [Zybo-Z7-Master.xdc](https://github.com/IEE2463-SEP/AYUD05-AXI-RGB/blob/HEAD/Zybo-Z7-Master.xdc) | Constraints de la tarjeta (mapeo de pines). |

---

## 🧪 Durante la ayudantía

El **ejercicio propuesto** es el trabajo de la sesión, y fue propuesto y desarrollado por el ayudante de este semestre, **Ernesto Ferrante**. Se parte del driver que maneja un solo canal y se llega a los **16 colores** de la tabla del enunciado, seleccionables con los switches:

- Modificar el IP-Core para exponer las **tres** salidas del LED RGB y ampliar la entrada de switches a **4 bits**. Recuerde propagar los puertos nuevos al archivo de mayor jerarquía y re-empaquetar el IP-Core antes de incorporarlo al *Block Design*.
- Ampliar la cantidad de registros esclavos. Los 16 colores se construyen a partir de un número reducido de niveles de intensidad distintos: determine cuántos son y dimensione los registros en consecuencia.
- Modificar los cuatro archivos `.COE` para cargar esos niveles en los registros esclavos, verificando que todos tengan la misma cantidad de transacciones y que las direcciones correspondan al mapa asignado en el *Address Editor*.
- Escribir una *Look-Up Table* que reciba los 4 switches y seleccione, para cada canal del LED, el registro esclavo con la intensidad del color pedido.
- Agregar un mecanismo de **parpadeo** (*blink*) que encienda y apague el color seleccionado de forma periódica, sin alterar el tono. El reloj del sistema es de **125 MHz**, y el tiempo de *blink* puede ser de 1 o 2 segundos: si tiene un grupo trabajando al lado, pónganse de acuerdo para usar tiempos distintos.
- Validar con el ILA que las transacciones AXI hacia el `RGB_driver` se realicen correctamente, capturando las señales de dirección, dato y control.

> ⚠️ Los valores de la tabla RGB están en el rango `[0, 255]`, mientras que el contador del PWM alcanza un valor máximo distinto: **escale cada nivel antes de escribirlo en el archivo `.COE`**.

### 📤 Entrega y bonificación

El desarrollo del ejercicio propuesto se sube a **Canvas el mismo día de la ayudantía, hasta las 14:50**. Entregarlo dentro de plazo otorga **una décima (+0,1)** en la nota del **Proyecto 1**.

---

<sub>IEE2463 · Sistemas Electrónicos Programables</sub>
