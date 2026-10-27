---
description: Sistema IoT de bajo costo para el monitoreo hídrico en Sabana Centro.
---

# Water Monitoring System (WMS)

## 💧 Water Monitoring System (WMS)

> **IoT Challenge 3 · Universidad de La Sabana · 2026-2**\
> **Sistema IoT de bajo costo para el monitoreo hídrico ante el fenómeno de El Niño**

{% hint style="info" %}
**Contexto operativo:** Ante el inicio anticipado del fenómeno de El Niño, la Sabana de Bogotá y Cundinamarca enfrentan riesgo de desabastecimiento hídrico. WMS usa una arquitectura distribuida para apoyar el monitoreo y la respuesta ante esta situación.
{% endhint %}

### Resumen del proyecto

El **Water Monitoring System (WMS)** es un prototipo IoT escalable. Monitorea en tiempo real la disponibilidad de agua en puntos críticos de suministro y almacenamiento de **Sabana Centro**.

Challenge #3 evoluciona los prototipos anteriores. Sustituye una solución local basada solo en ESP32 por una arquitectura híbrida. Esta incluye un Gateway Raspberry Pi, persistencia local, mensajería MQTT, visualización en la nube y análisis con inteligencia artificial.

### Equipo de desarrollo

| Integrante        | Rol principal                       | Aporte al proyecto                                                       |
| ----------------- | ----------------------------------- | ------------------------------------------------------------------------ |
| **Jasub Sastre**  | Arquitecto de software y requisitos | Planeación, arquitectura MQTT y seguimiento de requisitos de ingeniería. |
| **Thomas**        | Ingeniero de hardware y embebidos   | Integración de sensores, calibración ultrasónica y validación física.    |
| **Andrés Suárez** | Integrador de sistemas y logística  | Gestión de componentes, montaje y despliegue del Gateway Raspberry Pi.   |

El ensamblaje físico y las pruebas de campo fueron colaborativos. El equipo integró cada bloque de forma conjunta.

### Arquitectura del sistema

WMS organiza el monitoreo en dos capas conectadas mediante MQTT.

{% tabs %}
{% tab title="Dispositivo IoT · ESP32" %}
El ESP32 adquiere las variables físicas:

* **Nivel de agua:** HC-SR04.
* **Ambiente:** DHT22 y BMP180.
* **Radiación solar y luz:** BH1750.

El firmware usa una ISR para medir el eco ultrasónico. Un _superloop_ optimizado evita bloqueos del procesador.

El dispositivo también ofrece alertas locales. Incluye buzzer, LED RGB, LCD I²C de 16×2 y un tablero web WLAN. El tablero permite consultar el estado y desactivar alarmas físicas.
{% endtab %}

{% tab title="Gateway y nube" %}
La Raspberry Pi ejecuta el Broker Mosquitto. Recibe los mensajes MQTT publicados por el ESP32.

SQLite conserva los registros históricos antes de transferirlos. Ubidots recibe los datos mediante MQTT y permite consultar el tablero desde cualquier ubicación autorizada.

El módulo de inteligencia artificial se integra con APIs generativas, como Gemini o ChatGPT. Genera predicciones y recomendaciones a partir de anomalías detectadas.
{% endtab %}
{% endtabs %}

<details>

<summary>Flujo de datos</summary>

1. Los sensores capturan las variables del entorno y del suministro.
2. El ESP32 procesa las lecturas y publica los datos mediante MQTT.
3. La Raspberry Pi almacena los registros y los reenvía a Ubidots.
4. El módulo de IA analiza anomalías y apoya decisiones anticipadas.

</details>

{% hint style="warning" %}
Agrega aquí el diagrama de bloques de hardware y software cuando esté disponible. Ubícalo después de esta sección para complementar el flujo de datos.
{% endhint %}

### Componentes del repositorio

| Directorio            | Contenido                                                             |
| --------------------- | --------------------------------------------------------------------- |
| `/firmware/esp32/`    | Firmware del ESP32 con soporte MQTT y sensores.                       |
| `/gateway/raspberry/` | Configuración de Mosquitto, scripts SQLite y retransmisión a Ubidots. |
| `/ai_module/`         | Integración con modelos de IA para análisis predictivo.               |
| `/docs/`              | Documentación técnica sincronizada con este GitBook.                  |
