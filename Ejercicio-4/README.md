# EJERCICIO 4

Una de las bases de datos que se van a utilizar en la empresa almacenará datos durante
largos periodos de tiempo y con poca frecuencia de acceso. Deberás implementar esta
base de datos mediante la tecnología Stretch Database para que los datos más
antiguos se almacenen en Azure.

## Descarga e instalación de SQL Server 2016

Primero deberemos de realizar la instalación del SQL Server 2016 sobre una máquina on-premises. Para ello realizaremos la descarga desde el siguiente enlace:

https://www.microsoft.com/en-US/download/details.aspx?id=56836

Seleccionaremos la instalación Básica:

<p align="center">
  <a><img src="https://i.imgur.com/WPUYxjNh.png" title="basic" /></a>
</p>

Posteriormente debemos de realizar la instalación del SQL Server Management Studio (SSMS):

https://docs.microsoft.com/es-es/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017

## Importación de Base de datos

Para realizar el ejercicio, descargaremos una base de datos de ejemplo. En este caso, utilizaremos la del siguiente repositorio de GitHub:

https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0

De aquí, nos descargaremos la base de datos "WideWorldImporters-Standard.bak"

A continuación, accederemos a SQL Server Management Studio para realizar la importación de la base de datos. Conectaremos con el servidor local:

<p align="center">
  <a><img src="https://i.imgur.com/zGuUhQ8.png" title="SQL" /></a>
</p>

Seleccionaremos restaurar la base de datos:

<p align="center">
  <a><img src="https://i.imgur.com/reEoOI6.png" title="restore" /></a>
</p>

Buscamos dónde hemos almacenado el archivo .bak:

<p align="center">
  <a><img src="https://i.imgur.com/bEgEH1T.png" title="source: imgur.com" /></a>
</p>

<p align="center">
  <a><img src="https://i.imgur.com/6FQXUr5.png" title="source: imgur.com" /></a>
</p>

## Strech Database

Mediante el Strech Database lograremos seleccionar las tablas que queremos mover a Azure. Para ello lo habilitaremos desde la siguiente ruta:

<p align="center">
  <a><img src="https://i.imgur.com/RUtcrGR.png" title="strech" /></a>
</p>

Seleccionaremos la tabla de "stock" por ejemplo, para realizar la prueba:

<p align="center">
  <a><img src="https://i.imgur.com/uFpWVtM.png" title="source: imgur.com" /></a>
</p>
