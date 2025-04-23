#  Ejercicio #1 – RabbitMQ y procesamiento distribuido

Este proyecto corresponde al **Ejercicio #1** del taller sobre **RabbitMQ y arquitecturas distribuidas**, donde se implementa un sistema sencillo para distribuir tareas entre múltiples trabajadores (workers).

##  Problema planteado

Una empresa de análisis de datos necesita un sistema que distribuya tareas complejas para ser procesadas por múltiples workers en paralelo. Cada tarea tiene un nivel de complejidad (1-5 segundos para simular el trabajo). Se deben cumplir estos requisitos:

- **Ninguna tarea puede perderse**, incluso si falla un worker.
- **Distribución equitativa** de tareas entre workers.
- **Respeto al orden** y evitar sobrecargar workers.

---

##  Nuestra solución (MVP)

Implementamos una arquitectura mínima usando **Docker Compose**, **RabbitMQ** y **Node.js**:

- **Un productor** que publica exactamente **10 tareas** en una cola RabbitMQ.
- **Dos o más workers** consumen tareas en paralelo, simulando el trabajo con un tiempo de espera proporcional al nivel de complejidad.
- Configuración para asegurar que no se pierdan tareas (`acks`, mensajes durables y `prefetch=1`).

---

##  Herramientas usadas

-  Docker y Docker Compose
-  RabbitMQ (imagen oficial con interfaz web)
-  Node.js con la librería `amqplib`

---

## ▶ Cómo ejecutar el proyecto

**1\. Clona el repositorio**

```bash
git clone <URL_DE_TU_REPOSITORIO>
cd ejercicio-1-rabbitmq
```

**2\. Ejecuta la infraestructura**

```bash
docker-compose up --build --scale worker=2
```

**3\. Observa resultados**

- Los logs mostrarán cómo se distribuyen y procesan tareas.
- Verifica la interfaz web de RabbitMQ:  
  [http://localhost:15672](http://localhost:15672)  
  **Usuario:** `guest`, **Contraseña:** `guest`.

---

##  Probando la tolerancia a fallos

Puedes simular la caída de un worker y observar cómo RabbitMQ reenvía automáticamente la tarea pendiente al otro worker activo:

```bash
docker-compose kill -s SIGKILL ejercicio-1-rabbitmq_worker_1
```

---

##  Estructura del proyecto

```text
ejercicio-1-rabbitmq/
├── docker-compose.yml
├── producer/
│   ├── Dockerfile
│   ├── package.json
│   └── producer.js
└── worker/
    ├── Dockerfile
    ├── package.json
    └── worker.js
```

---

##  Entregables incluidos

- Código funcional de productor y workers.
- Evidencia de tareas distribuidas en logs.
- Prueba de tolerancia a fallos.
- Explicación simple del flujo de trabajo en este README.
