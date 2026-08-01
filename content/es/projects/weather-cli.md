---
title: "Herramienta CLI del clima"
translationKey: "weather-cli"
date: 2024-02-20
draft: true
tags: ["python", "cli", "api"]
summary: "Una herramienta de línea de comandos para consultar pronósticos del clima con una interfaz limpia y colorida."
---

## Descripción general

Una herramienta de línea de comandos en Python que obtiene y muestra información meteorológica para cualquier ciudad. Mi primer proyecto trabajando con APIs externas y construyendo una aplicación CLI.

**GitHub:** [github.com/yourusername/weather-cli](https://github.com/yourusername/weather-cli)

## Motivación

Quería consultar el clima sin abrir un navegador o lidiar con sitios meteorológicos llenos de anuncios. Además, quería aprender a construir herramientas CLI y trabajar con APIs.

## Stack tecnológico

- **Lenguaje:** Python 3.10
- **API:** OpenWeatherMap API
- **Bibliotecas:**
  - `requests` para llamadas a la API
  - `click` para la interfaz CLI
  - `rich` para salida colorida en terminal
  - `python-dotenv` para variables de entorno

## Características

```bash
# Uso básico
$ weather Seattle
Seattle, US
Temperatura: 52°F (11°C)
Condiciones: Parcialmente nublado
Humedad: 65%
Viento: 8 mph NO

# Pronóstico de 5 días
$ weather Seattle --forecast

# Diferentes unidades
$ weather Tokyo --units metric
```

Características principales:
- Clima actual para cualquier ciudad
- Opción de pronóstico de 5 días
- Soporte para unidades métricas e imperiales
- Salida colorida y fácil de leer
- Manejo de errores para ciudades inválidas
- Caché para reducir llamadas a la API

## Detalles de implementación

### Integración con API
```python
def get_weather(city, api_key, units='imperial'):
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    params = {
        'q': city,
        'appid': api_key,
        'units': units
    }
    response = requests.get(base_url, params=params)
    response.raise_for_status()
    return response.json()
```

### Manejo de errores
Aprendí a manejar varios casos de error:
- Nombres de ciudades inválidos (errores 404)
- Problemas de conectividad de red
- Límites de uso de la API
- Claves de API faltantes

### Gestión de configuración
Usé variables de entorno para la clave de API:
```bash
# archivo .env
OPENWEATHER_API_KEY=tu_clave_api_aqui
```

Esto me enseñó sobre mantener secretos fuera del código y buenas prácticas de configuración.

## Lo que aprendí

### Trabajo con APIs
- Leer documentación de APIs
- Entender límites de uso y cuotas
- Manejar respuestas JSON
- Códigos de error y manejo de estados

### Diseño de CLI
- Análisis de argumentos con `click`
- Crear interfaces de comando intuitivas
- Proporcionar mensajes de error útiles
- Hacer la salida legible y atractiva

### Buenas prácticas en Python
- Entornos virtuales
- Requirements.txt para dependencias
- Estructura y organización de proyectos
- Escribir docstrings y comentarios

## Desafíos

**Desafío 1: Seguridad de la clave API**
Inicialmente codifiqué la clave API directamente en el código (¡mal!). Aprendí sobre variables de entorno y `.gitignore` para mantener los secretos seguros.

**Desafío 2: Límites de uso**
Alcancé los límites de uso de la API durante las pruebas. Implementé un caché simple para almacenar consultas recientes durante 10 minutos.

**Desafío 3: Colores en terminal**
Diferentes terminales muestran los colores de manera diferente. La biblioteca `rich` resolvió esto con soporte multiplataforma.

## Resultados

- ✅ Herramienta CLI completamente funcional
- 📦 Publicada en GitHub con documentación clara
- 🎨 Salida limpia y colorida fácil de leer
- 🔒 Manejo seguro de claves API
- 📚 Aprendí los fundamentos de integración con APIs

## Estadísticas de uso

Después de compartirla con compañeros:
- 12 estrellas en GitHub
- 3 personas usándola regularmente
- 2 solicitudes de funcionalidades (agregadas al plan)

## Mejoras futuras

- Agregar alertas y advertencias meteorológicas
- Soporte para múltiples ciudades a la vez
- Datos meteorológicos históricos
- Iconos del clima en arte ASCII
- Empaquetar para distribución en PyPI

## Conclusiones

Este proyecto demostró que incluso herramientas pequeñas pueden ser valiosas. La uso diariamente y es satisfactorio usar algo que construí. El proyecto me enseñó que:

1. **Empieza pequeño:** Un simple verificador del clima fue perfecto para aprender APIs
2. **La experiencia del usuario importa:** Incluso las herramientas CLI se benefician de un buen diseño
3. **La documentación es crucial:** Un README claro ayudó a otros a usar y contribuir
4. **Seguridad desde el día uno:** Aprender la gestión adecuada de secretos temprano es importante

¡El código completo y las instrucciones de instalación están disponibles en [GitHub](https://github.com/yourusername/weather-cli). Siéntete libre de probarlo o contribuir!
