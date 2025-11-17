---
Name: 6 - Nuget
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN**
*****
>Gestor de bibliotecas de paquetes, trabaja con packages y no con librerías individuales.

Busca paquetes en repositorios locales y/o web. Gestiona conflictos de versiones y/o dependencias.

**BÚSQUEDA E INSTALACIÓN DE PAQUETES**
****
***A nivel de solución***
El paquete se descarga en el directorio `packages`. Antes de la compilación se comprobará si todos los paquetes están correctamente descargados.

***A nivel de proyecto***
Se creará o actualizará el fichero `packages.config` en el que se indican qué referencias es necesario añadir para ejecutar el proyecto. Único fichero que debería estar ubicado en el repositorio SVN, GIT...
Se modificará el fichero de configuración (`app.config` / `web.config`).