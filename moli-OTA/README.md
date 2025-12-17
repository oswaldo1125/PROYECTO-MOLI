# MOLI Core: Módulo de Optimización, Longevidad e Innovación

## MOLI-OTA: Prototipo Funcional (Fase 1 de Desarrollo)

Este proyecto inicial es la base de la plataforma **MOLI Core**. Su objetivo es validar la lógica de las Actualizaciones de Firmware Over-The-Air (OTA) a través de dos canales independientes: **Wi-Fi** y **Bluetooth Low Energy (BLE)**.

Una vez validada la funcionalidad en este *sketch*, el código será migrado a una librería de C++ modular (Fase 2) que permitirá una implementación rápida y profesional en futuros proyectos.

---

### I. Propuesta de Valor y Objetivos

El módulo MOLI-OTA resuelve el principal desafío de *DevOps* en Sistemas Embebidos: **la dependencia de la red**. Al ofrecer un sistema OTA Dual, garantizamos:

1.  **Resiliencia:** Si el Wi-Fi falla, el BOTA (Bluetooth OTA) permite la corrección de errores en campo.
2.  **Optimización del Desarrollador:** Elimina la necesidad de adaptadores y cables durante la fase de *debugging* y pruebas (nuestro objetivo inicial).

### II. 🛠️ Arquitectura y Diseño (Fase 1)

Este prototipo funcional está diseñado bajo un principio clave: **Separación de Capas.**

* **Capa de Transporte:** Manejada por las rutinas de Wi-Fi (`ArduinoOTA`) y Bluetooth (lógica de recepción GATT).
* **Capa de Escritura:** Una única función central de bajo nivel es responsable de escribir los datos binarios en la *flash*, desacoplando la lógica de la red.

### III.  Consideraciones de Operación y Activación

Las funcionalidades OTA consumen una alta demanda de recursos (CPU, memoria) en el ESP32, por lo que es esencial activarlas **solo bajo demanda**.

Para este propósito, se han designado comandos de activación que permiten al desarrollador seleccionar el modo de transporte deseado:

#### Comandos de Activación (Vía Monitor Serial):

| Comando | Función | Estado Esperado |
| :--- | :--- | :--- |
| **`OTA_WIFI`** | Activa el modo OTA a través de la red local (Wi-Fi). | El ESP32 se conecta al *router* y espera una subida desde el IDE de Arduino/PlatformIO. |
| **`OTA_BT`** | Activa el modo OTA a través de Bluetooth Low Energy. | El ESP32 publica el servicio GATT para recibir el *firmware* desde una aplicación móvil o *host*. |
| **`NORMAL`** | Desactiva ambos modos OTA y regresa a la ejecución del programa principal. | El dispositivo libera recursos de red y CPU. |

---

### IV.  Próximos Pasos (Transición a Librería)

* **Migración:** Mover la lógica de las funciones de `loop()` y `setup()` a la clase `MoliOTAClass`.
* **API:** Definir la interfaz de usuario final (`MoliOTA.begin()` y `MoliOTA.handle()`).
* **Documentación Final:** Preparar la documentación de la librería (`MoliOTA.h`) para la Fase 2 de masificación.


