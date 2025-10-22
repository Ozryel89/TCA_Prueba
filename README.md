# Librerías
Las librerías que se usaron fueron las siguientes:
| Librería | Uso |
| ------------- | ------ |
|import os|Se uso esta librería para tener acceso al directorio y facilitar el ingreso de direcciones|
|import pandas as pd|Se uso esta librería para realizar el proceso EDA|

# Comandos

Los comandos necesarios para el código fueron: 
| Comando | Uso |
| ------------- | ------ |
|os.getcwd() +| Este comando tiene la función de entregar el directorio de trabajo con el + para añadir el camino a los archivos parquet.|
|pd.read_parquet()|Permite leer los archivos parquet en un DataFrame de pandas se pueden agregar filtros para extraer solo la información deseada.|
|.info()|Entrega la información del DataFrame incluyendo el index, dtype, columnas, valores non-null y el uso de memoria|
|.describe ()|Da resultados estadísticos del DataFrame, con el parámetro include pude funcionar con strings|
|.value_counts()|Entrega una serie con la frecuencia en la que se repiten los valores únicos|
|.str.strip()|Elimina los whitespaces de los strings|
|.to_dict()|Transforma el elemento en un diccionario|
|.astype()|Permite cambiar el tipo de dtype al que se especifique|
|display()|Permite hacer una inspección del DataFrame que cumpla con unas condiciones y puedes cargar solo las columnas que deseas|
|.groupby()|Crea agrupaciones de que cumplan con el elemento a agrupar con el fin de aplicar una función y combinar resultados|
|.unstack(level=1)|Deja crear un pivot con los indices|
|.plot.|Deja crear las gráficas deseadas| 
|.corr()|Crea una tabla de correlación entre las columnas|
|.dt.day_name()|Se usa con elementos del tipo datetime para dar el día de la semana|
|.dt.year()|Se usa con elementos del tipo datetime para dar el año|
|.unique()|Busca los valores únicos|
|.sum()|Hace una suma de los valores|
|.mean()|Obtiene el promedio|
|pd.ExcelWriter|Exporta el dataFrame a .xlsx|

## Ejemplo de Display
```python
display(iar_ocupaciones[(iar_ocupaciones['ID_Agencia']>-1) & (iar_ocupaciones['ingresos_habitaciones']<0) & (iar_ocupaciones['cuartos_noche']<0) & (iar_ocupaciones['TREVPEC']!=0)][['Fecha_hoy', 'ID_Agencia', 'ID_Tipo_Habitacion', 'ID_empresa', 'ingresos_habitaciones','cuartos_noche','ADR','num_adultos', 'TREVPEC']]) 
```
## Ejemplo de groupby()
```python
|iar_ocupaciones.groupby([iar_ocupaciones['Fecha_hoy'].dt.year, iar_ocupaciones['ID_Tipo_Habitacion']])['ingresos_habitaciones'].mean() 
```

# Explicación del análisis libre

El análisis se empezó a través de una exploración por encima de los datos buscando alguna anomalía o elemento extraño dentro de estos. Después siguió un análisis de los distintos archivos y que elementos contienen.

Se limpiaron varios string eliminando los whitespaces, los archivos parquet con dos columnas una siendo el indice y la otra información se convirtieron en diccionarios para poder usarlos de forma directa o traducir los ID por el nombre real facilitando la exposición y guardar las constantes en este caso fueron el total de habitaciones. Por ultimo se crearon diccionarios para cambiar el dtype de múltiples columnas para facilitar futuras operaciones.

Usando la función display se vieron múltiples elementos con una configuración de filtros encontrando unos duplicados. Se trato de buscar una identidad para la columna *estatus_reservacion_id* tambien se trato de buscar el costo del *ingresos_habitaciones* dependiendo del *ID_Tipo_Habitacion* obteniendo el promedio del ingreso anual que tiene cada tipo de habitación.

Después se realizo un análisis para determinar el porcentaje de las ocupaciones que las agencias pertenecientes al grupo hotelero es responsable. Durante este análisis se vio que había ciertos hoteles con una ocupación mayor a la que se estableció en **iar_empresas**. Lo que llevo a tratar de eliminar valores duplicados usando la siguiente función.

```python
sin_duplicados = iar_ocupaciones.drop_duplicates(['Fecha_hoy', 'ID_Agencia', 'ID_Tipo_Habitacion', 'cuartos_noche', 'ID_empresa'], keep='first')
```
# Justificación 

| Columnas | Motivo |
| ------------- | ------ |
|*Fecha_hoy*|se uso para encontrar valores que ocurren el mismo día y no perjudicar resultados de personas que se quedaron mas de un día|
|*ID_Agencia, ID_Tipo_Habitacion, cuartos_noche, ID_empresa*|Estos fueron usados en conjunto porque representan quien necesita el cuarto, el tipo de cuarto, la cantidad de cuartos y el hotel a hospedarse|

Después de hacer esto todavía existía una ocupación excesiva, una forma de eliminar mas duplicados seria dejar el *ID_empresa* pero esto tiene el problema que no se sabe cual es el hotel que ocuparon. Se podría optimizar el *ID_Agencia* ya que ciertas agencias no podrían tener ocupados mas de un hotel al mismo tiempo.

# Continuación de la explicación del análisis libre

Una vez esto se continuo con el análisis para determinar el porcentaje de las ocupaciones que las agencias pertenecientes al grupo hotelero es responsable.

Se hicieron unas exploraciones con pocos resultados como la distribución de la ocupación de los cuartos dependiendo del día de la semana pero no hubo una distribución significativa entre los días fuera de que el sábado es el día mas ocupado.

Se hizo un pequeño análisis de *ingresos_habitaciones* para ver que tanto aporta las agencias del grupo. Terminando con las 10 Agencias que mas *ingresos_habitaciones* han tenido este periodo.

# Conclusiones

Se vio que casi 30% de las ocupaciones registradas en los últimos 4 años corresponden a las agencias del grupo siendo solo 133 de las 1053 agencias que tienen ocupaciones. También se vio que hay un sobre exceso de ocupaciones con respecto a la disponibilidad, posiblemente duplicados, errores al capturar los hoteles (no aparecen 2) o una desactualización de las habitaciones disponibles con el paso del tiempo. Por ultimo se vio como la UNIVERSIDAD AUTONOMA DE GUERRERO es las mas genera *ingresos_habitaciones*.
