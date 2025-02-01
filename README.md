# Proyecto Final de Web Semántica y Datos Abiertos

## Integrantes del Proyecto
- **Oscar Mendoza**
- **Nicolás Sepúlveda**
- **Gabriela Castillo**

![Foto de los Integrantes](https://deinsoluciones.cl/web_semantica/integrantes.png)

---

## Descripción del Proyecto
Este proyecto tiene como objetivo transformar datos de importaciones provenientes de fuentes de datos públicas proporcionadas por el servicio nacional de aduana, en una representación semántica utilizando grafos RDF y consultas SPARQL. A través de esta representación, se facilita la integración, consulta y análisis de la información.



---

## Fuentes de Datos Utilizadas
![Foto de las Fuentas de Informacion](https://deinsoluciones.cl/web_semantica/digrama.png)

1. **Datos públicos del Registro de Importación 2024**  
   [Registro de Importación - datos.gob.cl](https://datos.gob.cl/dataset/registro-de-importacion-2024)  
   - Datos en formato CSV que se convertirán a RDF.

2. **Códigos Complementarios Aduaneros**  
   [Información Complementaria de Códigos Aduaneros](http://comext.aduana.cl:7001/codigos/)  
   - Información proporcionada en formato HTML que sera copiada a JSON  y que se transformará a RDF.

3. **Manifiestos Marítimos**  
   [Manifiestos Marítimos](http://comext.aduana.cl:7001/ManifestacionMaritima/)  
   - Datos en formato HTML que seran copiados a JSON que seran utilizados para la representación semántica de los manifiestos maritimos.

---

## Representación de la Información en Grafo
El grafo de la representación semántica será estructurado de acuerdo con la siguiente visualización:  

![Representación del Grafo](https://deinsoluciones.cl/web_semantica/web_semantica.png)

---

## Esquema Shex de las Clases

### **Clase `:Importacion`**
```shex
prefix :       <http://example.org/>
prefix xsd:    <http://www.w3.org/2001/XMLSchema#>
prefix schema: <http://schema.org/>

:Importacion {
    schema:num_encriptado                   xsd:integer ; 
    schema:arancel_nacional                 xsd:string ;           
    schema:total_bultos                     xsd:integer ;               
    schema:fob                              xsd:float ;                                                          
    schema:importador                       @:Cliente * ;                
    schema:aduana                           @:Aduana * ;                    
    schema:pais_origen                      @:Pais * ;                           
    schema:puerto_embarque                  @:Puerto * ;           
    schema:puerto_desembarque               @:Puerto * ;        
    schema:nave                             @:Nave *                     
}

:Cliente { 
    schema:numero_unico_importador          xsd:integer ; 
    schema:comuna_importador                xsd:string 
}

:Aduana {
    schema:nombre                           xsd:string ; 
}

:Pais {
    schema:nombre                           xsd:string ; 
}

:Puerto {
    schema:nombre                           xsd:string ; 
}

:Nave {
    schema:manifiesto                       xsd:string ;
    schema:nombre                           xsd:string ; 
    schema:agente_nave                      xsd:string  
}
```

### **Clase `:MarcasImportaciones`**
```shex
prefix :       <http://example.org/>
prefix xsd:    <http://www.w3.org/2001/XMLSchema#>
prefix schema: <http://schema.org/>

:MarcasImportaciones { 
    schema:numero_identificador          xsd:integer ; 
    schema:nombre                        xsd:string ;
    schema:marca                         xsd:string 
}
```

## Pasos para la Ejecución

### 1. Ejecución del Notebook `01_Analisis_Importaciones_de_Vehiculos_Remasterizado.ipynb`

Este notebook ubicado en  **Carpeta `01/03_notebook`**  trabaja con el archivo **`importaciones_enero_2024.csv`**, filtrando únicamente la información relacionada con importaciones de vehículos. Durante la ejecución, se generan los siguientes archivos base:

- **`informacion_base_importaciones_vehiculos.csv`**: Este archivo base servirá como input para generar:  
  - **`importadores.ttl`**: Contiene información de los clientes que importan vehículos a Chile.  

Adicionalmente, utilizando el archivo **`manifiestos_maritimos.json`** y cruzándolo con **`informacion_base_importaciones_vehiculos.csv`**, se genera el archivo:

- **`naves.ttl`**: Contiene información RDF sobre las naves involucradas en el proceso de importación.

A partir del archivo **`datos_importaciones.json`**, se generan los siguientes archivos TTL adicionales:

- **`aduana.ttl`**: Información de las aduanas.  
- **`paises.ttl`**: Información de los países de origen.  
- **`puertos.ttl`**: Información de los puertos de embarque y desembarque.  

---

### 1.2. Ejecución del Notebook `02_Generacion_Archivo_Rdf_Consolidado.ipynb`

Este notebook ubicado en  **Carpeta `01/03_notebook`** toma el archivo base **`informacion_base_importaciones_vehiculos.csv`** y lo enriquece con los archivos TTL generados previamente:

- **`aduanas.ttl`**  
- **`importadores.ttl`**  
- **`naves.ttl`**  
- **`paises.ttl`**  
- **`puertos.ttl`**  

El resultado final se consolida en el archivo **`importaciones.ttl`**, disponible en el dominio:  
[https://deinsoluciones.cl/web_semantica/](https://deinsoluciones.cl/web_semantica/)

---

### 2. Ejecución de Consultas SPARQL

Para este paso, se deben cargar los siguientes archivos TTL en endpoints distintos:

- **`importaciones.ttl`** debe estar cargado en el endpoint **`localhost:3030`**.  
- **`marcas_importaciones.ttl`** debe estar cargado en el endpoint **`localhost:3031`**.  

Las consultas necesarias se encuentran almacenadas en la ruta:  
**`02/04_querys`**

---

### 3. Evidencias de Validación Formatos Shex y SHACL

En esta sección se incluye evidencia del proceso de validación mediante ejemplos de Shape Expressions (ShEx) y SHACL:

- **Carpeta `03/01_shex_example`**: Contiene ejemplos de validación mediante ShEx.  
- **Carpeta `03/02_shacl_example`**: Contiene ejemplos de validación mediante SHACL.  

---

### 4. Prompts Generados y Validación con LLM

En la carpeta **`04`** se encuentran los prompts utilizados y la consulta de validación. Esta consulta permite comparar los resultados obtenidos con las respuestas generadas por el modelo de lenguaje (LLM).

--- 
## Presentación del Proyecto
- **Versión Oficial Entregable**: https://www.youtube.com/watch?v=61i2uG9bsK4
- **Versión Extendida**: https://www.youtube.com/watch?v=13ALPZiNtj0 
