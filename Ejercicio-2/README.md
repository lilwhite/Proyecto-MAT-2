# Ejercicio 2

La  organización  quiere  publicar  un  vídeo  corporativo  y  para  esto  va  a  utilizar  Azure Media Services. Debes implementar la infraestructura necesaria para que este vídeo pueda  ser  visualizado  desde  cualquier  dispositivo  (incluyendo  dispositivos  móviles) mediante   Azure  Services.  La  empresa   quiere   llevar  un  control  del  número  de visualizaciones del vídeo que se han llevado a cabo.

Para llevar a cabo el siguiente ejercicio se ha seguido el tutorial de la documentación de micrososft "Hacer streaming de archivos de vídeo mediante CLI"

https://docs.microsoft.com/es-es/azure/media-services/latest/stream-files-cli-quickstart

## Creación de cuenta Media Service

Azure Media Servie, nos permitirá reunir una serie de soluciones para la condificación, empaquetado, protección del contenido y difusión de eventos en directo. También se integra perfectamente con Azure CDN, que en nuestro caso utilizaremos los servicios de Verizon.

### Creación de grupo de recursos

```PowerShell
az group create -n RG-Ejercicio2 -l eastus
```

<p align="center">
<a><img src="https://i.imgur.com/4y31fV4h.png" title="recurso" /></a>
</p>

### Creación de cuenta de almacenamiento

Dentro de nuestro grupo de recursos, crearemos una nueva cuenta de almacenamiento que nos servirá para almacenar los vídeos que vayamos a publicar, y como contenedor para la condificación del Media Service. Intentaremos crear todos los recursos dentro de la misma región para reducir los costes del tráfico generado.

```PowerShell
az storage account create -n samat02t38w03ams --kind StorageV2 --sku Standard_LRS -l eastus -g RG-Ejercicio2
```
<p align="center">
<a><img src="https://i.imgur.com/8y8YIRQh.png" title="cuenta almacenamiento" /></a>
</p>

### Creación de una cuenta de Azure Media Services

```PowerShell
az ams account create --n amsmat02t38w03 -g RG-Ejercicio2 --storage-account samat02t38w03ams -l eastus
```
<p align="center">
<a><img src="https://i.imgur.com/a31xoiHh.png" title="AMS" /></a>
</p>

Como vemos en la salida, el comando "ams" se encuentra en preview, por lo que en un futuro es posbile que sufra algún cambio.

### Inicio del punto de conexión de streaming

Antes de crear el punto de conexión debemos de tener en cuenta que se nos pide que se pueda visualizar el número de visualizaciones que se han llevado a cabo, por lo que deberemos de publicar los servicios de CDN de Premium de Verizon, que son los que nos permiten mostrar esta información. Para ello nos dirigiremos a la cuenta de AMS y crearemos un nuevo "streaming endpoint".

<p align="center">
  <a><img src="https://i.imgur.com/itqkmlMh.png" title="source: imgur.com" /></a>
</p>

<p align="center">
  <a><img src="https://i.imgur.com/fEcwdFih.png" title="source: imgur.com" /></a>
</p>

```PowerShell
az ams streaming-endpoint start  -n default -a amsaccount -g RG-Ejercicio2
```
