# Proyecto_PDI_FINAL
#Link de huggin face
https://huggingface.co/spaces/mariaposada2002/alarma_inteligente_pdi
# ⏰ Despertador Inteligente Anti-Snooze con Validación de Objetos por IA

Este repositorio contiene el código fuente, el pipeline de entrenamiento y los artefactos de producción para el **Despertador Inteligente Anti-Snooze**, un sistema interactivo de visión por computador diseñado para mitigar el hábito de postergar las alarmas matutinas. 

A diferencia de los despertadores convencionales, este sistema entra en un estado de alarma activa (`🔊 SONANDO`) y **únicamente se conmuta al estado `🔕 DESACTIVADA` cuando el usuario se levanta de la cama y enfoca un "Token Físico" específico ante la cámara** (por ejemplo, su taza de café matutina).

---

## 🚀 Características Principales

* **Detección Multiescala en Tiempo Real:** Impulsado por la arquitectura de vanguardia **YOLOv11m (Medium)** para un balance óptimo en la frontera de Pareto entre velocidad de inferencia (FPS) y precisión (mAP).
* **Compilación Gráfica con TorchScript:** Grafo binario optimizado con soporte dinámico de tensores (`dynamic=True`), reduciendo el consumo de memoria en producción e inmunizando el despliegue ante cámaras con distintas relaciones de aspecto.
* **Interfaz de Usuario Web:** Despliegue interactivo en **Hugging Face Spaces** utilizando **Gradio**, simulando un entorno de producción en la nube.
* **Resiliencia ante Pruebas de Estrés:** Diseñado para evitar falsos positivos mediante un pipeline robusto de preprocesamiento y funciones de pérdida avanzadas ($CIoU$, $DFL$ y $BCE$).

---

## 📊 Descripción de la Base de Datos

El modelo fue entrenado y calibrado utilizando un conjunto de datos estructurado de ambientes residenciales y cotidianos. Para maximizar la estabilidad del entrenamiento y evitar la confusión del modelo ante objetos pequeños o fragmentados, se aplicó una estrategia de **ingeniería de datos y consolidación taxonómica**.

### De 20 Categorías a 8 Macro-clases Domésticas
El dataset original contaba con 20 clases altamente específicas, lo que causaba un desbalance crítico. Estas fueron mapeadas y agrupadas semánticamente en **8 macro-clases de alta densidad**, las cuales operan como los "Retos Matutinos" del despertador:

| Macro-clase | Identificador del Token Físico | Rol Operativo en el Despertador |
| :--- | :--- | :--- |
| **`dishes_utensils`** | Tazas de café, platos, vasos, cubiertos. | **Reto Principal:** Obliga al usuario a levantarse e ir a la cocina a escanear su taza para apagar la alarma. |
| **`electronics`** | Laptops, pantallas, teclados, mouse. | **Reto de Estudio:** Valida que el usuario se encuentre en su escritorio de trabajo. |
| **`appliances`** | Microondas, refrigeradores, tostadoras. | **Reto de Desayuno:** Identifica la interacción con electrodomésticos de cocina. |
| **`security_access`** | Llaves, carteras, candados. | **Reto de Salida:** Valida la preparación previa antes de salir del hogar. |
| **`sharp_objects`** | Cuchillos, herramientas de corte. | Monitoreo de entorno y prevención de riesgos cotidianos. |
| **`containers`** | Botellas, cajas, termos de agua. | Soporte de contextualización espacial. |
| **`furniture`** | Sillas, mesas, escritorios. | Clasificación geométrica del entorno de fondo. |
| **`personal_items`**| Mochilas, ropa, calzado, libros. | Identificación de elementos de rutina matutina. |

### Robustez Matemático-Visual del Dataset
Las imágenes sufrieron un proceso de aumentación de datos para garantizar que el despertador funcione de manera confiable bajo cualquier condición matutina:
* **Variabilidad Lumínica:** Resiliencia ante habitaciones oscuras (madrugadas) o sobreexpuestas (luz solar directa).
* **Oclusiones Parciales:** Capacidad de detectar la taza de café incluso si la mano del usuario cubre una parte del objeto.
* **Perspectiva y Rotación:** Invarianza geométrica ante enfoques diagonales, cenitales o de cabeza desde la cámara del teléfono móvil.
