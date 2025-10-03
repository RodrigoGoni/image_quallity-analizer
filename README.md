# Análisis de Calidad de Imagen y Detección de Enfoque

Este proyecto implementa algoritmos para la detección automática de puntos de máximo enfoque en videos, utilizando técnicas de análisis espectral similares a las empleadas en cámaras digitales modernas.

## Video de Entrada
- **Archivo**: `focus_video.mov`
- **Resolución**: 1920x1080
- **FPS**: 30.00
- **Duración**: ~4.53 segundos
- **Total de frames**: 136

## Algoritmos Implementados

### 1. Métrica del Dominio de Frecuencia (Paper Original)
Basado en *"Image Sharpness Measure for Blurred Images in Frequency Domain"*:
- Calcula la transformada de Fourier de la imagen
- Identifica pixeles con componentes de frecuencia superiores a un umbral
- **Fórmula**: FM = TH / (M × N), donde TH es el número de pixeles sobre el umbral

### 2. Algoritmos de Enfoque Avanzados
Implementación de algoritmos del paper *"Analysis of focus measure operators in shape from focus"*:

#### STA3 (Gray-level Variance)
- **Resistente al ruido gaussiano**
- Calcula la varianza de niveles de gris en ventanas locales
- **Fórmula**: STA3(i,j) = (1/N) × Σ(I(x,y) - μ)²

#### LAP2 (Laplaciano Modificado)
- **Mejor rendimiento en condiciones ideales**
- Sensible al ruido pero muy preciso
- **Fórmula**: LAP2(x,y) = Σ|I*LX| + |I*LY|

## Experimentos Realizados

### 1. Análisis de Frame Completo
- Detección del frame de máximo enfoque: **Frame 109**
- Métrica de calidad máxima: **2.10e-09**

### 2. Análisis por ROI (Región de Interés)
- **ROI 5%**: Máximo enfoque en frame 91 (calidad: 2.90e-05)
- **ROI 10%**: Máximo enfoque en frame 91 (calidad: 6.00e-06)

### 3. Matriz de Enfoque
Análisis con matrices equiespaciadas dentro del 60% central:
- **3×3**: Efectivo para análisis general
- **5×7** y **7×5**: Mayor granularidad en la detección

## Mejoras con Unsharp Masking

### Configuraciones Implementadas
- **Moderado**: σ=1.0, amount=1.5, kernel 5×5
- **Intenso**: σ=1.5, amount=2.0, kernel 7×7

### Resultados
El Unsharp Masking mejora significativamente la detección de enfoque, especialmente en frames con menor calidad inicial, expandiendo las zonas de enfoque detectables.

## Videos Generados

### Videos de Análisis Básico

#### 1. Transformada de Fourier
Visualización del espectro de frecuencias frame por frame:

[Ver video: fourier_transform_video.mp4](outputs/fourier_transform_video.mp4)

#### 2. Detección STA3 (Gray-level Variance)
Puntos de máximo enfoque usando algoritmo STA3 - Marcadores rojos indican zonas de máximo enfoque:

[Ver video: focus_points_video_STA3.mp4](outputs/focus_points_video_STA3.mp4)

#### 3. Detección LAP2 (Laplaciano Modificado)
Puntos de máximo enfoque usando algoritmo LAP2 - Mayor precisión en condiciones ideales:

[Ver video: focus_points_video_LAP2.mp4](outputs/focus_points_video_LAP2.mp4)

### Videos con Unsharp Masking

#### 4. STA3 + Unsharp Moderado
Algoritmo STA3 con realce moderado de imagen - Marcadores amarillos para distinguir del análisis básico:

[Ver video: focus_points_STA3_Unsharp_Moderate.mp4](outputs/focus_points_STA3_Unsharp_Moderate.mp4)

#### 5. LAP2 + Unsharp Intenso
Algoritmo LAP2 con realce intenso de imagen - Mejor detección en zonas de bajo contraste:

[Ver video: focus_points_LAP2_Unsharp_Intense.mp4](outputs/focus_points_LAP2_Unsharp_Intense.mp4)

## Requisitos

- OpenCV (cv2)
- NumPy
- Matplotlib
- tqdm
