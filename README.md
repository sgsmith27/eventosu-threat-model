# EventosU · Modelado de amenazas

**Autor:** Sergio Geovany García Smith  
**Modalidad:** individual  


EventosU es un sitio ficticio en fase de diseño. La universidad publica eventos y los asistentes pueden inscribirse, pagar y recibir una entrada QR. El modelo estudia cómo proteger estos flujos y los datos que manejan.

## Alcance

| Dentro del modelo | Fuera del modelo |
| --- | --- |
| Catálogo público, registro con correo, inicio de sesión local e institucional y restablecimiento de contraseña. | Infraestructura interna del proveedor de identidad y del servicio de correo. Sus intercambios con EventosU sí se modelan. |
| Creación de inscripciones, redirección a pago, recepción del webhook y actualización del estado del pago. | Procesamiento interno de tarjetas y conciliación bancaria. La redirección y el webhook de la pasarela sí se modelan. |
| Emisión, envío, descarga y validación del QR en la App de Puerta. | Seguridad física del recinto y operación de la red del lugar. Se modela el intercambio entre la App de Puerta y la API. |
| Creación de eventos, subida de afiches, consulta de inscritos, exportación y descarga de CSV, gestión de usuarios y roles, y consulta de reportes. | Sistemas universitarios ajenos al caso, excepto la interfaz de inicio de sesión institucional descrita en el enunciado. |

El modelo incluye React y su CDN, la ruta /admin, la API Node.js, PostgreSQL, el bucket de objetos, la App de Puerta y las integraciones con Microsoft Entra ID, la pasarela de pagos y el servicio de correo.

## Activos

| ID | Activo | Valor para EventosU |
| --- | --- | --- |
| A01 | Credenciales y tokens de sesión | Protegen el acceso a las cuentas y a sus operaciones. |
| A02 | Usuarios, roles y permisos | Determinan quién puede administrar usuarios, gestionar eventos o validar entradas. |
| A03 | Catálogo, eventos y afiches | Permiten publicar información fiable sobre los eventos. |
| A04 | Inscripciones | Vinculan a los asistentes con sus eventos y permiten gestionar los cupos y las entradas. |
| A05 | Referencias y estados de pago | Permiten asociar el cobro con la inscripción correspondiente y respaldar la emisión de la entrada. |
| A06 | Entradas QR y estado de uso | Permiten decidir quién puede ingresar y detectar intentos de reutilización. |
| A07 | Datos personales y CSV de inscritos | Permiten al organizador gestionar asistentes. Incluyen nombre, correo, carné y teléfono. |
| A08 | Disponibilidad de la validación en puerta | Permite mantener el ingreso al evento durante la llegada de los asistentes. |
| A09 | Registros de auditoría propuestos | Permitirían investigar pagos, accesos, cambios de rol y exportaciones. |

## Supuestos de trabajo

1. La API conserva en PostgreSQL la referencia que relaciona una inscripción con su pago y un registro de la entrada que indica si ya fue utilizada. El enunciado deja abierta la implementación interna del QR.
2. La App de Puerta necesita conectividad con la API para escanear y validar una entrada en el flujo normal. El escenario base se modela con validación en línea.
3. Se asume que cada organizador está autorizado a gestionar sus propios eventos y que el personal de puerta se asigna por evento. Estos permisos representan reglas de negocio para evaluar amenazas. Su implementación queda por diseñar.
4. Se asume que el organizador solicita y descarga los CSV a través de la API, que accede al bucket. El enunciado solo especifica que el bucket almacena los CSV exportados.
5. Para evaluar la trazabilidad se considera un almacén lógico de auditoría administrado por la aplicación. La persistencia y protección de estos registros son decisiones de diseño.
6. Para el DFD se representarán las interfaces web y móviles con HTTPS. La protección de las conexiones, la autenticidad del webhook, la autorización de las exportaciones y el manejo seguro del token se evaluarán como controles de diseño.

## Diagramas en Threat Dragon

Abre `model/eventosu.json` desde la opción de abrir un modelo local en OWASP Threat Dragon Desktop.  Desde el resumen de EventosU, abre cada diagrama para revisar sus componentes y flujos.

- **Nivel 0, contexto.** EventosU como un proceso, cinco roles de usuario y tres proveedores externos.
- **Nivel 1, componentes y flujos.** Cuatro límites de confianza, para el frontend/CDN, la API, los servicios de datos y el dispositivo de puerta. Los identificadores F01–F17 permiten vincular escenarios STRIDE a flujos concretos.
- **Flujos obligatorios.** F11 corresponde al webhook de pago, F10 a la validación del QR y F08/F15 a la solicitud, descarga y almacenamiento del CSV.

En el DFD, el sitio React representa también el cliente que se ejecuta en el navegador. La pasarela recibe los datos de pago directamente del asistente (F17). La integración con Entra ID (F12) resume el flujo OAuth que incluye redirecciones del navegador. Los transportes internos propuestos y la auditoría son supuestos de diseño.

## Informe y amenazas

- [Informe final](docs/informe.pdf)
- [Inventario con riesgo y respuesta](docs/amenazas.csv)

Las 18 amenazas están registradas en los flujos del DFD de nivel 1. En Threat Dragon figuran abiertas mientras las mitigaciones se mantengan como propuestas y la aceptación de R03 esté pendiente de aprobación.
