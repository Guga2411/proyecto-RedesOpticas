# Desafío 3: Optimización de Despliegue PON

## Descripción
Este proyecto implementa un sistema de optimización para el despliegue de redes PON (Passive Optical Networks) utilizando:
- **Distancia Manhattan** para rutas realistas siguiendo la red vial
- **Power Budget** como restricción de calidad en el modelo ILP
- **Programación Lineal Entera (ILP)** con datos topológicos reales de OpenStreetMap

**Integrantes:**
- Camila Herrera
- Gustavo Venegas
- Javier Cáceres

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
- **jupyter** y **ipykernel**: Para ejecutar Jupyter Notebooks

### Paso 4: Seleccionar el Entorno en VS Code

#### Opción A: Desde el Notebook
1. Abre el archivo `Desafio3.ipynb` en VS Code
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

## Ejecutar el Notebook

Una vez configurado el entorno:

1. Abre `Desafio3.ipynb` en VS Code
2. Verifica que el kernel seleccionado sea `wdm_new (Python 3.13.3)`
3. Ejecuta las celdas en orden secuencial:
   - **Sección 1:** Importar librerías y parámetros (incluyendo Power Budget)
   - **Sección 2:** Extraer topología de OpenStreetMap
   - **Sección 3:** Calcular rutas con distancia Manhattan
   - **Sección 4:** Calcular Power Budget para validación
   - **Sección 5:** Formulación y resolución del ILP con restricciones de Power Budget
   - **Sección 6:** Validación final de presupuesto óptico y latencia
   - **Sección 7:** Visualización de resultados (estática e interactiva)
   - **Sección 8:** Conclusiones y análisis

## Estructura del Proyecto

```
proyecto-RedesOpticas/
│
├── Desafio3.ipynb          # Notebook principal con el análisis
├── requirements.txt         # Dependencias del proyecto
├── README.md               # Este archivo
├── wdm_new/                # Entorno virtual (no incluir en git)
└── cache/                  # Cache de datos de OSMnx
```

## Descripción de las Secciones del Notebook

### 1. Preparación: Instalación e Imports
Importa todas las librerías necesarias y configura parámetros globales:
- Costos de fibra y splitters
- **Power Budget GPON**: potencia TX, sensibilidad RX, margen de sistema
- Parámetros de atenuación física
- Ratio de división y alcance máximo

### 2. Extracción Topológica
- Descarga la red vial de Valparaíso, Chile usando OSMnx
- Genera usuarios sintéticos en nodos de la red
- Identifica candidatos para ubicación de splitters (intersecciones de alta conectividad)

### 3. Cálculo de Rutas Manhattan
- Calcula rutas óptimas usando **distancia Manhattan** (siguiendo calles reales)
- Almacena longitudes y aristas de cada ruta
- Proporciona distancias realistas para instalación de fibra

### 4. Cálculo de Power Budget
- Evalúa pérdidas ópticas para cada ruta: fibra, splitter, conectores, empalmes
- Valida factibilidad según presupuesto GPON (30 dB para clase B+)
- Identifica rutas que cumplen requisitos de transmisión

### 5. Formulación ILP con Restricciones de Power Budget
- Define variables de decisión (instalación de fibra, splitters, asignaciones)
- **Incorpora Power Budget como restricción del ILP** (no solo validación posterior)
- Implementa restricciones de capacidad, conectividad y alcance
- Resuelve el problema de optimización garantizando calidad óptica

### 6. Validación Física Final
- Verifica que todas las asignaciones cumplen Power Budget
- Calcula pérdidas ópticas y márgenes de sistema
- Valida latencia de propagación

### 7. Visualización
- Mapa estático (matplotlib): vista general del despliegue
- Mapa interactivo (folium): exploración detallada con métricas de Power Budget
- Usuarios coloreados según margen de Power Budget disponible
- Métricas finales: utilización, costos y cumplimiento de estándares

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
