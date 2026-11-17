# Respuesta y trazabilidad de EventosU

El inventario [amenazas.csv](amenazas.csv) documenta la decisión y el control propuesto para cada una de las 18 amenazas. Las pruebas siguientes son criterios de aceptación para una implementación futura en un ambiente de pruebas con datos ficticios.

## Cinco amenazas priorizadas

| ID · Riesgo | OWASP ASVS v5.0.0 | Tarea de desarrollo | Prueba de verificación y resultado esperado |
| --- | --- | --- | --- |
| E01 · Crítico | **8.2.1** · Restringir cada función a quienes tienen permiso explícito. | **SEG-01**. Proteger en la API todas las operaciones de asignación de roles con el permiso administrativo correspondiente. | **DAST autenticado.** Con una cuenta ordinaria, solicitar el cambio de rol y comprobar que la API responda 403 y que el rol permanezca igual. Repetir con un administrador autorizado y comprobar que el cambio se aplique. |
| E02 · Crítico | **15.3.3** · Restringir los campos de entrada para evitar la asignación masiva. | **SEG-02**. Definir un esquema estricto para registro público y asignar el rol inicial en el servidor. | **DAST.** Registrar un usuario incluyendo un campo de rol administrativo. Esperar un error de validación y que no se cree ninguna cuenta. Repetir con campos válidos y comprobar que el rol asignado sea el de asistente. |
| S02 · Crítico | **4.1.5** · Usar firma digital por mensaje en transacciones sensibles. | **SEG-03**. Adoptar una integración de pago que firme las notificaciones y verificar la firma y vigencia antes de acreditar la inscripción. La pasarela concreta queda por seleccionar. | **Prueba manual de API.** Enviar para una inscripción pendiente mensajes sin firma, con firma inválida y con cuerpo alterado. Comprobar que la inscripción siga impaga y que no se emita ningún QR. Una notificación auténtica y vigente de la pasarela de pruebas debe acreditar el pago y emitir la entrada. |
| I01 · Alto | **8.2.2** · Limitar el acceso a objetos concretos según los permisos. | **SEG-04**. Autorizar la generación y descarga de exportaciones por evento, a partir de la identidad del organizador y del evento asociado al objeto. | **DAST autenticado.** Crear eventos para dos organizadores. Desde la cuenta A, pedir la exportación y la descarga de B cambiando sus identificadores. Esperar 403 o 404 sin datos. Confirmar que A sí puede exportar y descargar el CSV de su evento. |
| I02 · Alto | **8.2.2** · Impedir el acceso al objeto sin permiso explícito, aquí en el almacenamiento. | **SEG-05**. Definir mediante IaC almacenamiento privado para los CSV y dar permiso de lectura a la identidad de la API. Mantener los afiches bajo una política separada. | **IaC y prueba manual.** Comprobar que la política bloquea acceso público y limita la lectura a la API. En un entorno de pruebas, consultar la URL directa de un CSV con acceso anónimo y con una identidad ajena. Ambas solicitudes deben fallar. Verificar que la API autorizada sí pueda leerlo. |

Referencia oficial: [OWASP Application Security Verification Standard 5.0.0](https://raw.githubusercontent.com/OWASP/ASVS/v5.0.0/5.0/OWASP_Application_Security_Verification_Standard_5.0.0_en.pdf).

## Riesgo aceptado para el ejercicio

**R03 · Escaneo sin atribución · Bajo.** Si una validación queda disputada, el estado de consumo permite controlar la reutilización del QR, aunque se pierde la atribución individual del escaneo. Se propone aceptar este riesgo limitado inicialmente y remitir las disputas al supervisor de puerta sin reactivar el código. El **responsable de Operaciones de EventosU** debe decidir su aceptación antes del piloto. Si se repiten las disputas o aumenta su impacto, debe reconsiderar la decisión y evaluar el registro de operador, evento y hora.
