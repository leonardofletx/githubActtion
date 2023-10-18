# Github

Plataforma de automatizacion que nos permite crear flujos de trabajo de codigo personalizado dentro de nuestros equipos

Github provee herramientas de :

- seguridad
- auto completado de codigo mediante copilot
- code reviews en cada Pull Rquest
- github actions 

Estas herramientas hacen parte de las practicas de desarrollo llamadas integraciones continuas y despliegues continuos. existen muchas herramientas que permiten el CI/CD como travis, jenkins pero github tiene su propia plataforma llamada action que trabaja de manera nativa con el codigo creado en la platforma de github.

# CI y CD

flujo de desarrollo que acompaña todo el ciclo de vida de un projecto, tenemos continue integration

1. plan
2. code
3. build
4. test

y continue deployment:

1. release
2. deploy
3. operate
4. measure

# Github actions

herramienta de github que nos permite automatizar los procesos de CI/CD de manera nativa

## componentes github action

- workflows : Un Workflow es el proceso más amplio y automatizado que ejecuta uno o varios Jobs. Se define mediante un archivo YAML en la carpeta .github/workflows de cada repositorio y cada repositorio puede tener varios Workflows. (ej: para pasar pruebas y desplegar)

- jobs : Un Job es un conjunto de Steps o tareas que viven dentro de un Workflow. Los Steps de un Job siempre se ejecutan en orden y de forma secuencial, dependiendo del término del anterior.

- steps : Un Step es una parte del Job que puede ser un script de Shell (un comando en la terminal) o un Action predefinido que se ejecuta.

- action : Un Action es una aplicación personalizada que realiza una tarea compleja de forma repetitiva para evitar escribir código repetitivo.

- event : Un Evento es una actividad específica en el repositorio que activa la ejecución de un Workflow ( se internos o externos).

- runner : Un Runner es el servidor donde se ejecutan nuestros workflows. GitHub proporciona runners que tienen diferentes sistemas operativos: Ubuntu, Windows o MacOS.

