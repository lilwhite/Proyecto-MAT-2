# Ejercicio 1
El inicio de sesión de cualquiera de estos usuarios debe quedar registrado y los eventos de inicio de sesión correspondientes deben ser visibles en un Workspace de OMS. Además, siempre que un usuario de sistemas inicie sesión debe enviarse un correo electrónico al administrador global.

Primero procederemos a crear el Workspace sobre el grupo de recursos:

<a><img src="https://i.imgur.com/C6mEn9jh.png?1" title="source: imgur.com" /></a>

Una vez creado realizar la búsqueda sobre los "Signing Logs" de los usuarios de sistemas, para lo que ejecutaremos la siguiente Query

```Kusto
SigninLogs
| where Identity like "sys"
```
