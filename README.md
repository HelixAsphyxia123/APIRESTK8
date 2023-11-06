# API REST RUST CON K8
# Guía de Configuración y Despliegue de Aplicación en Kubernetes con Minikube

![Kubernetes](kubernetes-logo.png)

Esta guía proporciona los pasos detallados para configurar y desplegar una aplicación en un clúster local de Kubernetes utilizando Minikube. La aplicación de ejemplo es una API REST desarrollada en Rust, y se utiliza un Ingress Controller para redirigir el tráfico desde un dominio personalizado a la aplicación.

## Requisitos

Antes de comenzar, asegúrate de tener instalados los siguientes requisitos en tu máquina:

- Minikube
- Kubectl
- Docker (si deseas construir tus propias imágenes de Docker)
- Un entorno de desarrollo Rust configurado (si la aplicación es en Rust)

## Pasos

### 1. Configuración de Minikube

Asegúrate de que Minikube esté instalado y en funcionamiento. Puedes verificar su estado ejecutando:

```bash
minikube status
```
### 2. Iniciar el Clúster de Minikube
Si Minikube no está en funcionamiento, inícialo con el siguiente comando:
```bash
minikube status
```
### 3. Crear un Despliegue y un Servicio para la Aplicación
Para crear un despliegue y un servicio para tu aplicación en Kubernetes, utiliza los siguientes comandos. Asegúrate de que la aplicación esté en un contenedor Docker antes de ejecutar este paso.
```bash
kubectl create deployment rustapp-deployment --image=tu-imagen-docker:etiqueta
kubectl expose deployment rustapp-deployment --name=rustapp-service --type=ClusterIP --port=8080
```
### 4. Configuración de Ingress
Crea un recurso de Ingress para configurar el enrutamiento desde un dominio personalizado a tu servicio de la aplicación. Reemplaza <tudominio> por tu dominio personalizado.
```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api-ingress
spec:
  rules:
  - host: api.<tudominio>.com
    http:
      paths:
      - path: /icecreams
        pathType: Prefix
        backend:
          service:
            name: rustapp-service
            port:
              number: 8080
```
Después, aplica la configuración del Ingress:
```bash
kubectl apply -f nombre-del-archivo-ingress.yaml
```
### 5. Configuración de DNS
Edita tu archivo hosts en tu sistema para mapear el dominio personalizado a la IP de Minikube:
```bash
127.0.0.1    api.<tudominio>.com
```
### 6.Monitorización
Puedes utilizar Kubectl para supervisar el estado de tus recursos:
```bash
kubectl get pods
kubectl get services
kubectl describe ingress api-ingress
```
### Contribución
Si deseas contribuir a esta guía o mejorar la configuración, ¡estamos abiertos a tus sugerencias y aportaciones!

### Licencia
```bash
Asegúrate de personalizar los marcadores de posición (`<tudominio>`, `tu-imagen-docker:etiqueta`, etc.) con la información específica de tu aplicación y dominio. También, si tienes requisitos adicionales o información importante que deba incluirse, no dudes en agregarla al README.

Espero que esta guía te sea útil y te ayude a documentar y compartir tus configuraciones de Kubernetes con Minikube.
```
