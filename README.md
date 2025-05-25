![Static Badge](https://img.shields.io/badge/Oracle_Next_Education-blue?style=plastic&logo=python&logoColor=green&logoSize=auto&label=Alura%20Latam)
![Static Badge](https://img.shields.io/badge/Proyecto-blue?style=plastic&logo=pandas&logoColor=green&logoSize=auto&label=Pandas%20ETL)
![Static Badge](https://img.shields.io/badge/Certificado_del_curso-Alura?logo=pandas&color=%231e4181&link=https%3A%2F%2Fapp.aluracursos.com%2Fcertificate%2Fguz27-unameconomia%2Fpandas-transformacion-manipulacion-datos)
# Análisis de ventas

## **Objetivo del proyecto**
Analizar los resultados de un evento con los clientes de una empresa de venta online. Se recopiló un conjunto de datos que contiene los clientes que más gastaron en productos durante los 5 días de ventas, que es la duración del evento. Este análisis identificará al cliente con la mayor compra esta semana, quien recibirá un premio de la tienda, y posteriormente, puede ayudar a la empresa a crear nuevas estrategias para atraer más clientes.

## Desarrollo
### **Importar librerías y archivo datos_ventas_clientes.json**

    datos_ventas=pd.read_json('/content/datos_ventas_clientes.json')
Al importar los datos nos encontramos con columnas anidadas. Para la resolución de la problemática se aplicó la normalización de datos.
|index|dados\_vendas|
|---|---|
|0|\{'Data de venda': '06/06/2022', 'Cliente': \['@ANA \_LUCIA 321', 'DieGO ARMANDIU 210', 'DieGO ARMANDIU 210', 'DieGO ARMANDIU 210'\], 'Valor da compra': \['R$ 836,5', 'R$ 573,33', 'R$ 392,8', 'R$ 512,34'\]\}|
|1|\{'Data de venda': '07/06/2022', 'Cliente': \['Isabely JOanes 738', 'Isabely JOanes 738', 'Isabely JOanes 738', 'Isabely JOanes 738'\], 'Valor da compra': \['R$ 825,31', 'R$ 168,07', 'R$ 339,18', 'R$ 314,69'\]\}|
|2|\{'Data de venda': '08/06/2022', 'Cliente': \['Isabely JOanes 738', 'JOãO Gabriel 671', 'Julya meireles 914', 'Julya meireles 914'\], 'Valor da compra': \['R$ 682,05', 'R$ 386,34', 'R$ 622,65', 'R$ 630,79'\]\}|
|3|\{'Data de venda': '09/06/2022', 'Cliente': \['Julya meireles 914', 'MaRIA Julia 444', 'MaRIA Julia 444', 'MaRIA Julia 444'\], 'Valor da compra': \['R$ 390,3', 'R$ 759,16', 'R$ 334,47', 'R$ 678,78'\]\}|
|4|\{'Data de venda': '10/06/2022', 'Cliente': \['MaRIA Julia 444', 'PEDRO PASCO 812', 'Paulo castro 481', 'Thiago fritzz 883'\], 'Valor da compra': \['R$ 314,24', 'R$ 311,15', 'R$ 899,16', 'R$ 885,24'\]\}|

### **Normalización y transformación de datos**

**Objetivo particular:**
- Eliminar datos en listas dentro del DataFrame

- Verificar tipos de datos

- Identificar columnas numéricas

- Transformar la columna numérica a tipo numérico
---
**Normalización de datos**

datos_ventas_normalize = pd.json_normalize(datos_ventas['dados_vendas'])
La normalización de datos se llevó a cabo mediante el método `pd.jason_normalize` a la columna 'dados_vendas', sin
embargo, al ejecutar el código se presentó de nuevo la situación de anidamiento en las columnas 'cliente' y 'valor de compra'.    
   
|index|Data de venda|Cliente|Valor da compra|
|---|---|---|---|
|0|06/06/2022|@ANA \_LUCIA 321,DieGO ARMANDIU 210,DieGO ARMANDIU 210,DieGO ARMANDIU 210|R$ 836,5,R$ 573,33,R$ 392,8,R$ 512,34|
|1|07/06/2022|Isabely JOanes 738,Isabely JOanes 738,Isabely JOanes 738,Isabely JOanes 738|R$ 825,31,R$ 168,07,R$ 339,18,R$ 314,69|
|2|08/06/2022|Isabely JOanes 738,JOãO Gabriel 671,Julya meireles 914,Julya meireles 914|R$ 682,05,R$ 386,34,R$ 622,65,R$ 630,79|
|3|09/06/2022|Julya meireles 914,MaRIA Julia 444,MaRIA Julia 444,MaRIA Julia 444|R$ 390,3,R$ 759,16,R$ 334,47,R$ 678,78|
|4|10/06/2022|MaRIA Julia 444,PEDRO PASCO 812,Paulo castro 481,Thiago fritzz 883|R$ 314,24,R$ 311,15,R$ 899,16,R$ 885,24|

Primero, se declaró la lista de `'columnas'`.
Para llevar a cabo la normalización de una o más columnas se aplicó el método `.explode(columnas[1:]`y se tomó como parámetro la variable `'columas'`
Al desplegar las columnas anidadas, los índices de la tabla se modificaron, por lo que, se aplicó el método `.reset_index(drop=True, inplace=True)`

    columnas = list(datos_ventas_normalize.columns)
    datos_ventas_normalize = datos_ventas_normalize.explode(columnas[1:])
    datos_ventas_normalize.reset_index(drop=True, inplace=True)

|index|Data de venda|Cliente|Valor da compra|
|---|---|---|---|
|0|06/06/2022|@ANA \_LUCIA 321|R$ 836,5|
|1|06/06/2022|DieGO ARMANDIU 210|R$ 573,33|
|2|06/06/2022|DieGO ARMANDIU 210|R$ 392,8|
|3|06/06/2022|DieGO ARMANDIU 210|R$ 512,34|
|4|07/06/2022|Isabely JOanes 738|R$ 825,31|
|5|07/06/2022|Isabely JOanes 738|R$ 168,07|
|6|07/06/2022|Isabely JOanes 738|R$ 339,18|
|7|07/06/2022|Isabely JOanes 738|R$ 314,69|
|8|08/06/2022|Isabely JOanes 738|R$ 682,05|
|9|08/06/2022|JOãO Gabriel 671|R$ 386,34|
|10|08/06/2022|Julya meireles 914|R$ 622,65|
|11|08/06/2022|Julya meireles 914|R$ 630,79|
|12|09/06/2022|Julya meireles 914|R$ 390,3|
|13|09/06/2022|MaRIA Julia 444|R$ 759,16|
|14|09/06/2022|MaRIA Julia 444|R$ 334,47|
|15|09/06/2022|MaRIA Julia 444|R$ 678,78|
|16|10/06/2022|MaRIA Julia 444|R$ 314,24|
|17|10/06/2022|PEDRO PASCO 812|R$ 311,15|
|18|10/06/2022|Paulo castro 481|R$ 899,16|
|19|10/06/2022|Thiago fritzz 883|R$ 885,24|

---
**Limpieza de datos**
Una vez normalizadas las columnas, identificamos que en las columnas `'Cliente', 'valor de compra'`los datos de cada una de las filas presentan las siguientes dificultades
- Clientes: los nombres de los clientes fueron capturados con mayúsculas y minúsculas, adicionalmente están acompañados por caracteres numéricos.
- Valor de compra: los datos de esta columna serían más provechosos si se usan los registros como valores float.


Para retirar los caracteres de no numéricos de la columna `'Valor de compra'`, se aplicó `.apply(lambda x: x.replace('R$','').replace(',','.').strip())` 
```
datos_ventas_normalize['Valor da compra']=datos_ventas_normalize['Valor da compra'].apply(lambda x: x.replace('R$','').replace(',','.').strip())
```
```
datos_ventas_normalize['Valor da compra']=datos_ventas_normalize['Valor da compra'].astype(np.float64)
```

|index|Data de venda|Cliente|Valor da compra|
|---|---|---|---|
|0|06/06/2022|@ANA \_LUCIA 321|836\.5|
|1|06/06/2022|DieGO ARMANDIU 210|573\.33|
|2|06/06/2022|DieGO ARMANDIU 210|392\.8|
|3|06/06/2022|DieGO ARMANDIU 210|512\.34|
|4|07/06/2022|Isabely JOanes 738|825\.31|

Para realizar la limpieza de datos de la columna `'CLiente'`se aplicaron las siguientes líneas de código.

```
datos_ventas_normalize['Cliente']=datos_ventas_normalize['Cliente'].str.lower()
```
```
datos_ventas_normalize['Cliente']=datos_ventas_normalize['Cliente'].str.replace('[^a-z ]',  '', regex=True)
```
```
datos_ventas_normalize['Cliente']=datos_ventas_normalize['Cliente'].str.strip()
```
```
datos_ventas_normalize['Cliente']=datos_ventas_normalize['Cliente'].str.strip()
```
|index|Data de venda|Cliente|Valor da compra|
|---|---|---|---|
|0|06/06/2022|ana lucia|836\.5|
|1|06/06/2022|diego armandiu|573\.33|
|2|06/06/2022|diego armandiu|392\.8|
|3|06/06/2022|diego armandiu|512\.34|
|4|07/06/2022|isabely joanes|825\.31|

### **Datos de tiempo**

**Objetivo particular:**
En la columna Fecha de venta tenemos fechas en el formato 'día/mes/año' (dd/mm/AAAA). Transforme estos datos al tipo datetime y busque una forma de visualización de subconjunto que pueda contribuir al objetivo del contexto en el que se insertan los datos.

Al realizar un chequeo del DataFrame, se observó que la columna `'Data de venda'` contiene datos referidos a fechas, no obstante, los datos son de tipo `'object'`. Por lo que, hay que realizar una transformación de datos mediante los siguientes scripts:
```
# Transformar para el tipo datetime definiendo el formato de fecha como DD/MM/AAAA ('%d/%m/%Y')
datos_ventas_normalize['Data de venda'] = pd.to_datetime(datos_ventas_normalize['Data de venda'], format='%d/%m/%Y')```
```
```
# Verificar el resultado de la transformación
datos_ventas_normalize.info()```
```

```
<class 'pandas.core.frame.DataFrame'> RangeIndex: 20 entries, 0 to 19 Data columns (total 3 columns): # Column Non-Null Count Dtype --- ------ -------------- ----- 0 Data de venda 20 non-null datetime64[ns] 1 Cliente 20 non-null object 2 Valor da compra 20 non-null float64 dtypes: datetime64[ns](1), float64(1), object(1) memory usage: 612.0+ byte```
```
## **Resultado**

Retomando el objetivo del ejercicio
[...]**Este análisis identificará al cliente con la mayor compra esta semana, quien recibirá un premio de la tienda, y posteriormente, puede ayudar a la empresa a crear nuevas estrategias para atraer más clientes**

De acuerdo a los datos, Isabely Joanes recibió los beneficios de la tienda al ser la cliente con la mayor compra. 
```
# Calcular el total recaudado en compras por cada cliente
total_compras = datos_ventas_normalize.groupby(['Cliente'])['Valor da compra'].sum()
```
```
# Visualizar el resultado
total_compras.sort_values(ascending=False)
```
|Cliente|Valor da compra|
|---|---|
|isabely joanes|2329\.3|
|maria julia|2086\.65|
|julya meireles|1643\.74|
|diego armandiu|1478\.47|
|paulo castro|899\.16|
|thiago fritzz|885\.24|
|ana lucia|836\.5|
|joo gabriel|386\.34|
|pedro pasco|311\.15|


![Image](https://github.com/user-attachments/assets/24d7ea71-5906-437e-889d-7b5a01363c67)
