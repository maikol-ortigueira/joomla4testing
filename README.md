# Joomla Ready for Testing - Boilerplate

## Desarrollo de extensiones para Joomla con entorno de pruebas

### Contenido

- docker-compose con contenedores para iniciar una instalación limpia de Joomla, útil para el desarrollo de extensiones personalizadas. Los contenedores que se inician son:
    - MySql para los datos de Joomla
    - PhpMyadmin con acceso a la base de datos de Joomla
    - Servidor Apache para la instalación de Joomla
- Carpeta test para automatizar las pruebas. Actualmente he añadido cypress, partiendo de la configuración utilizada por el equipo de desarrollo de Joomla, realizando unas pequeñas modificaciones para adaptar a este repositorio.
- Carpeta extension2joomla. Esta carpeta es una modificación de mi repositorio para desarrollo de extensiones en joomla. Permite añadir las carpetas con el código de mis extensiones en una ruta fuera del árbol de ficheros de joomla. Los beneficios son que puedo tener agrupado todo el código de mis extensiones en una carpeta que fácilmente puedo añadir a un repositorio.

## Requisitos

- SO Ubuntu 22.04, aunque no he realizado pruebas en otros sistemas operativos. Debería funcionar en MAC
- Nodejs (https://nodejs.org/)
- Make (https://www.gnu.org/software/make/)
- Docker (https://docs.docker.com/desktop/)
- docker-compose (https://docs.docker.com/compose/install/). También funciona con "docker compose"
- Gulp

## Uso

Antes de nada copia y pega el fichero de environments, y adapta a tu configuración:

```bash
cp .env.dist .env
```

Es necesario cargar los paquetes de node con el comando:

```bash
npm ci
```


El fichero Makefile contiene una serie de scripts que permiten automatizar muchas cosas. Para saber las posibles opciones solo tienes que escribir en línea de comandos:

```bash
make help
```

Mostrará la lista de opciones con un pequeño resumen por opción.

## Levantar servidor

Con el comando `make up` realiza una instalación de joomla en la versión y con los datos expecificados en el fichero `.env`. Para una primera prueba no es necesario modificar ningún dato del fichero `.env`

## Desarrollo de extensiones Joomla

Para el desarrollo de extensiones Joomla! deberás preparar tu entorno teniendo en cuenta lo siguiente:

* Tus extensiones deben estar ubicadas en la carpeta `/joomla_extensions/`
* La estructura de ficheros dentro de esta carpeta será el siguiente:

```bash
.
├── components
│   └── <com_name>
│       ├── admin
│       │   ├── access.xml
│       │   ├── config.xml
│       │   ├── forms
│       │   ├── language
│       │   ├── layouts
│       │   ├── services
│       │   ├── sql
│       │   ├── src
│       │   └── tmpl
│       ├── media
│       │   ├── css
│       │   ├── images
│       │   ├── joomla.asset.json
│       │   └── js
│       ├── <com_name>.xml
│       └── site
│           ├── layouts
│           ├── src
│           └── tmpl
├── plugins
│   └── [ system|user|content|... ]
│       └── <plugin_name>
│           ├── language
│           ├── services
│           ├── src
│           └── <plugin_name>.xml
└── templates
    └── [ admin|site ]
        └── <template_name>
            ├── media
            ├── language
            ├── component.php
            ├── error.php
            ├── index.php
            ├── joomla.asset.json
            ├── offline.php
            ├── template_preview.png
            ├── template_thumbnail.png
            └── templaDetails.xml
```

### Renombrar y configurar `extensions-config.json`

Copiar y renombrar el fichero `extensions-config.json.dist`

```bash
cp ./extensions-config.json.dist ./extensions-config.json
```

Es necesario configurar el fichero teniendo en cuenta las extensiones en desarrollo. Actualmente el fichero de ejemplo está configurado para mapear 2 componentes (foo, bar), un plugin de sistema (bar), y una plantilla para el front de Joomla (foo_template). Las extensiones de ejemplo deberán borrarse de la configuración para evitar error.

El resto de extensiones se configura del mismo modo.

**IMPORTANTE:** Solo se mapearán las extensiones que se registren en este fichero de configuración, y si se registra alguna extensión que no tenga correspondencia en la carpeta `joomla_extensions`, siguiendo la estructura de carpetas propuestas, lanzará un error al querer mapear las extensiones.

## Posibles incidencias

- Antes de levantar contenedores deberás comprobar que el puerto 80 de tu sistema está libre. Puedes comprobar con el siguiente comando en Ubuntu:
```bash
ss -ltn | grep ':80'
```

## Testing

Para realizar test e2e me he decidido por Cypress. Toda la información en [Cypress para joomla4testing](./test/README.md)
