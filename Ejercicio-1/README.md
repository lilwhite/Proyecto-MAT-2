# Ejercicio 1
El inicio de sesión de cualquiera de estos usuarios debe quedar registrado y los eventos de inicio de sesión correspondientes deben ser visibles en un Workspace de OMS. Además, siempre que un usuario de sistemas inicie sesión debe enviarse un correo electrónico al administrador global.

## Query Usuarios de Sistemas

Primero procederemos a crear el Workspace sobre el grupo de recursos:

<p align="center">
  <a><img src="https://i.imgur.com/C6mEn9jh.png?1" title="Workspace" /></a>
</p>

Para que recoja los datos de los accesos, deberemos de recopilarlos a través de "Log Analytics", por lo que nos dirigiremos a Azure Active Directory, al apartado de "Sign-Ins":

<a align="center"><img src="https://i.imgur.com/hR8Krla.png" title="Sign-Ins" /></a>

A continuación exportaremos estos datos desde "Export Data Settings" a nuestro Workspace de Log Analytics:

<p align="center">
  <a><img src="https://i.imgur.com/EB8LqcS.png" title="DataSettings" /></a>
</p>

Una vez que se exporten los datos (puede que tarde un tiempo), realizaremos la búsqueda sobre los "Signinlogs" de los usuarios de sistemas, para lo que ejecutaremos la siguiente Query:

```Kusto
SigninLogs
| where Identity like "sys"
```
<p align="center">
<a><img src="https://i.imgur.com/J5QAypzh.png" title="Logs" /></a>
</p>

Con ello, lograremos que nos aparezcan los usuarios de sistemas que han accedido a nuestro dominio.

## Creación de alerta

Una vez que tenemos la query seleccionada, pulsaremos en crear nueva alerta.

<p align="center">
  <a><img src="https://i.imgur.com/bz93Rzbh.png" title="alerta" /></a>
</p>

Haciendo esto, la alerta la activará a la hora de recibir cualquier registro de un usuario de sistemas en nuestro sistema.

<p align="center">
<a><img src="https://i.imgur.com/ugiWaMqh.png" title="condicion" /></a>
</p>

El grupo de acción que tenemos creado para asociar a la alerta, nos enviará un correo por cada registro creado:

<p align="center">
<a><img src="https://i.imgur.com/Cxx5tSjh.png" title="registro" /></a>
</p>

## Prueba de envío de correo

A continuación veremos la rececpción del correo al generarse una de las alertas. Accederemos con uno de los usuarios creados para el departamento de sistemas:

<p align="center">
  <a><img src="https://i.imgur.com/5DDqD4Xh.png" title="sysmat" /></a>
</p>

Pasado un tiempo comprobaremos que recibimos un mensaje en nuestro buzón de correo:

<p align="center">
  <a><img src="https://i.imgur.com/w7G7wMq.png" title="correo" /></a>
</p>
