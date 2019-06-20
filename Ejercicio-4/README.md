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
