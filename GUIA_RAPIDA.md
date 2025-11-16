# Guía Rápida - Taller 4

## Inicio Rápido (5 minutos)

### 1. Activar entorno virtual
```powershell
.\wdm_new\Scripts\Activate.ps1
```

### 2. Instalar dependencias (si es primera vez)
```powershell
pip install -r requirements.txt
```

### 3. Abrir notebook
- Abrir `Taller_4.ipynb` en VS Code
- Seleccionar kernel `wdm_new`

### 4. Configurar área
En la celda de configuración (después de imports):
```python
AREA_TYPE = 'urbana'  # Opciones: 'urbana', 'semi_urbana', 'rural'
```

### 5. Ejecutar
- Ejecutar todas las celdas (Ctrl+Shift+P → "Run All")
- Esperar ~1-2 minutos
- Revisar resultados

---

## Parámetros Configurables

### Selección de Área
```python
AREA_TYPE = 'urbana'      # Área a analizar
```

| Área | Radio | Usuarios | Densidad |
|------|-------|----------|----------|
| `'urbana'` | 500m | ~100 | Alta |
| `'semi_urbana'` | 800m | ~60 | Media |
| `'rural'` | 1200m | ~30 | Baja |

### Parámetros Económicos
```python
COST_FIBER_PER_M = 1.0    # Costo por metro de fibra
COST_SPLITTER = 200.0      # Costo de instalación de splitter
SPLIT_RATIO = 32           # Usuarios máximos por splitter
```

### Power Budget (GPON clase B+)
```python
TX_POWER_DBM = 5.0         # Potencia transmitida (dBm)
RX_SENSITIVITY_DBM = -28.0 # Sensibilidad receptor (dBm)
SYSTEM_MARGIN_DB = 3.0     # Margen de sistema (dB)
# POWER_BUDGET_DB = 30.0   # Calculado automáticamente
```

### Proceso Iterativo
```python
MAX_ITERATIONS = 5         # Máximo de iteraciones
MIN_COVERAGE = 0.95        # Cobertura mínima requerida (95%)
```

---

## Resultados Esperados

### Archivos Generados
- `mapa_despliegue_pon.html` - Mapa interactivo
- `resultados_{area}.csv` - Resumen general
- `asignaciones_{area}.csv` - Detalles por usuario

### Métricas Clave

**Cobertura:**
- Urbana: 98-100%
- Semi-urbana: 95-100%
- Rural: 90-100%

**Splitters:**
- Urbana: 4-6 splitters
- Semi-urbana: 2-4 splitters
- Rural: 1-2 splitters

**Power Budget:**
- Pérdida promedio: 15-20 dB
- Margen promedio: 10-15 dB
- Cumplimiento: 100%

**Tiempo de Ejecución:**
- Urbana (100 usuarios): ~40-60s
- Semi-urbana (60 usuarios): ~25-35s
- Rural (30 usuarios): ~15-20s

---

## Interpretación de Visualizaciones

### Mapa Interactivo

**Splitters (marcadores rojos):**
- Click para ver utilización
- Tooltip muestra número de usuarios conectados

**Usuarios (círculos azules):**
- **Azul oscuro**: Margen >5 dB (excelente)
- **Azul**: Margen 2-5 dB (bueno)
- **Azul claro**: Margen 0-2 dB (ajustado)
- Click para ver:
  - Distancia Manhattan real
  - Pérdida óptica
  - Margen de Power Budget
  - Latencia de propagación

**Fibra (líneas verdes):**
- Rutas siguiendo calles reales
- Click para ver longitud del segmento

### Métricas en Terminal

```
VALIDACIÓN FINAL DEL DISEÑO
======================================================================
Total de usuarios asignados:           98/100
Cobertura:                             98.0%

POWER BUDGET (disponible: 30.0 dB):
  Pérdida mínima:                      12.34 dB
  Pérdida promedio:                    18.56 dB
  Pérdida máxima:                      24.78 dB
  Margen mínimo disponible:            5.22 dB
  Cumplimiento Power Budget:           SÍ

LATENCIA DE PROPAGACIÓN:
  Latencia mínima:                     2.45 µs
  Latencia promedio:                   12.34 µs
  Latencia máxima:                     32.10 µs

UTILIZACIÓN DE SPLITTERS:
  Splitters instalados:                4
  Utilización promedio:                87.5%
======================================================================
```

---

## Solución de Problemas

### Error: "No se encontraron rutas válidas"
**Causa:** Área muy pequeña o usuarios muy dispersos  
**Solución:** Aumentar radio del área o cambiar punto central

### Advertencia: "Usuario X no tiene splitters factibles"
**Causa:** Usuario muy alejado (Power Budget insuficiente)  
**Solución:** 
- Reducir `SPLIT_RATIO` (ej: 16 en lugar de 32)
- Aumentar `MAX_ITERATIONS`
- Considerar área más pequeña

### ILP toma mucho tiempo
**Causa:** Muchos usuarios o área muy grande  
**Solución:**
- Reducir `num_users` en configuración del área
- Aumentar límite de tiempo en `solve_ilp_with_power_budget(time_limit=600)`

### Numba: "First execution is slow"
**Causa:** JIT compilation en primera ejecución  
**Solución:** Es normal, ejecuciones posteriores son rápidas

---

## Comparación Rápida con Desafio3

| Característica | Desafio3 | Taller_4 |
|----------------|----------|----------|
| **Tiempo** | ~11 min | ~40s |
| **Cobertura** | 88-95% | 98-100% |
| **Splitters** | 8-12 | 4-6 |
| **Costo** | Mayor | Menor |
| **Escalabilidad** | <100 usuarios | <1000 usuarios |

---

## Próximos Pasos

### Experimentar con diferentes áreas:
1. Ejecutar con `AREA_TYPE = 'urbana'`
2. Ejecutar con `AREA_TYPE = 'semi_urbana'`
3. Ejecutar con `AREA_TYPE = 'rural'`
4. Comparar resultados en CSV

### Analizar sensibilidad:
- Cambiar `SPLIT_RATIO` (16, 32, 64)
- Variar `SYSTEM_MARGIN_DB` (2, 3, 5)
- Modificar costos económicos

### Personalizar área:
Editar diccionario `areas` para tu ciudad:
```python
areas = {
    'mi_area': {
        'point': (-33.XXXX, -71.XXXX),  # Coordenadas
        'radius': 600,  # metros
        'num_users': 80,
        'description': 'Mi área personalizada'
    }
}
```

---

## Contacto y Soporte

**Equipo:**
- Camila Herrera
- Gustavo Venegas
- Javier Cáceres

**Documentación:**
- `README.md` - Configuración general
- `CAMBIOS_TALLER4.md` - Detalles técnicos
- `COMPARACION_VERSIONES.md` - Análisis comparativo
