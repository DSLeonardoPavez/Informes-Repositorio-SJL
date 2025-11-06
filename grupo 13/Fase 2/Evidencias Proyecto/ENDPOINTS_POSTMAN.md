# 🚀 Endpoints NeoTotem API - Guía para Postman

## Base URL
```
http://localhost:8001
```

---

## 📋 Índice
1. [Búsqueda](#búsqueda)
2. [Recomendaciones](#recomendaciones)
3. [Detección CV/IA](#detección-cvia)
4. [Voz (ASR)](#voz-asr)
5. [Productos](#productos)
6. [Sesiones](#sesiones)
7. [Analytics](#analytics)
8. [Dashboard](#dashboard)
9. [Calificaciones](#calificaciones)
10. [Tracking](#tracking)

---

## 🔍 Búsqueda

### 1. Buscar Productos
**GET** `/busqueda/?q={query}&limit={limit}&session_id={session_id}`

**Ejemplo:**
```
GET http://localhost:8001/busqueda/?q=casio&limit=10
```

**Parámetros:**
- `q` (required): Término de búsqueda
- `limit` (optional, default: 10): Número de resultados
- `session_id` (optional): ID de sesión para tracking

**Respuesta:**
```json
{
  "query": "casio",
  "total_results": 3,
  "results": [
    {
      "id_variante": 123,
      "nombre": "Casio G-Shock",
      "marca": "Casio",
      "categoria": "Accesorios",
      "color": "Negro",
      "talla": "Único",
      "precio": 89990.0
    }
  ]
}
```

---

### 2. Autocompletar Búsqueda
**GET** `/busqueda/autocomplete?q={query}&limit={limit}`

**Ejemplo:**
```
GET http://localhost:8001/busqueda/autocomplete?q=adid&limit=5
```

---

### 3. Tendencias de Búsqueda
**GET** `/busqueda/trending?limit={limit}`

**Ejemplo:**
```
GET http://localhost:8001/busqueda/trending?limit=10
```

---

## 💡 Recomendaciones

### 4. Recomendaciones Inteligentes (⭐ MÁS IMPORTANTE)
**GET** `/recomendaciones/smart`

**Parámetros de Query:**
- `texto_voz` (optional): Texto transcrito de voz (ej: "casio", "ropa formal")
- `intencion_voz` (optional): Intención detectada (ej: "buscar")
- `edad` (optional): Rango de edad (ej: "18-25", "26-35")
- `color` (optional): Color detectado (ej: "negro", "azul")
- `estilo` (optional): Estilo detectado (ej: "formal", "deportivo", "casual")
- `prenda` (optional): Prenda detectada (ej: "camiseta", "chaqueta")
- `session_id` (optional): ID de sesión
- `limit` (optional, default: 8): Número de recomendaciones

**Ejemplo 1 - Por Voz:**
```
GET http://localhost:8001/recomendaciones/smart?texto_voz=casio&intencion_voz=buscar&limit=8
```

**Ejemplo 2 - Por Imagen:**
```
GET http://localhost:8001/recomendaciones/smart?edad=26-35&color=negro&estilo=formal&prenda=chaqueta&limit=8
```

**Ejemplo 3 - Combinado:**
```
GET http://localhost:8001/recomendaciones/smart?texto_voz=ropa%20formal&edad=26-35&color=negro&estilo=formal&limit=8
```

**Respuesta:**
```json
{
  "recommendations": [
    {
      "id_variante": 123,
      "nombre": "Casio G-Shock",
      "marca": "Casio",
      "categoria": "Accesorios",
      "color": "Negro",
      "precio": 89990.0,
      "image_url": "..."
    }
  ],
  "strategy": "voice+image_detection",
  "total_found": 8,
  "confidence": "high"
}
```

---

### 5. Recomendaciones por Marca
**GET** `/recomendaciones/marca/{marca}?limit={limit}`

**Ejemplo:**
```
GET http://localhost:8001/recomendaciones/marca/Adidas?limit=10
```

---

### 6. Recomendaciones por Categoría
**GET** `/recomendaciones/categoria/{categoria}?limit={limit}`

**Ejemplo:**
```
GET http://localhost:8001/recomendaciones/categoria/Zapatillas?limit=10
```

---

### 7. Recomendaciones Trending
**GET** `/recomendaciones/trending?dias={dias}&limit={limit}`

**Ejemplo:**
```
GET http://localhost:8001/recomendaciones/trending?dias=7&limit=10
```

---

## 🎯 Detección CV/IA

### 8. Detectar Ropa en Imagen (⭐ MÁS IMPORTANTE)
**POST** `/cv/detect-frame`

**Headers:**
```
Content-Type: multipart/form-data
```

**Body (form-data):**
- `file`: Archivo de imagen (jpg, png)

**Ejemplo en Postman:**
1. Método: POST
2. URL: `http://localhost:8001/cv/detect-frame`
3. Body → form-data
4. Key: `file` (tipo: File)
5. Value: Seleccionar imagen

**Respuesta:**
```json
{
  "clothing_item": "camiseta",
  "style": "casual",
  "primary_color": "negro",
  "secondary_color": "blanco",
  "head_accessory": null,
  "bag": null,
  "age_range": "26-35",
  "annotated_image": "base64_encoded_image"
}
```

---

### 9. Análisis Completo de Cliente
**POST** `/cv/analyze-customer-ai-real`

**Body (JSON):**
```json
{
  "image_data": "base64_encoded_image",
  "session_id": "session_123"
}
```

**Ejemplo:**
```
POST http://localhost:8001/cv/analyze-customer-ai-real
Content-Type: application/json

{
  "image_data": "iVBORw0KGgoAAAANSUhEUgAA...",
  "session_id": "session_123"
}
```

---

## 🎤 Voz (ASR)

### 10. Transcribir Voz
**POST** `/asr/transcribe`

**Body (form-data):**
- `audio`: Archivo de audio (wav, mp3, m4a)

**Ejemplo en Postman:**
1. Método: POST
2. URL: `http://localhost:8001/asr/transcribe`
3. Body → form-data
4. Key: `audio` (tipo: File)
5. Value: Seleccionar archivo de audio

**Respuesta:**
```json
{
  "text": "buscar relojes casio",
  "confidence": 0.95,
  "language": "es"
}
```

---

### 11. Análisis de Voz con NLU
**POST** `/asr/voice`

**Body (form-data):**
- `audio`: Archivo de audio

**Respuesta:**
```json
{
  "text": "buscar relojes casio",
  "intent": "buscar",
  "confidence": 0.85,
  "entities": {
    "marca": "Casio",
    "categoria": "Accesorios"
  }
}
```

---

## 📦 Productos

### 12. Listar Productos
**GET** `/productos/?limit={limit}&offset={offset}`

**Ejemplo:**
```
GET http://localhost:8001/productos/?limit=20&offset=0
```

---

### 13. Detalle de Producto
**GET** `/productos/{id_producto}`

**Ejemplo:**
```
GET http://localhost:8001/productos/30
```

---

### 14. Detalle de Variante
**GET** `/product-detail/{variant_id}/detail`

**Ejemplo:**
```
GET http://localhost:8001/product-detail/123/detail
```

---

## 📊 Sesiones

### 15. Iniciar Sesión
**POST** `/sesiones/start`

**Body (JSON):**
```json
{
  "id_dispositivo": 1,
  "canal": "mixto"
}
```

**Ejemplo:**
```
POST http://localhost:8001/sesiones/start
Content-Type: application/json

{
  "id_dispositivo": 1,
  "canal": "mixto"
}
```

**Respuesta:**
```json
{
  "id_sesion": "session_abc123",
  "fecha_inicio": "2025-01-15T10:00:00",
  "estado": "activa"
}
```

---

### 16. Finalizar Sesión
**POST** `/sesiones/end/{id_sesion}`

**Ejemplo:**
```
POST http://localhost:8001/sesiones/end/session_abc123
```

---

## 📈 Analytics

### 17. Analytics de Sesión
**GET** `/analytics/sesion/{session_id}/metricas`

**Ejemplo:**
```
GET http://localhost:8001/analytics/sesion/session_abc123/metricas
```

---

### 18. Productos Top
**GET** `/analytics/productos-top?limit={limit}`

**Ejemplo:**
```
GET http://localhost:8001/analytics/productos-top?limit=10
```

---

### 19. Dashboard Analytics
**GET** `/analytics/dashboard`

**Ejemplo:**
```
GET http://localhost:8001/analytics/dashboard
```

---

## 🎛️ Dashboard

### 20. Dashboard en Tiempo Real
**GET** `/dashboard/real-time`

**Ejemplo:**
```
GET http://localhost:8001/dashboard/real-time
```

---

### 21. Analytics del Dashboard
**GET** `/dashboard/analytics`

**Ejemplo:**
```
GET http://localhost:8001/dashboard/analytics
```

---

### 22. Métricas en Vivo
**GET** `/dashboard/metrics/live`

**Ejemplo:**
```
GET http://localhost:8001/dashboard/metrics/live
```

---

## ⭐ Calificaciones

### 23. Calificar Recomendación
**POST** `/calificaciones/calificar`

**Body (JSON):**
```json
{
  "id_recomendacion": 123,
  "calificacion": 5,
  "comentario": "Excelente recomendación",
  "session_id": "session_abc123"
}
```

**Ejemplo:**
```
POST http://localhost:8001/calificaciones/calificar
Content-Type: application/json

{
  "id_recomendacion": 123,
  "calificacion": 5,
  "comentario": "Excelente recomendación",
  "session_id": "session_abc123"
}
```

---

### 24. Estadísticas de Calificaciones
**GET** `/calificaciones/estadisticas`

**Ejemplo:**
```
GET http://localhost:8001/calificaciones/estadisticas
```

---

## 📝 Tracking

### 25. Registrar Interacción
**POST** `/tracking/interaction`

**Body (JSON):**
```json
{
  "session_id": "session_abc123",
  "tipo_interaccion": "click_producto",
  "id_producto": 30,
  "metadata": {
    "pantalla": "recomendaciones",
    "timestamp": "2025-01-15T10:00:00"
  }
}
```

**Ejemplo:**
```
POST http://localhost:8001/tracking/interaction
Content-Type: application/json

{
  "session_id": "session_abc123",
  "tipo_interaccion": "click_producto",
  "id_producto": 30
}
```

---

### 26. Registrar Click en Producto
**POST** `/tracking/product/click`

**Body (JSON):**
```json
{
  "session_id": "session_abc123",
  "id_variante": 123,
  "fuente": "recomendaciones"
}
```

---

## 🔄 Turnos (Shifts)

### 27. Turno Actual
**GET** `/shifts/current`

**Ejemplo:**
```
GET http://localhost:8001/shifts/current
```

---

### 28. Estadísticas del Turno
**GET** `/shifts/{id_turno}/stats`

**Ejemplo:**
```
GET http://localhost:8001/shifts/1/stats
```

---

### 29. Analytics del Día
**GET** `/shifts/analytics/today`

**Ejemplo:**
```
GET http://localhost:8001/shifts/analytics/today
```

---

## 🛒 Compras

### 30. Verificar Precio
**GET** `/compra/verificar-precio/{id_recomendacion}`

**Ejemplo:**
```
GET http://localhost:8001/compra/verificar-precio/123
```

---

### 31. Comprar Directo
**POST** `/compra/comprar-directo`

**Body (JSON):**
```json
{
  "id_recomendacion": 123,
  "session_id": "session_abc123",
  "metodo_pago": "efectivo"
}
```

---

## 🧪 Health Check

### 32. Health Check de Búsqueda
**GET** `/busqueda/health`

**Ejemplo:**
```
GET http://localhost:8001/busqueda/health
```

**Respuesta:**
```json
{
  "status": "healthy",
  "search_engine": "operational",
  "database_stats": {
    "total_productos": 80,
    "total_variantes": 250,
    "total_categorias": 8
  }
}
```

---

## 📋 Colección Postman Recomendada

### Endpoints Esenciales (Top 10)

1. **GET** `/busqueda/?q=casio` - Búsqueda básica
2. **GET** `/recomendaciones/smart?texto_voz=casio` - Recomendaciones por voz
3. **GET** `/recomendaciones/smart?edad=26-35&color=negro&estilo=formal` - Recomendaciones por imagen
4. **POST** `/cv/detect-frame` - Detección de ropa
5. **POST** `/asr/transcribe` - Transcripción de voz
6. **GET** `/recomendaciones/marca/Adidas` - Recomendaciones por marca
7. **GET** `/productos/` - Listar productos
8. **POST** `/sesiones/start` - Iniciar sesión
9. **GET** `/dashboard/real-time` - Dashboard en tiempo real
10. **GET** `/busqueda/health` - Health check

---

## 💡 Tips para Postman

### Variables de Entorno
Crea un entorno en Postman con:
- `base_url`: `http://localhost:8001`
- `session_id`: `session_test_123`

### Ejemplo de uso:
```
GET {{base_url}}/busqueda/?q=casio&session_id={{session_id}}
```

### Headers Comunes
```
Content-Type: application/json
Accept: application/json
```

### Para archivos (multipart/form-data):
- Postman detecta automáticamente cuando agregas un archivo
- No necesitas agregar el header `Content-Type` manualmente

---

## 🔗 WebSocket (No disponible en Postman)

Para WebSocket, usa herramientas como:
- **wscat**: `wscat -c ws://localhost:8001/ws`
- **Postman** (soporta WebSocket en versiones recientes)
- **Insomnia** o **Thunder Client**

**Endpoint WebSocket:**
```
ws://localhost:8001/ws
```

**Mensaje de ejemplo:**
```json
{
  "type": "image_stream",
  "image_data": "base64_encoded_image",
  "camera_active": true
}
```

---

## 📝 Notas

- Todos los endpoints soportan CORS
- Los parámetros opcionales pueden omitirse
- `session_id` es opcional pero recomendado para tracking
- Los límites por defecto suelen ser 10 resultados
- Los archivos de imagen/audio deben ser < 10MB

---

**Última actualización:** 2025-01-15

