Informe de Modelado de Procesos de Negocio (BPMN)
Proyecto: Gestión de Salida y Adecuación de Inmuebles (Check-out & Turnover)
Cliente: Empresa de Administración de Apartamentos (Tipo Airbnb)
Asignatura: Modelado de Procesos de Negocio
1. Descripción del Proceso Seleccionado
El proceso seleccionado para la digitalización es el "Check-out de Arrendatario y Adecuación de Inmueble". Este es un proceso crítico en la cadena de valor de la gestión de propiedades vacacionales, ya que conecta la finalización de un servicio con la preparación para el siguiente cliente, impactando directamente en la calidad percibida y la rentabilidad.
Alcance del proceso:
El flujo inicia cuando el arrendatario notifica su salida o se cumple la hora estipulada de check-out y finaliza cuando el apartamento está validado como "listo" para el siguiente huésped.
Actores involucrados:
 * Coordinador de Operaciones (Empresa): Responsable de la gestión del inventario y la validación final.
 * Arrendatario (Huésped): Actor externo que detona el inicio del proceso.
 * Proveedor de Limpieza y Lavandería (Externo/Tercerizado): Entidad encargada de la ejecución operativa de higiene y lencería.
2. Explicación del Modelo BPMN Propuesto
Para digitalizar este modelo, se ha estructurado un diagrama de colaboración que interactúa entre la empresa y el proveedor externo.
> 📝 Guía para dibujar tu diagrama (Instrucciones para Draw.io/Camunda):
>  * Pool 1 (Empresa de Administración):
>    * Evento de Inicio: "Hora de Check-out cumplida" o "Huésped entrega llaves".
>    * Tarea 1: "Realizar inspección de inventario" (Usuario).
>    * Compuerta (Gateway Exclusiva): ¿Hay daños o faltantes?
>      * Sí: Tarea "Registrar penalidad/cobro" -> Fin (o continuar).
>      * No: Continuar flujo.
>    * Tarea 2: "Solicitar servicio de adecuación" (Envío de mensaje).
>  * Pool 2 (Proveedor de Limpieza - Tercerizado):
>    * Evento de Mensaje: "Solicitud recibida".
>    * Subproceso (o Tarea): "Ejecutar limpieza y cambio de lencería" (Aquí se incluye el lavado).
>    * Evento de Fin: "Servicio completado".
>  * Retorno al Pool 1:
>    * Tarea 3: "Verificar calidad de limpieza".
>    * Compuerta: ¿Cumple estándares?
>      * No: Regresa a solicitar corrección.
>      * Sí: Tarea "Actualizar estado a Disponible".
>    * Evento de Fin: "Apartamento listo".
> 
Justificación del Flujo:
El modelo destaca la dependencia de un proveedor externo. Se han utilizado "Message Flows" (líneas punteadas) para conectar la solicitud de la empresa con la ejecución del proveedor, respetando la realidad de que la lavandería y limpieza son servicios tercerizados.
3. Análisis Comparativo: Caso Base vs. Caso Cliente
A continuación, se presentan las diferencias estructurales entre el caso académico (Clínica Salud Viva) y el caso real aplicado (Airbnb Management).
| Característica | Caso Base (Clínica Salud Viva) | Caso Cliente (Airbnb Management) |
|---|---|---|
| Naturaleza del Flujo | Informacional: El flujo es principalmente de datos (fechas, disponibilidad, confirmaciones). | Físico y Logístico: Implica el movimiento de personas al sitio, inspección tangible de objetos y transporte de lencería. |
| Interacción de Actores | Lineal y Automatizada: Paciente ↔ Sistema. La interacción humana es mínima en la etapa de agendamiento. | Colaborativa y Tercerizada: Requiere coordinación humana entre el gestor y un equipo externo de limpieza. |
| Manejo de Errores | Preventivo: El sistema no deja agendar si no hay hueco en la agenda. | Reactivo: Los problemas (daños, mala limpieza) se detectan durante el proceso y requieren bucles de reproceso. |
| Criticidad Temporal | Media/Alta: Depende de la disponibilidad. | Muy Alta (Ventana de Turnover): El tiempo entre la salida de un huésped (11 AM) y la entrada del siguiente (3 PM) es rígido. |
4. Investigación: Buenas Prácticas BPMN en la Industria
Para complementar el modelado, se han investigado estándares aplicables a la gestión de "Facility Management" y Hospitalidad bajo la norma BPMN 2.0.
A. Uso de Pools y Lanes para Tercerización
Según las buenas prácticas de la BPMN Method and Style (Silver, B.), cuando un proceso involucra una entidad externa sobre la cual no tenemos control total (como la empresa de lavandería), se debe modelar como una "Black Box" (Caja Negra) o un Pool separado.
 * Aplicación en el caso: Esto justifica por qué en nuestro modelo la empresa de limpieza está en un Pool distinto. Esto permite diferenciar claramente las responsabilidades contractuales.
B. Gestión de Excepciones (Exception Handling)
En la industria hotelera, el "Happy Path" (el flujo ideal donde todo está limpio y sin daños) ocurre solo en el 80% de los casos.
 * Aplicación: Es vital incluir compuertas (Gateways) para el manejo de "Inventario Dañado". En sistemas avanzados, esto detonaría un subproceso de "Mantenimiento" o "Reclamo de Fianza", alineándose con estándares de calidad ISO 9001 para servicios.
C. Digitalización y Automatización (BPM)
Empresas líderes en el sector (como Casai o Sonder) integran sus modelos BPMN con software PMS (Property Management Systems).
 * Tendencia: La tarea "Solicitar limpieza" en el diagrama no debería ser una llamada telefónica, sino una "Service Task" automatizada que envía una orden de trabajo a la app de las limpiadoras tan pronto el gestor marca el check-out en su tableta.
Recomendaciones Finales para tu Diagrama:
 * Objetos de Datos: Agrega un icono de "Documento" conectado a la tarea de inspección llamado "Checklist de Inventario".
 * Símbolos de Tarea: A la tarea "Solicitar servicio", ponle un sobrecito (Send Task) y a la del proveedor un sobrecito blanco (Receive Task) para mostrar que es una comunicación.
