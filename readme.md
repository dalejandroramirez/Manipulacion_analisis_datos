
# Series e indexacción 
_________________________________________________________________________
sr = pd.Series(<list>) ⇒ crear una serie

sr = pd.Series(dic, index=<list_of_indexes>) ⇒ crea una Serie con índices específicos.

sr.values ⇒ obtener los valores de la serie

sr.index ⇒ obtener los índices de la serie

sr.shape ⇒ obtener la dimensión

sr[<index>] ⇒ obtener un elemento de la serie a través de un índice

sr[<list_of_indexes>] ⇒ obtener valores a través de una lista índices.

sr.isnull() ⇒ Lista con True cuando el valor es nulo

sr.notnull() ⇒ Lista con True cuando el valor es no nulo

_____________________________________________________________________________
# Filtrado de datos

df.loc Para ubicar por el nombre de las columnas e indices

df.iloc Para ubicar por número de columna y fila

______________________________________________________________________________
# Indexado y manejo CSV, EXCEL, json, pickle, parquet,dhf

CSV - Es muy versatil ya que solo tiene comas y saltos de linea.

JSON - Tiene un formato muy similar al de un diccionario de Python.

Excel - Permite guardar el archivo en formato .xls para trabajar con el en Excel o Spreadsheets.

Pickle - Permite comprimir la información, es util cuando se tienen tablas grandes.

Parquet - Permite darle un formato que puede usarse en ambientes de Big Data como Hadoop
________________________________________________________________________________________
# Consumo de memoria y disco
- CSV y formatos String : Son simples, requieren alto costo computacional y algo lentos.

- HDF : Gran soporte, adecuado para grandes cantidades de datos, rápido a costo de alto costo computacional.

- Parquet : Puede igualar a hdf e inclusive trabajar por chunks y en paralelo.

- Pickle : Es práctico pero lento con grandes cantidades de dato


https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html

- # Para cargar un archivo basta con tener un dataframe llamado df

    - df.to_csv("path\nombre.csv", sep = ",",index= False) 

    - df.to_excel("path\nombre.xlsx", sep = ",",index= False,sheet_name="numero_hoja")

    - df.to_json("path\nombre.json")

    - df.to_pickle("path\nombre.pkl")

    - df.to_parquet("path\nombre.parquet")

    - df.to_dhf("path\nombre.hdf",key="data",format="table)


- # Para leer un archivo basta con usar
    
    - pd.read_csv("path\nombre.csv)

    - pd.read_excel("path\nombre.xlsx")

    - pd.read_json("path\nombre.json")

    - pd.read_pickle("path\nombre.pkl")

    - pd.read_parquet("path\nombre.parquet")

    - pd.read_hdf("path\nombre.hdfs")

# Estructura de los dataframe

Comandos utiltes:

    - df_meteorites["fall"].unique()  # devuelve los tipos de la columna fall

    - df_meteorites["fall"].value_counts() # cuenta las ocurrencias de cada categoria

# Transformaciones 

# Variables dummies

    - pd.get_dummies(df_meteorites["fall"])

descripcion: En df_meteorites["fall"] es una columna que tiene solo dos catagorias "Fell" y "Found" y pd.get_dummies() crea columnas categoricas para cada uno de estos tipos 

# Formato tiempo 

- pd.to_datetime(df_meteorites["year"],errors="coerce",format="%m/%d/%Y %H:%M:%S %p")

    Donde errors ="coerce " dice que donde haya un formato que no sea tiempo genera una variable no numerica

___________________________________________________________

# Renombrar columna

df_meteorites.rename(columns={"old key" : "new key" }, inplace=True)

- el implace=True hace que no se genere una nueva vairiable sino que se guarda en el mismo dataframe

# Borrar filas o columnas

df_meteorites.drop(["nombre_columna"],axis=1,inplace=True)

df_meteorites.drop(["nombre_fila"],axis=0,inplace=True)

df_meteorites.drop(columns=["nombre_columnas"],index=["indices_columnas"],inplace=True)

# Copia del dataframe

df_copiado=df_meteorite.copy(deep=True)

- el parametro deep es para garantizar que no se modifique el original

# Funciones lambdas

- aplicar una funcion a una columna, donde el numbre de la función es func, con argumentos estaticos en args
    df["nombre_colum"].apply(func,args=())

- df["nombre_columna"].apply(lambda x : x**2)

- df.apply(lambda x : x.mean() )  # Esto calcula el promedio de cada columna del df

- df.apply(labmda x : x.mean() ,axis=0) #calcula por las filas

- Además de apply, también se pueden usar las funciones applymap y map, dependiendo de la necesidad.

- apply() se utiliza para aplicar una función a lo largo de un eje (columna o fila).

- applymap() se usa para aplicar una función a todos los elementos del DataFrame

- map() se usa para sustituir cada valor de una fila por otro valor.

__________________________________________________________
# Multiples indices

https://data.worldbank.org/ : base de datos publica

- pd.melt(df,id_vars=["Country Name"],var_name="year", value_name="pob")

originalmente df tenia en cada columna cada año lo que hace melt es concatenar todas estas columns usando la columna de identificacion "Country Name" y nombre de variable "year" y la columan con los valores que sera "pob"

- df["Country Name"].isin(["Aruba","Colombia"])

devuelve una columan booleana donde es verdadera si esta aruba o colomiba y false en otro caso

# crear multiples indices 

- df_sample.set_index(["Country Name","year"])
En este caso el df_sample tendra dos indices que son el nombre del pais y el año

- df_sample.xs("Colombia","2015")
Realiza un filtro de indice Colombia y el año 2015

-df_countries=df.set_index(["Country Name","year"]).sort_index(ascending=[True,True])
Ordena los indices de manera creciente

- ids=pd.IndexSlice
- df_countries.loc[ids["Aruba":"Colombia","2015":"2017"],:].sort_index()

realiza una seleccion de las filas teniendo un orden especifico y toma 

# Obtener los indices

- df_countries.index.get_level_values(0)
- df_countries.index.get_level_values(1)
obtiene los indices del primer nivel y segundo nivel respectivamente

# Volver los indices columnas

- df_sample.unstack("year")

Cada uno de los indices del año se volveran columnas

# Como tratar datos faltantes

- df.dropna()
dropna es perfecto para elimnar rapidamente las filas con registros faltantes: 

- df.fillna(0) 
Usando la función fillna podremos reemplazarlas por el valor que querramos, en este caso 0.

- df_d = pd.concat([df[['colmna']], df[['colmna']].interpolate()],axis=1)
Por último, Pandas también puede interporlar los valores faltanes calculando el valor que puede haber existido en el medio.


# Group by

- df.groupby('nombre_columna') 
 Agrupa los datos en relacion a la columna seleccionada


- df.groupby('nombre_columna').mean()
 Calcula el valor esperado

- df.groupby('nombre_columna').count()
  cuenta las ocurrencias de cada clase

- df.groupby('nombre_columna')['Otra_columna'].mean()  
    Calcula la media de uno de los elementos de la columna 

# groupby de multiples indices

- df.groupby(['cut','color']).mean()
crea un dataframe con dos indices y luego calcula la media de cada una de las columnas

# Aggregate
- df.groupby(['cut','color']).aggregate([f1,f2,f3])
realiza un agrupamiento de las dos columnas y crea tres columnas donde se aplica las funciones f1,f2 y f3

con un diccionario se puede decir a que columnas quiero aplicar el aggregate
ejemplo:
dict_agg={"carat":[min,max],"price":[np.mean]}
df.groupby(["cut","color"]).aggregate(dict_agg)

# Crear filtros con filter()
Sea una funcion booleana func,
- df.groupby("cut").filter(func)

# Datos duplicados
Es muy usual que los registros de una base de datos aparezcan más de una vez, así que veamos cómo pandas puede ayudarnos a lidiar con estos casos. Para comenzar, importemos pandas y creemos un DataFrame con dos columnas y algunos datos repetidos.

Para encontrar los registros duplicados usamos duplicated , que marca con True aquellos casos de filas duplicadas:

- df.duplicated()

Podemos usar keep='first' para marcar solo la primera ocurrencia o keep='last' para marcar la última:

- df.duplicated(keep='first')

Identificados los casos duplicados, podemos usar este resultado para filtrar y seleccionar aquellos que no tienen un registro duplicado:

- df[~ df.duplicated()]

Remarco el hecho de que usé negación '~' para ver los registros no duplicados.

si me interesara ver cuáles son los registros duplicados, podemos usar keep=False:

- df.duplicated(keep=False)

 puedes usar el comando 'drop_duplicates' para eliminar los duplicados. Por defecto, la función guarda el primer resultado keep='first':

 - df.drop_duplicates()

 Y si quieres solo borrar duplicados teniendo en cuenta una sola columna, lo puedes hacer mediante una lista nombrando las columnas donde vas a eliminar los duplicados, en este caso, ['a']:

 df.drop_duplicates(['a'],keep='last')