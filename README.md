# nido-releases

Manifiesto público de versiones de **nidobox**.

Un nidobox se instala por artefacto (`git archive`), así que **no tiene repositorio**: no puede
preguntarle a git si hay algo más nuevo. Este repositorio es lo único que necesita consultar.

```
releases.json      qué commit es el vigente en cada canal
releases.json.sig  firma ed25519 del archivo de arriba
```

## Por qué está firmado

Sin firma, quien controlara este alojamiento podría decirle a toda la flota cuál es la última
versión. La firma la hace Santiago **a mano**, con una clave privada que no vive en ningún CI ni
en ninguna caja de cliente. La clave pública viaja **dentro de cada release**, así que un equipo
verifica sin consultar a nadie y sin guardar ninguna credencial.

Un manifiesto cuya firma no verifica **no se usa**: el equipo lo dice en alto y se queda con lo
que tenía.

## Verificarlo tú mismo

```bash
curl -fsSO https://raw.githubusercontent.com/SantiagoBP95/nido-releases/main/releases.json
curl -fsSO https://raw.githubusercontent.com/SantiagoBP95/nido-releases/main/releases.json.sig
openssl pkeyutl -verify -pubin -inkey nido-releases.pub -rawin \
  -in releases.json -sigfile releases.json.sig
```

La clave pública está en el repositorio del producto, en `security/nido-releases.pub`.

## Qué NO hay aquí

Ningún dato de ningún cliente, ninguna dirección, ningún secreto. Solo identificadores de commit
y fechas. Es público a propósito: así una caja no necesita credenciales para consultarlo, y una
credencial por equipo es justo lo que este diseño evita.

## Qué hace un equipo con esto

Comparar y **avisar**. Nada más. El nidobox no se actualiza solo: informa a su dueño de que hay
una versión nueva y es él quien decide cuándo aplicarla (ADR-P028).
