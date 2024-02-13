# Dataset call_center .csv

Nuestro dataset en bruto tiene informacion del Call-Center de "Anonymous Bank" en Israel.
El dataset contiene las llamadas registradas durante 12 meses (desde el 01/01/99 hasta el 31/12/99)

## Descripcion Servicios

- Servicios del Call center:
  _ **Servicios a Clientes**: 1. Información y transacciones sobre cheques y cuentas de ahorros. 2. Respuestas VRU 3. Soporte Home Banking
  _ Atencion a **Potenciales Clientes**
  - Estructura del call Center: <br>
    - 1 Supervisor <br>
      - 8 agentes para atender a los clientes y potenciales clientes <br>
      - 5 agentes brindando soporte para Home-Banking a los clientes<br>
  - Horarios de Atencion: <br>
    - Domingo a Jueves: 7.00 am a 12.00 pm <br>
    - Viernes: 7.00 am a 14.00 pm <br>
    - Sabado: 20.00 pm a 12.00 pm <br>
    - VRU esta disponible 24 x 7<br>

## Descripcion Servicios

## Descripcion del Dataset

El dataset consta de 17 columnas:<br>

1. **VRU+LINEA**
   <br>

   - Hay 6 VRUs (AA01 AL AA06) con 16 lineas a asignar.<br>
   - Hay un total de 65 lineas<br>
   - Cada llamada es asignada a un **numero de VRU** + **un numero de linea**<br>
     ![Alt text](image.png)
     <br>

2. **Call_id** = 5 digitos<br>
   A cada llamada telefónica entrante se le asigna un “call_id”.<br>
   Aunque son diferentes, los identificadores no son necesariamente consecutivos por estar asignado a diferentes VRUs.<br>

3. **Customer_id** = 0 a 12 dígitos<br>
   • Es la identificación del cliente. **Es única por cliente**.<br>
   • si el ID es cero: - es porque el sistema no pudo identificar a la persona que realiza la llamada - Para el caso de los prospectos no se identifican = Nuevo posible cliente<br>

4. **Priority** = 1 digito<br>

   - **ALTA** - 2 indica clientes de Alta Prioridad.<br>
     - A los **clientes de Alta Prioridad** se les asigna un **tiempo de espera de 1.5 minutos** al comienzo de su llamada (esto les permite avanzar en la posición de la cola de llamadas).<br>
     - También están **exentos de pagar un fee mensual**, que los clientes regulares deben pagar.<br>
   - **REGULAR** - 0 identifican a clientes sin identificar <br>
     - 1 identifican a las llamadas de los clientes regulares<br>
     - Hay clientes con prioridad 0, pero son tratados como si fueran de prioridad <br>
       - Se definió que los clientes con prioridad 0 corresponden a clientes de prioridad 1 que no realizaron Upgrade a Alta Prioridad (prioridad 2). <br>
       - Debido a un error en el sistema, el cliente I.D. no fue registrado para aquellos que no esperaron en la cola, Por lo tanto, su prioridad es 0.<br>

5. **Type** = 2 digitos<br>
   Es el tipo de transaccion que el cliente requiere realizar.<br>
   Hay 6 tipos diferentes de servicio:<br>
   • PS - Actividad Regular<br>
   • PE - Actividad Regular en inglés<br>
   • IN - Actividad / Consulta por internet<br>
   • NE - Actividad por Acciones (stock exchange)<br>
   • NW - Cliente potencial (prospecto) solicitando información<br>
   • TT – clientes que dejan un mensaje pidiendo al banco que le devuelvan su llamado pero que cuando el sistema automático devuelve el llamado, el agente pasó a estado “ocupado”, dejando al cliente en espera en la cola.<br>
6. **Date** = formato 6 digitos: dd-mm-yy<br>

7. **VRU_entry** = 6 digitos (hh:mm:secs)<br>
   **Hora** en que la llamada telefónica **ingresa al call center**.<br>
   Más específicamente, cada cliente que llama debe identificarse primero lo que se realiza proporcionando a la VRU la ID del cliente. Por lo tanto, _esta es la hora en que la llamada ingresa a la VRU_ luego de que el cliente se identifico.<br>

8. **VRU_exit** = 6 digitos (hh:mm:secs)<br>

   - Hora de salida de la VRU:<br>
     1. A la cola<br>
     2. O directamente para recibir el servicio.<br>
     3. O para dejar el Sistema (abandono).<br>

9. **VRU_time** = 1 a 3 dígitos (segundos totales en cola)<br>
   Tiempo (en segundos) de espera en la VRU (calculada como vru_time = VRU_exit – VRU_entry)<br>

10. **q_start** = 6 dígitos (hh:mm:secs)<br>
    Hora en la que se une a la cola.<br>

    - la llamada queda “en espera”, mientras esta en la cola<br>
    - Este valor es 00:00:00, para clientes que llegan a ponerse en la cola (abandonan cuando están en la VRU)<br>

11. **q_exit** = 6 digitos (hh:mm:secs)<br>

    - Hora en la que se sale de la cola.<br>
    - Ya sea porque recibe el servicio o por qué abandona el llamado.<br>

12. **q_time** = 1 a 3 dígitos (segundos totales en cola)<br>

    - Tiempo de espera en la cola (calculado por q_time = q_exit – q_start)<br>

13. **Outcome** = 4,5 o 7 digitos<br>
    Hay tres posibles salidas por cada llamada<br>

    1. AGENT: se dio servicio <br>
    2. HANG: se cortó la llamada y no se dió servicio <br>
    3. PHANTOM: una llamada en la que virtualmente se ignora lo que sucedió (afortunadamente son pocas llamadas en esta situación).<br>

14. **ser_start** = 6 digitos (hh:mm:secs)<br>
    Hora de comienzo del servicio por un agente.<br>

15. **ser_exit** = 6 digitos (hh:mm:secs)<br>
    Hora de salida del servicio por un agente.<br>

16. **ser_time** = 1 a 3 dígitos (segundos totales en servicio)<br>
    Duración del servicio en segundos (calculada como ser_time = ser_exit – ser_start)<br>

17. **Server_text**

    - Nombre del agente que atendió la llamada.<br>
    - Este campo es NO_SERVER, si el servicio no fue provisto.<br>

18. **startdate**

# Diagrama de flujo del proceso

![Alt text](image-1.png)
