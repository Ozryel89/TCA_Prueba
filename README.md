Las librerias que se usaron fueron las siguientes: 

+ import os

+ import

Los comandos necesarios para el código fueron: 

+ os.getcwd()+

+ pd.read_parquet()

+ .info()

+ . describe ()

+ .value_counts()

+ .str.strip().to_dict() 

+ .str.strip() 

+ .astype() 

+ display(iar_ocupaciones[(iar_ocupaciones['ID_Agencia']>-1) & (iar_ocupaciones['ingresos_habitaciones']<0) & (iar_ocupaciones['cuartos_noche']<0) & (iar_ocupaciones['TREVPEC']!=0)][['Fecha_hoy', 'ID_Agencia', 'ID_Tipo_Habitacion', 'ID_empresa', 'ingresos_habitaciones','cuartos_noche','ADR','num_adultos', 'TREVPEC']]) 

+ iar_ocupaciones.groupby([iar_ocupaciones['Fecha_hoy'].dt.year, iar_ocupaciones['ID_Tipo_Habitacion']])['ingresos_habitaciones'].mean() 

+ .unstack(level=1) 

.plot.line() 

 

El análisis libre consistió en buscar que tanto llenan los hoteles las agencias que están relacionadas con nuestro catálogo de hoteles usando la información de ocupaciones 

Se opto por eliminar los datos repetidos ya que estos excedían la cantidad de habitaciones disponibles esto se hizo con la siguiente funcion: 

sin_duplicados = iar_ocupaciones.drop_duplicates(['Fecha_hoy', 'ID_Agencia', 'ID_Tipo_Habitacion', 'cuartos_noche', 'ID_empresa'], keep='first') 

Como conclusión se vio que casi 30% de las ocupaciones en los últimos 4 siendo solo 133 de las 1053 agencias que tienen ocupaciones. 

También se vio que hay un sobre exceso de ocupaciones con respecto a la disponibilidad. Y como la UNIVERSIDAD AUTONOMA DE GUERRERO de las mayores fuentes de ingreso. 
