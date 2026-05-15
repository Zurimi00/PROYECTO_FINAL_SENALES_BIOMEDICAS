# Proyecto Final Simplificado: Senales Biomedicas con Pacientes Simulados

**Modulo base:** `00_señales_biomedicas.ipynb`
**Perfil:** Estudiantes de Ingenieria Biomedica
**Herramientas obligatorias:** funciones en Python, `pandas`, `matplotlib`

## Objetivo general

Construir un cuaderno de analisis que cargue una base de datos de pacientes
simulados y, a partir de sus parametros clinicos, genere y analice senales
biomedicas sinteticas.

El proyecto debe usar las funciones vistas en `00_señales_biomedicas.ipynb`:

- `gaussiana()`
- `ecg_sintetico()`
- `eeg_sintetico()`
- `emg_sintetico()`
- `nivel_contraccion()`
- `presion_arterial()`
- `spo2_ppg()`

## Dependencias sugeridas

Antes de comenzar, aseguren que el entorno tenga instaladas estas bibliotecas:

```bash
pip install pandas matplotlib
```

## Contexto del proyecto

El departamento de ingenieria biomedica de un hospital quiere probar un
prototipo de monitor virtual. Para ello les entrega una base de datos de
pacientes simulados con distintos perfiles clinicos. Su trabajo consiste en:

1. Leer la base con `pandas`.
2. Seleccionar pacientes de interes.
3. Generar sus senales sinteticas con las funciones del notebook 00.
4. Analizar los datos con tablas y estadisticas simples.
5. Presentar graficas clinicamente interpretables con `matplotlib`.

## Archivo de datos

Usen la base:

`Modulo_19_Ingenieria_Biomedica/datos/proyecto_final_pacientes_biomedicos.csv`

## Diccionario de datos

| Columna | Significado |
|---|---|
| `id_paciente` | Identificador unico del paciente |
| `edad` | Edad en anios |
| `sexo` | Sexo reportado (`F` o `M`) |
| `grupo_clinico` | Perfil clinico simulado |
| `fc_lpm` | Frecuencia cardiaca en latidos por minuto |
| `ruido_ecg` | Nivel de ruido para `ecg_sintetico()` |
| `estado_eeg` | Estado para `eeg_sintetico()` |
| `ruido_eeg` | Nivel de ruido para `eeg_sintetico()` |
| `factor_contraccion` | Intensidad base para el EMG |
| `pa_sistolica` | Presion arterial sistolica en mmHg |
| `pa_diastolica` | Presion arterial diastolica en mmHg |
| `spo2_media` | Saturacion promedio de oxigeno esperada |
| `duracion_segundos` | Duracion sugerida de la simulacion |
| `prioridad` | Nivel de prioridad clinica del caso |
| `nota_clinica` | Descripcion breve del caso |

## Instrucciones de desarrollo

Trabajen en un notebook llamado:

`entrega_final_equipo_apellidos.ipynb`

### Parte 1. Carga y exploracion de datos

1. Importen `pandas`, `matplotlib.pyplot`, `math` y `random`.
2. Carguen el CSV con `pd.read_csv()`.
3. Muestren:
   - las primeras 5 filas,
   - dimensiones de la tabla,
   - tipos de datos,
   - conteo de pacientes por `grupo_clinico`.
4. Verifiquen si hay valores faltantes.

### Parte 2. Preparacion de la base con pandas

Agreguen al menos estas columnas nuevas:

- `pulso_presion = pa_sistolica - pa_diastolica`
- `hipoxemia = spo2_media < 92`
- `taquicardia = fc_lpm > 100`
- `hipertension = (pa_sistolica >= 140) o (pa_diastolica >= 90)`

Despues respondan:

1. Cuantos pacientes tienen hipoxemia.
2. Cuantos pacientes tienen taquicardia.
3. Cual grupo presenta mayor presion sistolica promedio.

### Parte 3. Reutilizacion de funciones del notebook 00

Copien o adapten las funciones del notebook `00_señales_biomedicas.ipynb`.

Requisito minimo:

- `gaussiana()` debe seguir usandose dentro de `ecg_sintetico()` y
  `presion_arterial()`.
- `nivel_contraccion()` debe usarse para modular el EMG en el tiempo.

Sugerencia para el EMG:

```python
contraccion = min(1.0, fila["factor_contraccion"] * nivel_contraccion(t))
valor_emg = emg_sintetico(t, contraccion=contraccion)
```

### Parte 4. Generacion de senales por paciente

Construyan una funcion propia, por ejemplo:

```python
def generar_senales_paciente(fila):
    pass
```

La funcion debe devolver un `DataFrame` con columnas como:

- `tiempo_s`
- `ecg_mV`
- `eeg_uV`
- `emg_mV`
- `pa_mmHg`
- `spo2_pct`

Cada valor debe generarse usando los parametros de la fila del paciente.

### Parte 5. Pacientes a analizar

Seleccionen al menos:

- 1 paciente de `control_estable`
- 1 paciente de `taquicardia_estres`
- 1 paciente de `hipertension`
- 1 paciente de `hipoxemia_fatiga`

Para cada uno generen sus senales durante la duracion indicada en el CSV.

### Parte 6. Analisis obligatorio

Realicen como minimo estos analisis:

1. Tabla resumen por `grupo_clinico` con `groupby()`:
   - media de `fc_lpm`
   - media de `spo2_media`
   - media de `pa_sistolica`
   - media de `pa_diastolica`
2. Identificacion del paciente con:
   - mayor `fc_lpm`
   - menor `spo2_media`
   - mayor `pulso_presion`
3. Comparacion clinica breve entre los cuatro grupos.

### Parte 7. Graficas obligatorias

Incluyan al menos estas 5 figuras:

1. **Dashboard de un paciente** con 5 subplots:
   - ECG
   - EEG
   - EMG
   - Presion arterial
   - SpO2
2. **Comparacion de ECG** de tres pacientes:
   - normal
   - taquicardia
   - hipertension o hipoxemia
3. **Boxplot** de `spo2_media` por `grupo_clinico`.
4. **Grafica de barras** con la `pa_sistolica` promedio por grupo.
5. **Grafica de dispersion** de `fc_lpm` vs `spo2_media`, coloreada por grupo.

### Parte 8. Interpretacion clinica

Respondan en texto dentro del notebook:

1. Que grupo presenta mayor riesgo cardiovascular y por que.
2. Que diferencia observan entre un ECG normal y uno con taquicardia.
3. Como cambia el EEG entre `relajado`, `alerta` y `concentrado`.
4. Que relacion encuentran entre `fc_lpm` y `spo2_media`.
5. Que limitaciones tiene trabajar con datos simulados en lugar de datos reales.

## Requisitos minimos de entrega

La entrega debe contener:

1. `entrega_final_equipo_apellidos.ipynb`
2. Una exportacion en PDF o HTML del notebook
3. Todas las graficas visibles y tituladas
4. Comentarios breves explicando el flujo del codigo

## Estructura sugerida del notebook

1. Portada
2. Carga de bibliotecas
3. Lectura del CSV
4. Limpieza y exploracion con `pandas`
5. Definicion de funciones
6. Generacion de senales
7. Analisis estadistico basico
8. Graficas
9. Conclusiones clinicas

## Rubrica simplificada

| Criterio | Puntos |
|---|---:|
| Uso correcto de las funciones del notebook 00 | 25 |
| Manejo de `pandas` y analisis tabular | 20 |
| Calidad de las graficas en `matplotlib` | 20 |
| Interpretacion clinica de resultados | 20 |
| Orden, claridad y documentacion del notebook | 15 |

## Reto opcional

Si terminan temprano, agreguen una columna de clasificacion de riesgo:

- `bajo`
- `moderado`
- `alto`

Basen su criterio en una combinacion de:

- `fc_lpm`
- `spo2_media`
- `pa_sistolica`
- `pa_diastolica`

Luego grafiquen cuántos pacientes hay en cada categoria.
