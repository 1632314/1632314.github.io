---
title: Web GitHub
date: 2025-01-19
draft: true
tags:
  - GitHub
  - web
  - cloudflare
---
# Utilizar *GitHub Pages* para hostear tu propia web
## Introducción
Siempre he querido tener una página web sencillita donde ir posteando **fácilmente** (enfasis en esto) artículos. Hoy en día es más fácil que antaño, pero se deben tener en cuenta muchas cosas que no són evidentes como certificados SSL, dominio o hosting. Por suerte tenemos una forma de no tener que lidiar con ello a través de GitHub. Para ello utilizaremos un renderizador de páginas web estáticas, en mi caso es Hugo, pero se pueden utilizar otros sin problema. También se puede añadir el paso opcional de utilizar un dominio propio, la única parte que cuesta dinero, pero al cabo del año no es nada.

Para hacer un adelanto de lo que será. El workflow será el siguiente. Tendremos un repositorio que contendrá nuestras páginas web. En este repositorio subiremos y modificaremos los *posts* o páginas que querramos. Una vez subido a GitHub, a través	de GitHub Actions, se ejecutará un *script* para renderizar y subir nuestra página web a GitHub Pages, de manera completamente automática. Hay muchas tecnologías implicadas, pero el resultado final es algo muy cómodo (solo si eres programador😅). Para personas que nunca han programado esto debe ser complicado y poco intuitivo. 

## Repositorio de GitHub
### Creación
Primerisimo, tener una cuenta de GitHub, pero si estas aquí seguramente ya tengas una. Si no es así aprovecha para crearte una, es gratis 😉. Ahora solo debemos crear un repositorio de GitHub. Podemos hacerlo a través de nuestro IDE favorito o a través de la página web. 

A la hora de crear el repositorio es conveniente ponerle el nombre de `<tu nombre de usuario>.github.io`. De esta manera el enlace a la web será más corto, es solo un detalle no es obligatorio. Lo que es más importante es que si no tienes una cuenta premium hagas el repositorio público, sino no te dejará publicarlo en GitHub Pages. El README es opcional, si pretendes compartir o enseñar el repositorio seria conveniente tenerlo. A la hora de seleccionar la plantilla del `.gitignore` selecciona GitHubPages, será conveniente modificarlo, pero sirve de base. Licencia la que quieras.

![Pantalla de creación de nuevo repositorio en GitHub](new-repository-github-page-censored.PNG "Utiliza la imagen como guia")

### Workflow 
Aunque no tengamos nada publicado en GitHub podemos dejar esto listo para más adelante. Vamos a aprovechar la funcionalidad de GitHub Actions para ejecutar un *script* cada vez que publiquemos en la rama principal que se encargue de publicar los cambios. 

Para cambiar el funcionamiento de la publicación vamos a `Ajustes > Pages` desde nuestro repositorio. Una vez allí marcamos como fuente GitHub Actions.  

![Selecciona GitHub Actions como forma de publicación](select-build-github.png)

Si no tienes ningún workflow creado tienes la opción de seleccionar una plantilla. En mi caso busco la plantilla de Hugo.

![Búsqueda plantilla hugo workflow github](hugo-workflow-github.png)

Una vez seleccionada podemos hacer un *commit* directamente pues no vamos a tocar nada de aquí. Al menos yo no.

Puedes aprovechar para modificar el fichero `.gitignore` para añadir una nueva línea con `/public/`, esto evitará que tus pruebas locales se suban al repositorio.

## Hugo
Ahora necesitamos instalar Hugo. Hugo es un renderizador de páginas web. ¿Esto que es? Pues es un programa que recoge una serie de ficheros que por si mismos no son una página web y genera unos que si lo son. En nuestro caso Hugo coge unos ficheros en markdown y los transforma a html. 

### Instalación
Para instalarlo la forma más sencilla que se me ocurre es a través del terminal. Si estas en Linux debería ser tan fácil como 
```bash
sudo apt install hugo
``` 
o con el gestor de paquetes que te toque. Si estas en MAC tendrás que utilizar el gestor de paquetes brew.

```shell
brew install hugo
```

En Windows también tenemos la opción de utilizar un gestor de paquetes que es `winget`. Si vas al terminal y escribes 

```
winget install Hugo.Hugo.Extended
```

debería instalarse. 

Si no te funciona puedes revisar la [documentación de hugo](https://gohugo.io/categories/installation/).


### Creación del sitio
Antes de seguir deberías tener instalado git, puedes mirar en la [página oficial](https://git-scm.com/downloads) como hacerlo para tu sistema operativo, pero me extraña que no lo tengas ya.

Para crear el sitio primero debemos crear el repositorio de git en nuestro ordenador. Para esto es recomendable un IDE y nada de terminal. En mi caso es Visual Studio Code. Conectamos nuestra cuenta de GitHub. Y realizamos un *Git Clone* del repositorio que hemos creado antes. El directorio donde lo hagas da igual, pero un sitio del que te acuerdes, no lo pongas en descargas.

Una vez clonado el repositorio deberíamos tener poco nada y menos. En esta ubicación, donde esta el contenido del repositorio puedes crear la estructura de carpetas de Hugo utilizando:

```shell
hugo new site .
```

Ahora debes seleccionar un estilo o tema para tu página web, puedes ver una selección de ellos en esta [página web](https://themes.gohugo.io/) que los agrupa con capturas de pantalla. 

Una vez hecho esto podemos utilizar una funcionalidad de git tan increíble como desconocida: los submodulos. Un repositorio git dentro de otro. Una vez tengas el estilo decidido lo puedes incluir a tu repositorio mediante:

```shell
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

Y para acabar deberás modificar el archivo de configuración de hugo `hugo.toml` para cambiar el tema. 

```toml
theme = 'anake'
```

Una vez hecho esto deberías poder ejecutar `hugo server` para iniciar un servidor web con el contenido que tengas. 

Para ver los pasos más en detalle tienes una [guía de hugo](https://gohugo.io/getting-started/quick-start/). Para gran parte de la configuración de tu web deberás utilizar la documentación que provea el creador del tema. 

### Creación de contenido
Para crear un nuevo post tienes dos opciones, crear el fichero manualmente o ejecutar el comando `hugo new content /content/posts/my-post.md`. Aquí podrás utilizar las reglas de [sintaxis de markdown](https://www.markdownguide.org/basic-syntax/) para redactar artículos. Puedes incluir metadatos del artículo en la cabezera:

```markdown
+++
title = 'My First Post'
date = 2024-01-14T07:07:07+01:00
draft = true
+++
```

### Actualizar la web
Todo lo que tienes que hacer es hacer un push de tu git local a github. Una vez hecho esto se desencadenará el workflow que hemos definido. Y podrás verlo ejecutandose en la página de GitHub en la pestaña de Actions.  

![Workflow de GitHub Actions](github-actions-workflow.png)

Puedes seleccionar *build* o *deploy* para ver las partes ejecutadas y su resultado. Muy conveniente si se da algún error.

Antes de subir el contenido a la web podeis probar la ejecución en local utilizando el comando:

```sh
hugo server -D
```

Esto renderizará la web y podreis acceder a ella localmente tal y como se veria una vez colgada. Si actualizais un fichero mientras se ejecuta el servidor local actualizará la web automáticamente. Facilita muchisimo el desarrollo pues no teneis que esperar a que se suba a github ni nada. 

## Dominio Propio
Si quereis acceder a vuestra web todo lo que teneis que acceder es ir a `<yourname>.github.io` si le has puesto otro nombre al repositorio que no sea el indicado debereis añadir `\repository-name` al enlace.

Pero si queremos acceder a través de un dominio propio aun quedan unos cuantos pasos. Primero deberemos tener un dominio adquirido, en mi caso [jdrt.dev](https://jdrt.dev). Os recomiendo adquirir dominios con nuevos TLD (*Top Level Domain*) pues podeis adquirir dominios muy cortos, si eres desarrollador puedes adquirir un `.dev`. Para adquirirlo en mi caso utilizo Cloudflare pues puedo hacer cambios de DNS directamente, pero si os quereis hacer con un dominio barato podeis adquirirlo a través de [Namecheap](https://www.namecheap.com) y luego configurar los DNS de Cloudflare.


