## UNIVERSIDAD SAN CARLOS DE GUATEMALA ##
## FACULTAD DE INGENIERIA ##
## ESCUELA DE CIENCIAS Y SISTEMAS ##
## Sistemas operativos 1##
## SECCIÓN A ##

### Actividad 1 ###

- **Nombre:** Mynor Francisco Morán García   **Carne:** 201603232

>>


# Desplegar un Servidor Web (NGINX o Apache) en Kubernetes con Docker Desktop

## 1. Asegurarse de que Kubernetes está habilitado en Docker Desktop

1. Abre **Docker Desktop**.
2. Ve a **Settings (Configuración)** > **Kubernetes**.
3. Asegúrate de que la opción **Enable Kubernetes** está activada.
4. Aplica los cambios y espera a que Kubernetes esté en ejecución.

## 2. Crear un Deployment para NGINX o Apache

Crea un archivo YAML llamado `nginx-deployment.yaml` con el siguiente contenido:

### NGINX Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer

```
## 3. Desplegar en Kubernetes

Aplica el archivo YAML para desplegar el servidor NGINX en el clúster de Kubernetes de Docker Desktop:

```bash
kubectl apply -f nginx-deployment.yaml
```

## ¿En un ambiente local de Kubernetes existen los nodos masters y workers, como es que esto funciona?

### Clúster de un solo nodo (Single-node cluster):
En un entorno local, Kubernetes suele ejecutarse como un **clúster de un solo nodo**. Esto significa que hay un único nodo que asume las responsabilidades tanto del **control plane** (que en un clúster distribuido se ejecuta en los nodos master) como de los **nodos de trabajo (workers)**.

**Docker Desktop** y **Minikube** ejecutan un único nodo que actúa como **nodo master** y **nodo worker** al mismo tiempo. Todas las funciones de administración del clúster, como la asignación de pods, el monitoreo del estado del clúster y la orquestación, ocurren en este único nodo.

### Componentes del Control Plane en el nodo único:
- **API Server**: El punto de entrada para todos los comandos de administración de Kubernetes, gestionado a través de `kubectl` u otras herramientas.
- **etcd**: La base de datos clave-valor que almacena el estado del clúster. En un clúster local, el nodo único mantiene el estado de todo el clúster.
- **Scheduler**: Responsable de asignar los pods a los nodos. En un entorno local, dado que hay un solo nodo, el scheduler siempre asigna los pods a este nodo.
- **Controller Manager**: Se encarga de la gestión de los controladores que ejecutan las tareas automatizadas (como escalar o reparar nodos/pods).

### Nodos Worker en un clúster local:
En un entorno de producción, los **nodos worker** ejecutan los pods. En un clúster local, el único nodo cumple esta función también. Este nodo único ejecuta tanto los procesos del plano de control como los contenedores que forman parte de las aplicaciones que desplegamos.

- **Kubelet** y **Kube-proxy** se ejecutan en el mismo nodo:
  - **Kubelet**: Responsable de asegurar que los contenedores estén corriendo en el nodo.
  - **Kube-proxy**: Gestiona el enrutamiento de red y la exposición de servicios.

### Ejecución en Docker Desktop o Minikube:
- En **Docker Desktop**, cuando habilitas Kubernetes, Docker crea un clúster local de un solo nodo que ejecuta todos los componentes en un contenedor. Este contenedor simula un ambiente Kubernetes completo, pero dentro de tu máquina local.
- **Minikube** hace algo similar, pero crea una máquina virtual que actúa como un nodo Kubernetes (o varios nodos si lo configuras para ello).

### Resumen de Funcionamiento:
Aunque en un clúster local no tienes físicamente separados los **nodos master** y **nodos worker**, sí funcionan conceptualmente. El único nodo en tu clúster asume ambos roles.

Los componentes de Kubernetes (API Server, Scheduler, Controller Manager, etc.) están ejecutándose en el mismo nodo que también ejecuta los contenedores de tus aplicaciones.