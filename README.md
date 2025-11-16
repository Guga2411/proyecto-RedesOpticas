# Proyecto: Optimización de Despliegue PON

## Descripción
Este proyecto implementa un sistema de optimización para el despliegue de redes PON (Passive Optical Networks) utilizando:
- **Distancia Manhattan** para rutas realistas siguiendo la red vial
- **Clustering de usuarios** para posicionamiento óptimo de splitters
- **Power Budget** como restricción de calidad con validación iterativa
- **Programación Lineal Entera (ILP)** simplificada y eficiente
- **Optimización con Numba** para procesamiento de grandes datasets
- **Proceso iterativo** que garantiza cobertura completa de usuarios

**Integrantes:**
- Camila Herrera
- Gustavo Venegas
- Javier Cáceres

## Archivos del Proyecto

### Notebooks Principales:
- **`Desafio3.ipynb`**: Primera iteración funcional del proyecto
- **`Taller_4.ipynb`**: Segunda iteración mejorada (RECOMENDADO)

### Documentación:
- **`CAMBIOS_TALLER4.md`**: Detalle de modificaciones implementadas en Taller_4
- **`COMPARACION_VERSIONES.md`**: Análisis comparativo entre ambas versiones
- **`README.md`**: Este archivo (instrucciones de configuración y uso)

## Requisitos Previos
- Python 3.13.3 o superior
- Visual Studio Code con la extensión de Python
- Git (para clonar el repositorio)

## Configuración del Entorno

### Paso 1: Crear el Entorno Virtual

Abre una terminal en el directorio del proyecto y ejecuta:

```powershell
python -m venv wdm_new
```

Esto creará un entorno virtual llamado `wdm_new` en el directorio del proyecto.

### Paso 2: Activar el Entorno Virtual

#### En Windows (PowerShell):
```powershell
.\wdm_new\Scripts\Activate.ps1
```

#### En Windows (CMD):
```cmd
.\wdm_new\Scripts\activate.bat
```

#### En Linux/MacOS:
```bash
source wdm_new/bin/activate
```

**Nota:** Una vez activado, verás `(wdm_new)` al inicio de tu línea de comandos.

### Paso 3: Instalar las Dependencias

Con el entorno virtual activado, instala todas las dependencias necesarias ejecutando:

```powershell
pip install -r requirements.txt
```

Este comando instalará los siguientes paquetes:
- **osmnx** (2.0.6): Para obtener datos de OpenStreetMap
- **networkx** (3.5): Para análisis de grafos y rutas
- **geopandas** (1.1.1): Para manejo de datos geoespaciales
- **shapely** (2.1.2): Para operaciones geométricas
- **pulp** (3.3.0): Para resolver problemas de optimización lineal
- **matplotlib** (3.10.7): Para visualización estática
- **folium**: Para mapas interactivos
- **scikit-learn** (1.7.2): Para clustering (K-Means)
- **pandas** (2.3.3): Para manipulación de datos
- **numpy** (2.3.4): Para operaciones numéricas
- **numba** (0.60+): Para optimización con JIT compilation
- **jupyter** y **ipykernel**: Para ejecutar Jupyter Notebooks

### Paso 4: Seleccionar el Entorno en VS Code

#### Opción A: Desde el Notebook
1. Abre el archivo `Taller_4.ipynb` en VS Code
2. Haz clic en el selector de kernel en la esquina superior derecha (donde dice "Select Kernel")
3. Selecciona **"Python Environments..."**
4. Elige el entorno `wdm_new` de la lista (debería aparecer como "Python 3.13.3 ('wdm_new')")

#### Opción B: Usando la Paleta de Comandos
1. Presiona `Ctrl+Shift+P` (o `Cmd+Shift+P` en Mac)
2. Escribe y selecciona **"Python: Select Interpreter"**
3. Elige el intérprete que apunta a `.\wdm_new\Scripts\python.exe`

#### Opción C: Desde la Barra de Estado
1. Haz clic en la versión de Python mostrada en la barra de estado inferior
2. Selecciona el intérprete de `wdm_new`

## Uso del Proyecto

### Opción Recomendada: Taller_4.ipynb

1. Abre `Taller_4.ipynb` en VS Code
2. Verifica que el kernel seleccionado sea `wdm_new (Python 3.13.3)`
3. **Configurar área de estudio** en la celda de parámetros:
   ```python
   AREA_TYPE = 'urbana'  # Opciones: 'urbana', 'semi_urbana', 'rural'
   ```
4. Ejecuta todas las celdas en orden secuencial
5. El proceso iterativo se ejecutará automáticamente
6. Revisa resultados exportados en archivos CSV

### Secciones del Notebook (Taller_4):
1. Preparación (imports + Numba + parámetros)
2. Selección de áreas (urbana/semi-urbana/rural)
3. Generación de usuarios según densidad del área
4. **Posicionamiento de splitters por clustering**
5. Cálculo de rutas Manhattan (optimizado con Numba)
6. Validación de Power Budget
7. Preparación de datos para ILP
8. Formulación del ILP simplificado
9. **Bucle iterativo** (reposicionamiento automático)
10. Validación final (métricas completas)
11. Visualización (mapas estático e interactivo)
12. Conclusiones y análisis
13. Exportación de resultados

### Alternativa: Desafio3.ipynb (Primera Iteración)

Si deseas ver el enfoque original:

1. Abre `Desafio3.ipynb` en VS Code
2. Verifica el kernel `wdm_new`
3. Ejecuta las celdas en orden secuencial

**Nota:** Ver `COMPARACION_VERSIONES.md` para entender las diferencias entre ambas versiones.

## Estructura del Proyecto

```
proyecto-RedesOpticas/
│
├── Desafio3.ipynb               # Primera iteración (funcional)
├── Taller_4.ipynb               # Segunda iteración (RECOMENDADO)
├── CAMBIOS_TALLER4.md           # Documentación de cambios
├── COMPARACION_VERSIONES.md     # Análisis comparativo detallado
├── requirements.txt             # Dependencias del proyecto
├── README.md                    # Este archivo
├── mapa_despliegue_pon.html     # Mapa interactivo generado
├── resultados_*.csv             # Resultados exportados
├── asignaciones_*.csv           # Detalles de asignaciones
├── wdm_new/                     # Entorno virtual (no incluir en git)
└── cache/                       # Cache de datos de OSMnx
```

## Características Principales de Taller_4.ipynb

### 1. Áreas de Estudio Específicas
- **Urbana**: Centro denso, 500m radio, ~100 usuarios
- **Semi-Urbana**: Residencial, 800m radio, ~60 usuarios  
- **Rural**: Baja densidad, 1200m radio, ~30 usuarios

### 2. Posicionamiento Inteligente de Splitters
- Clustering K-Means de usuarios
- Splitters ubicados cerca de centroides
- Adaptación automática al número de usuarios

### 3. Optimización con Numba
- Funciones críticas con `@jit(nopython=True)`
- 10-100x más rápido que enfoque original
- Escalabilidad hasta 1000 usuarios

### 4. Proceso Iterativo
- Garantiza cobertura completa de usuarios
- Reposicionamiento automático de splitters
- Hasta 5 iteraciones (configurable)

### 5. ILP Simplificado
- Solo decide asignaciones (splitters pre-posicionados)
- Variables reducidas → Resolución 20-30x más rápida
- Power Budget como pre-validación

### 6. Validación Completa
- Power Budget por usuario
- Latencia de propagación
- Utilización de splitters
- Métricas de cobertura

### 7. Exportación de Resultados
- CSV con resumen general
- CSV con detalles por usuario
- Mapa interactivo HTML

## Parámetros Configurables

Puedes ajustar los siguientes parámetros en la sección 1:

**Modelo económico:**
```python
COST_FIBER_PER_M = 1.0      # Costo por metro de fibra
COST_SPLITTER = 200.0       # Costo por splitter
SPLIT_RATIO = 32            # Ratio de división (1:32)
```

**Power Budget (GPON):**
```python
TX_POWER_DBM = 5.0          # Potencia transmitida (dBm)
RX_SENSITIVITY_DBM = -28.0  # Sensibilidad receptor (dBm)
SYSTEM_MARGIN_DB = 3.0      # Margen de sistema (dB)
# Power Budget calculado: 30 dB
```

**Atenuación física:**
```python
FIBER_ATTEN_DB_PER_KM = 0.35
CONNECTOR_LOSS_DB = 0.5
RMAX = 20000.0              # Alcance máximo (metros)
```

En la sección 3.5 (poda):
```python
MAX_CANDIDATES = 500        # Máximo de nodos candidatos
K_NEAREST = 30             # Candidatos más cercanos por usuario
USE_CLUSTERING = False      # Activar clustering de usuarios
```

## Solución de Problemas

### Error: "No module named 'sklearn.utils'"
- Asegúrate de haber activado el entorno `wdm_new`
- Reinstala scikit-learn: `pip install --force-reinstall scikit-learn`

### Error: Kernel no disponible
- Verifica que el entorno esté activado
- Reinstala ipykernel: `pip install ipykernel`
- Registra el kernel: `python -m ipykernel install --user --name=wdm_new`

### Error al descargar datos de OSMnx
- Verifica tu conexión a internet
- OSMnx cachea datos en la carpeta `cache/`

### Tiempo de ejecución largo
- Reduce `MAX_CANDIDATES` y `K_NEAREST` en la celda 4
- Reduce `num_users_example` en la celda 2

## Notas Adicionales

- El primer run puede tardar varios minutos al descargar datos de OSMnx
- Los datos descargados se almacenan en cache para ejecuciones futuras
- El solver CBC se usa por defecto para resolver el ILP
- Se recomienda al menos 8GB de RAM para instancias grandes

## Referencias

- OSMnx: https://github.com/gboeing/osmnx
- PuLP: https://github.com/coin-or/pulp
- NetworkX: https://networkx.org/

## Licencia

Este proyecto es parte de un trabajo académico para el curso de TEL317 - Redes Ópticas WDM.
