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
  <a><img src="https://i.imgur.com/itqkmlMh.png" title="streaming" /></a>
</p>

<p align="center">
  <a><img src="https://i.imgur.com/fEcwdFih.png" title="endpoint" /></a>
</p>

Una vez que tengamos el endpoint ya creado, podemos iniciarlo:

```PowerShell
az ams streaming-endpoint start -n corporate -a amsmat02t38w03 -g RG-Ejercicio2
```
<p align="center">
  <a><img src="https://i.imgur.com/lRgwFFkh.png" title="startJob" /></a>
</p>

### Creación de una transformación para la codificación de velocidad de bits adaptable

Mediante la transformación lograremos realizar la codificación del vídeo para su correcta publicación:

```PowerShell
az ams transform create --name testEncodingTransform --preset AdaptiveStreaming --description 'a simple Transform for Adaptive Bitrate Encoding' -g RG-Ejercicio2 -a amsmat02t38w03
```
<p align="center">
  <a"><img src="https://i.imgur.com/mUvCDhYh.png" title="transform" /></a>
</p>

### Creación de un recurso de salida

Una vez que tengamos el trabajo de codificación deberemos de darle una salida dentro del Media Service.

```PowerShell
az ams asset create -n testOutputAssetName -a amsmat02t38w03 -g RG-Ejercicio2
```

<p align="center">
  <a><img src="https://i.imgur.com/lQXQvZsh.png" title="assets" /></a>
</p>

Con ello, comprobaremos que se crea un contenedor dentro de nuestra cuenta de almacenamiento, que contendrá un contenedor privado, para la posterior codificación del vídeo.

<p align="center">
  <a><img src="https://i.imgur.com/fsL1jAPh.png" title="assets1" /></a>
</p>

Posteriormente, crearemos un nuevo contenedor (tipo container) dentro de nuestra cuenta de almacenamiento para subir el video que queremos publicar.

<p align="center">
  <a><img src="https://i.imgur.com/GUaMjjch.png" title="videos" /></a>
</p>

### Inicio de un trabajo mediante la entrada HTTPS

A continuación le indicaremos a la cuenta de Media Service, que queremos iniciar el trabajo especificándole la URL del video de nuestro contenedor.

<p align="center">
  <a><img src="https://i.imgur.com/U4kPaIHh.png" title="container" /></a>
</p>

```PowerShell
az ams job start --name testJob001 --transform-name testEncodingTransform --base-uri 'https://samat02t38w03ams.blob.core.windows.net/videos/' --files 'vaporwave.mp4' --output-assets testOutputAssetName= -a amsmat02t38w03 -g RG-Ejercicio2
```
Nos devolverá la siguiente salida:

<p align="center">
  <a><img src="https://i.imgur.com/gi2a1Bdh.png" title="job" /></a>
</p>

Si nos dirigimos a la cuenta del Media Service, en "Jobs" podemos observar el estado del proceso:

<p align="center">
  <a><img src="https://i.imgur.com/A5hFxueh.png" title="estado" /></a>
</p>

Una vez finalizado el proceso, podremos observar en el contenedor de "assets" cómo se ha realizado la codificación del video:

<p align="center">
  <a><img src="https://i.imgur.com/WEBA0Wgh.png" title="codificacion" /></a>
</p>

### Creación de un localizador de streaming y obtención de la ruta de acceso

A continuación deberemos de poner el vídeo en un recurso de salida para que pueda ser consumido:

```PowerShell
az ams streaming-locator create -n testStreamingLocator --asset-name testOutputAssetName --streaming-policy-name Predefined_ClearStreamingOnly  -g RG-Ejercicio2 -a amsmat02t38w03
```
<p align="center">
  <a><img src="https://i.imgur.com/9X2kHP7h.png" title="source:salida" /></a>
</p>

### Obtención de las rutas de acceso del localizador de streaming

```PowerShell
az ams streaming-locator get-paths -a amsmat02t38w03 -g RG-Ejercicio2 -n testStreamingLocator
```
<p align="center">
  <a><img src="https://i.imgur.com/ie9jXbqh.png" title="source:locator" /></a>
</p>

Copiaremos la ruta de acceso a streaming en vivo (HLS), en este caso:

```PowerShell
/5c6be9b8-f606-4af8-adcf-07bdc8a42f0c/vaporwave.ism/manifest(format=m3u8-aapl)
```

### Creación de la dirección URL

Deberemos obtener el nombre del host del punto de conexión del streaming:

<p align="center">
  <a><img src="https://i.imgur.com/QQQBAN8.png" title="hostname" /></a>
</p>

```PowerShell
corporate-amsmat02t38w03-usea.streaming.media.azure.net
```

#### Ensamblado de la dirección URL

"https:// " + <valor de hostName> + <valor de ruta de acceso de HLS>

```PowerShell
https://corporate-amsmat02t38w03-usea.streaming.media.azure.net//5c6be9b8-f606-4af8-adcf-07bdc8a42f0c/vaporwave.ism/manifest(format=m3u8-aapl)
```
