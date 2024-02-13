# Analisis del Dataset

## Carga de los datos a un DataFrame de Pandas

Con la ayuda de la multiples librerias de Python procedemos a analizar los datos del dataset y a limpiarlo, normalizarlo y analizarlo.<br>
<br>

```
# cargar los datos desde el archivo csv.

import pandas as pd
import csv
import numpy as np
import matplotlib.pyplot as plt
import random as rnd
import seaborn as sns
```

### El dataset contiene 444448 registros **sin valores nulos**

![alt text](image-10.png)

El dataset esta compuesto por:
<resumen sintetico>

![alt text](image-11.png)

Utilizando Data Wrangler profundizamos el analisis exploratorio de cada columna, su informacion, oportunidades y sugerencias.

# Variables e informacion del dataset

## 1. vru.line

![Alt text](image-2.png)<br>

- En esta columna se designa el VRU que atiende la llamada, es unico para cada call_id <br>
- Pero hay 6 VRU con capacidad para asignar lineas del 1 al 16.<br>
- En esta columna no hay datos faltantes.<br>
- Y el datatype es object, deberia ser string.<br>
  <br>

### **Sugerencias** <br>

- Hay solo 31 lineas que se utilizan de manera regular de un total de 65 lineas disponibles.<br>
  Esto es obvio al analizar la distribucion de frecuencia de los VRU, cantidad de llamadas por ano por VRU

## 2. call_id

![Alt text](image-3.png)

- Es la identificacion que se le asigna a cada llamada entrante. <br>
- Vemos que en el dataset existen 444.443 registros, pero solo hay 54.472 call_id.<br>
  **El VRU designa este numero, pero se puede ver que el mismo numero corresponde a llamadas diferentes en momentos diferentes** <br>

  _Por lo que los call_id se repiten el mismo para distintas llamadas_ <br>

### **Sugerencia** <br>

- Este campo deberia ser **Primary Key** utilizarse como clave. <br>
- Otra alternativa es combinar **call_id + vru.line** para identificar cada registro <br>

## 3. customer_id

![Alt text](image-6.png)

- Tiene el **53% de los valores es cero** = 234.552 registros de un total de 444.443 <br>

- **Prospectos de Clientes** deberian ser los que tengan type == 'NW' customer_id == 0 <br>
  - El _0 solo deberia utilizarse para aquellas llamadas de prospectos de clientes_. <br>
    Estos son solo 14.381 registros del total de llamados recibidos.
- **Esto es 53% de las llamadas recibidas no identifican al cliente**

  - Vamos a identificar estas llamadas como customer_id = 'CustNotId'
  - Estos clientes pudieran resultar ser clientes prioritarios que estan siendo atendidos sin prioridad.
  - Estos deberian ser solo los prospectos de clientes, todos los clientes regulares deberian estar identificados y ser minimos los clientes no identificados.

- **Clientes regulares sin identificar** <br>
  Se han recibido **220.171 llamados de clientes regulares que no fueron identificados al momento de ingresar en el call_center**.<br>

- **Limpieza**<br>
  ![Alt text](image-7.png)
  1.  Tenemos valores guardados como floats, corregimos el datatype. <br>
  2.  Vamos a identificar los Prospectos de Clientes como **'ProspectCust'= 999999999999**<br>
  3.  Todos las llamadas que corresponden a clientes regulares y prioritarios sin identificar los vamos designar como **'CustNotId'= 0**<br>

# 4. priority

![Alt text](image-8.png)

- Todos los clientes deberian ser identificados y acorde al servicio, asignada una prioridad. Al ser defectuoso el proceso de identificacion de los clientes, la asignacion de prioridades tambien esta funcionando o seteada con fallas.<br>

**Funcionamiento ideal**

- Del total de los _clientes identificados_:<br> - **Clientes con Prioridad** (priority = 2) _son 137.453_, _el 31% del total_ de las llamadas y el **65% de las llamadas identificadas**<br>

  - **Clientes Regulares** (priority = 1) _son 71.827_, _el 16% del total_ de las llamadas y el **34% de las llamadas identificadas** <br>

- Clientes no identificados:
  - Verificamos que todos los Prospectos de clientes tengan prioridad 0 <br>

Analisis:

- El **85% de las llamadas entrantes son de clientes**, aunque solo se identificaron como **clientes solo el 47%**. <br> - El 15% son Prospectos de Clientes. <br> - Del restante 85% correspondientes a clientes:
  _ solo **47% se identifican como clientes**.
  _ _65% son prioritarios_ = 31% del total de llamadas \* _34% son regulares_ = 16% del total de llamadas

- **Limpieza**<br>
  Normalizamos los tipos corrigiendo los valores TT y AA.<br>
- Eliminamos los registros AA.<br>
- Corregimos ' TT' por 'TT'<br>

# 5. type

Hay 6 posibles servicios que provee el call center a los clientes que se contactan:<br> 1. PS = Actividad regular<br> 2. PE = Actividad regular en ingles<br> 3. NE = Actividad Acciones <br> 4. IN = Soporte Home-Banking <br> 5. TT = Solicitud de contacto directo del banco <br> 6. NW = Cliente Potencial - informacion <br>

    -   El 68% de los contactos al call center son para resolver actividades regulares.
    -   La segunda actividad del call center en orden de importancia, son los contactos de los prospectos de cliente, 15% de los contactos
    -   La tercera es suporte sobre Mercado de acciones con el 9% de los contactos.
    -   Por ultimo, solo 5% de los contactos son para recibir soporte para Home-Banking
    -   Y un 3% unicamente solicita que el banco los contacte de manera directa.

# 6. date

Tenemos registradas 444.443 llamadas a lo largo de todo un ano.

Analizando la frecuencia y cantidad de llamadas por mes, podemos ver que la cantidad de llamadas recibidas por mes es bastante constante:

- El _promedio de llamadas_ recibidas por mes es de **37.036 llamadas** y con un desvio estandar de 4.242 llamadas.
- El mes que mas llamadas se registraron fue _Diciembre_ con un total **43.065 llamadas**
- El mes que menos llamadas registro fue _Septiembre_ con un total de **31.370 llamadas**
- La **amplitud** es de _11.695 llamadas_.

# 7. vru_entry

# 13. outcome

![Alt text](image-9.png)
Hay 3 posibilidades de resolucion de una llamada:<br>

1.  **AGENT** = que corresponde a los llamados atendidos por los agentes del call center, son el **79% de las llamadas**
2.  **HANG** = el cliente corta el llamado antes de ser atendido. Son el **20% de las llamadas ingresadas**.
3.  **PHANTOM** = una llamada en la que virtualmente se ignora. **Son 4.440 llamadas en total, un promedio de 12 llamadas por dia. Un 1%**

# 18. Stardate

No esta definida dentro del proceso del callcenter, por lo que no se le puede asignar un significado a la misma ni relacionarla con algo en especial.
Es por ello que vamos a proceder a eliminar esta columna, y descartarla de nuestro analisis.
