# EJERCICIO 3

Dos máquinas virtuales en Azure implementarán una carga de trabajo crítica para la organización,  por  lo  que  es  imprescindible  contar  con  estas  máquinas  replicadas  en otra  región.  Cuando  una  de  estas  máquinas  falle,  su  réplica  deberá  iniciarse  en  la región de respaldo de forma automática

## Disaster Recovery

Primero procederemos a crear una máquina virtual con Windows Server en la región de Francia Central. Debemos de tener en cuenta que para la creación del "Disaster Recovery" las versiones de Ubuntu no son compatibles.

Una vez que tengamos la máquina virtual creada nos dirigiremos al apratado de "Disaster Recovery" seleccionando la región deseada para realizar el Recovery:

<p align="center">
  <a><img src="https://i.imgur.com/r5y9PBkh.png" title="region" /></a>
</p>

Una vez finalizado el proceso de replicación nos tendrá que aparecer que es estado es "Healthy"

<p align="center">
  <a><img src="https://i.imgur.com/6yT8OXhh.png" title="secured" /></a>
</p>
