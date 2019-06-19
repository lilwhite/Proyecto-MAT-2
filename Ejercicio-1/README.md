# Ejercicio 1
El inicio de sesión de cualquiera de estos usuarios debe quedar registrado y los eventos de inicio de sesión correspondientes deben ser visibles en un Workspace de OMS. Además, siempre que un usuario de sistemas inicie sesión debe enviarse un correo electrónico al administrador global.

## Query Usuarios de Sistemas

Primero procederemos a crear el Workspace sobre el grupo de recursos:

<a align="center"><img src="https://i.imgur.com/C6mEn9jh.png?1" title="Workspace" /></a>

Para que recoja los datos de los accesos, deberemos de recopilarlos a través de "Log Analytics", por lo que nos dirigiremos a Azure Active Directory, al apartado de "Sign-Ins":

<a align="center"><img src="https://i.imgur.com/hR8Krla.png" title="Sign-Ins" /></a>

Una vez creado, realizaremos la búsqueda sobre los "Signinlogs" de los usuarios de sistemas, para lo que ejecutaremos la siguiente Query:

```Kusto
SigninLogs
| where Identity like "sys"
```
<a align="center"><img src="https://i.imgur.com/J5QAypzh.png" title="Logs" /></a>

Con ello, lograremos que nos aparezcan los usuarios de sistemas que han accedido a nuestro dominio.

## Creación de alerta

Una vez que tenemos la query seleccionada, pulsaremos en crear nueva alerta.

<a align="center"><img src="https://i.imgur.com/bz93Rzbh.png" title="alerta" /></a>
