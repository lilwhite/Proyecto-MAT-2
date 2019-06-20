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
