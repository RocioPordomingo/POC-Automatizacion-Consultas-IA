# Documentación de la POC: Automatización de Consultas con IA

Enlace para acceder al google sheet: [https://docs.google.com/spreadsheets/d/1WS5BJgToo8GdPLNdGseiexmN7a8pcOgkBZbTiRoIGII/edit?usp=sharing](https://docs.google.com/spreadsheets/d/1WS5BJgToo8GdPLNdGseiexmN7a8pcOgkBZbTiRoIGII/edit?usp=sharing)

## 1. Entendimiento del problema

### **Contexto Operativo**

La organización gestiona diariamente un volumen de entre **50 y 100 consultas internas**. Estas interacciones, canalizadas a través de medios informales como **WhatsApp y correo electrónico**, se centran en información operativa crítica y recurrente:

- **Estado de pedidos:** Seguimiento de órdenes y tiempos de entrega.
- **Disponibilidad de stock:** Consultas sobre existencias de productos por SKU.
- **Información de clientes:** Datos básicos para la gestión de envíos.

### **Diagnóstico de la Situación Actual**

Actualmente, el flujo de respuesta es **100% manual**. El equipo operativo debe interrumpir sus tareas principales para buscar datos en distintas fuentes y redactar cada respuesta desde cero. Este modelo presenta tres puntos críticos de dolor:

1. **Ineficiencia y Pérdida de Tiempo:** Se destina una carga horaria excesiva a **tareas repetitivas de bajo valor agregado**, lo que impide al equipo enfocarse en actividades estratégicas.
2. **Riesgo de Error Humano e Inconsistencia:** La dependencia de la manipulación manual de datos (operaciones de **copiar/pegar**) genera errores en la información comunicada, afectando la fiabilidad del servicio.
3. **Falta de Escalabilidad:** El proceso actual no es capaz de absorber un incremento en el volumen de demanda sin aumentar linealmente los recursos humanos, lo que genera cuellos de botella operativos.

### **Oportunidad de Mejora**

Se identifica la necesidad de implementar una **Prueba de Concepto (POC)**  que actúe como un asistente inteligente. El objetivo es transformar un proceso manual y reactivo en uno **asistido por IA**, que sea capaz de interpretar consultas en lenguaje natural, cruzar datos de inventario de forma relacional y automatizar la redacción de respuestas profesionales

En este contexto, se identifica una oportunidad clara de mejora mediante la incorporación de automatización asistida por inteligencia artificial.

### Objetivo de la POC

El propósito central de esta Prueba de Concepto es diseñar, desarrollar e implementar una solución funcional de rápida adopción que, mediante el uso de Inteligencia Artificial Generativa, permita:

- **Optimizar los tiempos de respuesta**: Reducir drásticamente la latencia entre la recepción de una consulta operativa y la generación de una respuesta precisa.
- **Automatizar la gestión de datos relacionales**: Eliminar la necesidad de que el operador cruce manualmente tablas de pedidos y stock para identificar factibilidad de entrega.
- **Mitigar el riesgo operativo**: Minimizar los errores de transcripción o inconsistencias de datos mediante la extracción directa de información desde fuentes estructuradas.
- **Habilitar acciones proactivas**: No solo informar, sino asistir en la ejecución de tareas administrativas, como la redacción de correos electrónicos ante incidencias de stock.

### **Supuestos de Trabajo**

Para garantizar la viabilidad del proyecto dentro de los plazos establecidos, se definen los siguientes supuestos estratégicos:

- **Disponibilidad de Datos Estructurados**: La información crítica (pedidos, stock e inventario) reside en hojas de cálculo de Google Sheets que actúan como la "única fuente de verdad" para el modelo de IA.
- **Accesibilidad vía API**: Se asume el uso de servicios de IA gratuitos (Google AI Studio) que permiten la integración directa con el ecosistema de productividad sin costos de infraestructura adicionales.
- **Estandarización de Consultas**: Si bien los usuarios utilizan lenguaje natural, las consultas operativas mantienen patrones comunes que permiten al modelo identificar rápidamente la intención (estado, stock o logística).
- **Arquitectura de Bajo Acoplamiento**: No se requiere la integración con sistemas corporativos complejos (ERP/CRM) en esta fase, priorizando una solución autónoma, escalable y de fácil mantenimiento.
- **Supervisión Humana (Human-in-the-loop)**: La solución está diseñada para asistir al humano. Las acciones de alto impacto, como el envío final de correos electrónicos, se mantienen en una instancia de "borrador" para validación previa.

### Análisis del Flujo Actual (AS-IS)

El proceso actual de atención a consultas internas se caracteriza por ser lineal y dependiente de la intervención manual en cada etapa:

1. **Recepción**: El usuario envía una consulta por WhatsApp o email.
2. **Lectura e Interpretación**: El equipo operativo procesa el mensaje y determina qué información se necesita.
3. **Búsqueda Multifuente**: Se accede manualmente a hojas de cálculo de pedidos y stock para cruzar datos.
4. **Redacción**: Se construye la respuesta mediante procesos de "copy/paste".
5. **Envío**: Se despacha la respuesta al usuario.

#### **Principales problemáticas identificadas**

- **Alto tiempo de resolución**: La búsqueda y el cruce manual de datos dilatan la respuesta.
- **Dependencia del factor humano**: El proceso se detiene si el personal operativo está saturado o ausente.
- **Tareas de escaso valor agregado**: El equipo dedica tiempo crítico a transcribir datos en lugar de gestionar excepciones.
- **Inconsistencia en la comunicación**: La falta de plantillas o automatización genera respuestas con formatos variables y posibles errores.

## 2- Diseñar una solución

Se propone la creación de un **Asistente Operativo Integrado** utilizando **Google Sheets e IA Generativa**. La solución consiste en una consola centralizada que, mediante Google Apps Script, conecta las tablas de inventario y pedidos con el modelo **Gemini 1.5 Flash**. Este sistema permite procesar consultas en lenguaje natural, realizar análisis relacionales automáticos entre tablas y generar borradores de respuesta listos para ser enviados desde Gmail, eliminando la carga de redacción manual

### **Criterio de Priorización**

Para maximizar el impacto de esta POC en el tiempo estipulado (1 jornada), se priorizaron las consultas que cumplen con:

- **Alta frecuencia**: Consultas que representan el mayor volumen diario.
- **Bajo nivel de complejidad**: Preguntas que no requieren juicio subjetivo, sino datos objetivos.
- **Disponibilidad de datos**: Información que ya reside en estructuras de filas y columnas (Google Sheets).

### **Alcance de la Automatización**

#### **Consultas Incluidas**

- **Estado y Trazabilidad de Pedidos**: Localización de órdenes y fechas estimadas de entrega.
- **Disponibilidad de Stock**: Verificación de existencias por SKU y detección de quiebres.
- **Asistencia en Comunicación**: Redacción automatizada de borradores para clientes ante incidencias de stock (valor agregado de la solución construida).

#### **Consultas Excluidas (Fuera de Alcance)**

- **Gestión de Reclamos**: Casos que requieren empatía o negociación directa con el cliente.
- **Decisiones Estratégicas**: Consultas que impliquen autorizar descuentos o cambios en la política comercial.
- **Datos No Estructurados**: Información que no se encuentre cargada en las pestañas DB_Pedidos o DB_Stock

#### Por qué se tomaron estas decisiones

- **Practicidad y ejecución**: Se priorizó una solución simple y funcional sobre una compleja sin terminar, siguiendo los criterios de evaluación de la POC.
- **Mitigación de riesgos**: Al excluir la toma de decisiones críticas y el envío automático, se garantiza la integridad de la relación con el cliente.
- **Escalabilidad**: El uso de una arquitectura basada en scripts y API permite que el sistema crezca en funcionalidades sin cambiar la estructura base.

## **3- Construcción de la POC Funcional**

### **A. Herramientas Seleccionadas**

Se optó por una arquitectura de **"Low-Code"** que permite una implementación ágil y sin costos de infraestructura:

- **Google Sheets**: Como interfaz de usuario y motor de base de datos.
- **Google Apps Script**: Como capa de integración para conectar la planilla con servicios externos.
- **Gemini 1.5 Flash (Google AI Studio)**: Como motor de inteligencia artificial para el procesamiento de lenguaje natural.
- **Gmail API**: Para la automatización de la comunicación saliente.

### **B. Arquitectura de Datos**

La solución se apoya en tres componentes principales dentro del libro de cálculo:

1. **DB_Pedidos**: Contiene el registro de órdenes, vinculadas a productos específicos mediante un identificador único (**SKU**).
2. **DB_Stock**: Inventario actualizado con niveles de existencia por SKU.
3. **Consola_IA**: Interfaz donde el usuario ingresa consultas y recibe el análisis procesado por el modelo.
4. Historial_IA: Interfaz para visualizar el historial de consultas realizadas.

## **4- Explicación del Resultado**

### **A. Cómo funciona la solución**

El sistema opera bajo un flujo lógico de **Extracción-Razonamiento-Acción**:

- **Extracción**: Al presionar "Procesar consulta", el script captura los datos de todas las tablas y los envía al modelo Gemini junto con la pregunta del usuario.
- **Razonamiento**: La IA actúa como un analista de datos, cruzando el SKU del pedido con el stock disponible para determinar la factibilidad de entrega.
- **Acción**: Si se detecta un faltante, el sistema genera automáticamente un borrador de correo electrónico en la bandeja de salida del usuario, redactado de forma profesional y con los datos específicos de la incidencia.
- **Auditoría**: Cada interacción se registra en una hoja de **Historial_IA**, asegurando la trazabilidad de las consultas y respuestas.

### **B. Decisiones Tomadas**

- **Modelo Flash**: Se eligió **Gemini 1.5 Flash** por su baja latencia y eficiencia en tareas de extracción de datos, ideal para una herramienta operativa de respuesta rápida.
- **Uso de SKUs**: Se normalizó la información mediante códigos de producto (SKU) para evitar errores comunes de interpretación por nombres de productos similares.
- **Instancia de Borrador**: Se decidió no realizar el envío automático del correo para permitir una validación final humana, mitigando riesgos de comunicación.

### **C. Limitaciones**

- **Capacidad de la API**: El tier gratuito de Google AI Studio posee límites de solicitudes por minuto que podrían afectar el escalado masivo.
- **Dependencia de Datos**: La calidad de la respuesta depende totalmente de que el equipo operativo mantenga actualizada la hoja de stock.

### **D. Roadmap: Qué haríamos en una Versión 2**

1. **Integración con WhatsApp**: Conectar el script con la API de WhatsApp Business para responder por el mismo canal donde se recibe la consulta.
2. **Actualización Automática**: Que el sistema descuente el stock automáticamente una vez que el pedido pasa a estado "Enviado".
3. **Dashboard de Métricas**: Incorporar una pestaña con gráficos que muestren el tiempo de respuesta ahorrado y los productos con mayor quiebre de stock.

## 5- Prueba del uso de la solución

### Prueba 1:

Para validar la efectividad de la POC, se realizó una prueba con una consulta operativa real que requiere el cruce de datos entre las tablas de pedidos y el inventario actual.

#### A. Escenario de Prueba (Input)

- **Consulta del usuario**: *“¿Qué pedidos tenemos pendientes de entrega?”*
- **Contexto**: El sistema debe identificar los pedidos en estados distintos a "Entregado" y verificar la disponibilidad de stock para cada uno.

#### B. Análisis y Respuesta del Asistente (Output)

La IA procesó la información de las bases de datos DB_Pedidos y DB_Stock, generando la siguiente respuesta detallada:

- **Pedido ID: ORD-1001**
    - **Cliente**: Juan Pérez
    - **Estado**: En preparación
    - **Análisis de Stock**: El sistema detectó que se solicitan 50 unidades de Resma A4 (SKU: PAP-001). Al verificar el stock (150 unidades disponibles), confirmó que **el stock es suficiente** para procesar la orden.
- **Pedido ID: ORD-1004**
    - **Cliente**: Ana Lopez
    - **Estado**: Pendiente de pago
    - **Análisis de Stock**: Se identificó una solicitud de 1 unidad de Silla Ergonómica (SKU: MUE-088). El sistema advirtió que **no cuenta con stock actualmente** (0 unidades disponibles), señalando la necesidad de una gestión de reabastecimiento.

Esta respuesta se puede visualizar también en el Historial_IA

#### C. Valor Agregado Identificado

En esta prueba, la solución demostró su capacidad para:

1. **Discriminar estados**: Filtró correctamente solo los pedidos que requieren atención inmediata.
2. **Cruzar datos relacionales**: Vinculó el SKU de la orden con las existencias físicas sin intervención manual.
3. **Generar alertas tempranas**: Detectó el quiebre de stock para el pedido de Ana Lopez antes de que se proceda al despacho, permitiendo una gestión proactiva

### Prueba 2:

En esta instancia, se testeó la capacidad del asistente para detectar anomalías operativas y disparar procesos de comunicación externa de forma autónoma.

#### **1. Escenario de Prueba (Input)**

- **Consulta del usuario**: *"Analizá si hay algún pedido con falta de stock. Si encontrás uno, redactá un mail para el cliente explicando la situación y empezá el mensaje con la palabra BORRADOR_GMAIL:"*
- **Objetivo**: Validar que la IA identifique el quiebre de stock y active el disparador (trigger) para crear un borrador en Gmail.

#### **2. Análisis Operativo y Respuesta (Output)**

El sistema realizó una auditoría en tiempo real de todos los pedidos pendientes contra el inventario disponible:

- **Identificación de Conflictos**: Detectó que el pedido **ORD-1004** de Ana Lopez presenta una falta de stock (Cantidad pedida: 1 vs. Stock disponible: 0).
- **Ejecución de Lógica**: Al detectar la palabra clave BORRADOR_GMAIL, el asistente no solo respondió en la celda de texto, sino que redactó el cuerpo de un correo profesional incluyendo el SKU (MUE-088) y el nombre del artículo.
- **Confirmación de Sistema**: El asistente devolvió el mensaje: *"✅ ¡Historial guardado y Borrador creado!"*, indicando que la acción se completó con éxito.

#### **3. Resultado en Gmail (Evidencia de Ejecución)**

Como resultado de la operación, se generó automáticamente un borrador en la cuenta de Gmail vinculada con:

- **Destinatario**: Cliente configurado.
- **Asunto**: Información sobre su pedido.
- **Cuerpo**: Un mensaje estructurado que explica la falta de stock temporal, solicita disculpas y establece el compromiso de contacto para una nueva fecha de entrega.

### **4. Valor Agregado Identificado**

- **Eliminación de la Redacción Manual**: El equipo operativo ahorra el tiempo de redactar correos desde cero ante incidencias recurrentes.
- **Trazabilidad Automática**: Cada vez que se genera este borrador, el evento queda registrado con fecha y hora en la hoja de Historial_IA, garantizando el control administrativo del proceso.
