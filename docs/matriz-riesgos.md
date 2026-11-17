# Matriz de riesgos de EventosU

Se evalúa el riesgo inicial de cada escenario descrito, antes de aplicar mitigaciones propuestas. La probabilidad es una estimación cualitativa basada en las condiciones del modelo de diseño.

| Nivel | Impacto si el escenario se materializa | Probabilidad dadas sus condiciones |
| --- | --- | --- |
| Bajo (1) | Efecto localizado, como una disputa por un escaneo individual. | Requiere privilegios elevados, coordinación precisa o condiciones difíciles de reunir. |
| Medio (2) | Compromete una cuenta, una entrada o una función de un evento. | Requiere un rol, un identificador adicional, una acción de la víctima o volumen considerable. |
| Alto (3) | Puede causar fraude repetible, divulgar listados de datos personales, comprometer privilegios de administración o interrumpir una operación esencial. | Puede intentarse repetidamente desde Internet o con una cuenta ordinaria, con pocos prerrequisitos adicionales. |

## Matriz 3 × 3

| Impacto / Probabilidad | Baja (1) | Media (2) | Alta (3) |
| --- | --- | --- | --- |
| Alto (3) | Medio · 3 | Alto · 6 | Crítico · 9 |
| Medio (2) | Bajo · 2 | Medio · 4 | Alto · 6 |
| Bajo (1) | Bajo · 1 | Bajo · 2 | Medio · 3 |

El puntaje es impacto × probabilidad. La categoría se toma de la celda de la matriz usada en clase.

## Evaluación de las 18 amenazas

| ID | Impacto | Probabilidad | Puntaje | Riesgo | Justificación del impacto | Justificación de la probabilidad |
| --- | --- | --- | ---: | --- | --- | --- |
| S01 | Medio | Alto | 6 | Alto | Compromete inscripciones y entradas de una cuenta. | El acceso público permite probar credenciales reutilizadas automáticamente. |
| S02 | Alto | Alto | 9 | Crítico | Permite emitir entradas impagas en múltiples eventos. | El webhook es accesible externamente y las solicitudes falsas son repetibles si falta autenticación del emisor. |
| S03 | Medio | Medio | 4 | Medio | Permite registrar admisiones con identidad ajena en los eventos asignados. | Requiere obtener credenciales de un operador con acceso. |
| T01 | Alto | Medio | 6 | Alto | Permite repetir fraude de pago y emitir entradas de mayor valor. | Exige una referencia auténtica y que la API omita verificar su correspondencia con la inscripción. |
| T02 | Medio | Bajo | 2 | Bajo | Admite a una persona extra y afecta el control de aforo del evento. | Requiere un QR válido, dos accesos y coincidencia dentro de la ventana de concurrencia. |
| T03 | Medio | Medio | 4 | Medio | Puede alterar la hoja de cálculo o desencadenar acciones en el equipo que la abre. | Exige que el asistente controle un campo exportado y que el organizador abra el CSV con un programa que evalúe la fórmula. |
| R01 | Medio | Medio | 4 | Medio | Dificulta atribuir la descarga de un listado con datos personales. | Requiere un organizador con acceso a una exportación y que la descarga no genere un registro individual. |
| R02 | Alto | Bajo | 3 | Medio | Dificulta investigar cambios de privilegios con alcance administrativo. | Requiere un administrador capaz de cambiar roles y registros que solo conserven el estado final. |
| R03 | Bajo | Medio | 2 | Bajo | Dificulta atribuir un escaneo individual durante una disputa en puerta. | Requiere acceso de operador y que solo se conserve el estado usado. |
| I01 | Alto | Medio | 6 | Alto | Expone datos personales de múltiples inscritos y puede repetirse entre eventos. | Requiere un rol de organizador y un identificador ajeno accesible. |
| I02 | Alto | Medio | 6 | Alto | Puede exponer listados completos a terceros fuera de la aplicación. | Exige acceso público efectivo en el bucket y que un tercero obtenga la clave o URL. |
| I03 | Medio | Medio | 4 | Medio | Expone una entrada y puede dejar a su titular sin acceso. | Requiere una cuenta y un identificador ajeno accesible. Para usar el QR, además debe llegar antes que el titular. |
| D01 | Alto | Medio | 6 | Alto | Puede paralizar el ingreso durante el periodo de mayor demanda. | Requiere una cuenta de puerta comprometida y capacidad de enviar solicitudes en volumen. |
| D02 | Medio | Medio | 4 | Medio | Degrada el servicio compartido mediante trabajos de exportación costosos. | Requiere un organizador con acceso a un evento y datos suficientes para solicitar exportaciones pesadas. |
| D03 | Alto | Medio | 6 | Alto | Puede retrasar confirmaciones y dejar a varios asistentes sin QR a tiempo. | El webhook es público, pero la degradación depende del volumen, del costo por solicitud y de la capacidad disponible. |
| E01 | Alto | Alto | 9 | Crítico | Permite conceder privilegios administrativos y afectar usuarios y eventos. | Una cuenta ordinaria puede invocar repetidamente la operación si la API omite autorizarla. |
| E02 | Alto | Alto | 9 | Crítico | Puede crear cuentas privilegiadas desde el registro público. | Cualquiera puede enviar el campo adicional si la API persiste el rol recibido. |
| E03 | Medio | Medio | 4 | Medio | Permite validar entradas fuera del ámbito asignado al operador. | Requiere una cuenta de puerta y una referencia de otro evento. |

## Cinco amenazas priorizadas para trazabilidad

1. **E01 y E02 (9 cada una)** permiten alcanzar privilegios administrativos desde funciones expuestas.
2. **S02 (9)** permite confirmar pagos inexistentes y emitir entradas.
3. **I01 e I02 (6 cada una)** se seleccionan entre los empates por el alcance potencial de la divulgación de listados completos. Cubren dos vías distintas de exposición, la API y el bucket. Si cambian el volumen de datos o los supuestos de exposición, se debe revisar este desempate.
