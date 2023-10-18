# Github

Plataforma de automatizacion que nos permite crear flujos de trabajo de codigo personalizado dentro de nuestros equipos

Github provee herramientas de :

- seguridad
- auto completado de codigo mediante copilot
- code reviews en cada Pull Rquest

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


## MarketPlace

existen plantillas de diferentes action que se peuden incorporar a los projectos en el link acontinuacion
 https://github.com/marketplace?type=


# Integracion AWS 

En el siguiente ejemplo se muestra como incorporar en el workflow de github action el despliegue del dockerfile   donde al momento de un push ejecuta la tarea create-docker-image.

```yml

name: Deploy to production
 
on:
  push:
    branches:
      - master
 
jobs:
  create-docker-image:
    name: Build and push the Docker image to ECR
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repository
        uses: actions/checkout@v3
 
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1-node16
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
 
      - name: Log into the Amazon ECR Public
        id: login-ecr-public
        uses: aws-actions/amazon-ecr-login@v1
        with:
          registry-type: public
 
      - name: Build, tag, and push docker image to Amazon ECR Public
        env:
          REGISTRY: ${{ steps.login-ecr-public.outputs.registry }}
          REGISTRY_ALIAS: e2b3j8w6
          REPOSITORY: nestjs-api
          IMAGE_TAG: nestjs-api
        run: |
          docker build -t $IMAGE_TAG .
          docker tag $IMAGE_TAG:latest $REGISTRY/$REGISTRY_ALIAS/$IMAGE_TAG:latest
          docker push $REGISTRY/$REGISTRY_ALIAS/$REPOSITORY:latest
 
  deploy:
    name: Deploy the new Docker image to ECS
    runs-on: ubuntu-latest
    needs: create-docker-image
    steps:
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1-node16
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-central-1
 
      - name: Update ECS service
        run: |
          aws ecs update-service --cluster nest_cluster --service nestjs_service --task-definition nest_task --force-new-deployment
```

# Costos

El uso de github actions en repositorios publicos no tiene ningun costo , mientras que en repositorios privados estan contemplados los costos en este archivo https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions 