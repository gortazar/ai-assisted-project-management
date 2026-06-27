# Definition of Done (DoD) - Estándar de Calidad Obligatorio

Este documento define los requisitos mínimos e indispensables que CUALQUIER agente de IA o desarrollador humano debe cumplir antes de considerar una tarea como "Terminada" (Done) y abrir un Pull Request (PR). El incumplimiento de cualquiera de estos puntos resultará en el rechazo automático del cambio.

---
## 1. Código de Negocio (Application Code)
*   **Análisis Estático:** El código debe pasar el Linter y el formateador oficial sin warnings.
*   **Arquitectura:** Los cambios deben respetar los patrones definidos en `.ai/architecture.md`. Está prohibido introducir dependencias circulares o saltarse las capas de abstracción.
*   **Dependencies:** Every new dependency must be adequately motivated, and must imply an addition of an ADR record to the architecture document.
*   **Métricas de Calidad de Código:**
    *   **Complejidad Ciclomática:** No se permiten funciones con una complejidad superior a **10**.
    *   **Duplicación de Código:** El código duplicado introducido debe ser del **0%** (tolerancia cero a copiar y pegar bloques idénticos).
*   **Gestión de la Deuda Técnica (Regla de Oro):**
    *   Si una tarea incrementa la deuda técnica (p. ej., introduce un *code smell* necesario por limitaciones de una API externa), el agente **DEBE** cumplir una de estas tres condiciones:
        1. **Refactorización Acompañante:** Acompañar el cambio con un refactoring en el mismo PR que pague una cantidad equivalente o mayor de deuda técnica preexistente para mantener el balance en neutro o positivo.
        2. **Documentación Excepcional:** Justificar explícitamente en el código (`// DEBT: ...`) y en la descripción del PR el *porqué* es estrictamente necesario, junto con un plan para resolverlo.
        3. **Rechazo:** Si el incremento no está justificado ni compensado, el cambio será **rechazado automáticamente**.

## 2. Estrategia de Pruebas (Testing) - Criterios Estrictos
*   **Cobertura Mínima:** Cualquier línea de código nueva o modificada debe tener una cobertura de tests unitarios de al menos el **80%**.
*   **Pruebas de Integración:** Si la funcionalidad añade un nuevo endpoint, contrato, o flujo de datos con base de datos/APIs externas, se debe incluir al menos un test de integración.
*   **Regresión:** La suite completa de tests existentes debe ejecutarse y pasar con éxito ($100\%$ de éxito).
*   **Calidad del Test:** Está prohibido el uso de aserciones genéricas (ej. `expect(true).toBe(true)`). Cada test debe validar un comportamiento único y limpiar sus propios mocks/datos al finalizar.

## 3. Seguridad y Dependencias
*   **Vulnerabilidades:** No se permite la introducción de nuevas dependencias que contengan vulnerabilidades conocidas (CVEs) de nivel Medio, Alto o Crítico.
*   **Secretos:** Está estrictamente prohibido incluir credenciales, tokens, contraseñas o URLs de entornos productivos en el código. Usa variables de entorno.
*   **Análisis Estático de Seguridad (SAST):** El código debe pasar el escáner de seguridad del proyecto sin alertas activas.

## 4. Infraestructura como Código (IaC) y CI/CD
*   **Validación de Infraestructura:** Si la tarea requiere cambios en la infraestructura (Terraform, CloudFormation, etc.), el código de infraestructura debe ser validado con su respectivo linter (ej. `tflint`) y pasar los tests de arquitectura de la nube.
*   **Pipeline de CI:** El código propuesto debe ser capaz de compilar y pasar el pipeline de integración continua de manera local o en una rama efímera antes de solicitar la revisión de código.

## 5. Arquitectura y Documentación Viva
*   **Registros de Decisión Arquitectónica (ADR):** Cualquier cambio que modifique el diseño del software, la estructura de la base de datos, los contratos de las APIs principales o introduzca una nueva biblioteca clave **DEBE ir acompañado de un nuevo archivo ADR** en la carpeta `docs/architecture-decisions/`.
    *   El formato del ADR debe seguir el estándar MADS (Título, Contexto, Decisión, Consecuencias).
    *   El *Reviewer Agent* rechazará cualquier PR que altere la estructura del proyecto si no detecta un commit en la carpeta de ADRs.
*   **Documentación de Código:** Las funciones complejas o decisiones de diseño no evidentes deben estar documentadas en el propio código mediante comentarios estandarizados.
*   **Documentación del Proyecto:** Si el cambio modifica el comportamiento de un componente o API, se debe actualizar el archivo `.md` correspondiente en la carpeta `docs/`.
*   **Registro de Cambios (Changelog):** Añadir una breve descripción del cambio en el archivo de control o generar un mensaje de commit semántico (Semantic Commit) claro y descriptivo.

---

## Lista de Verificación para el Agente Revisor (Reviewer Agent)
Antes de aprobar un PR, verifica:
1. [ ] ¿El código hace EXACTAMENTE lo que pide el ticket?
2. [ ] ¿Se han añadido los tests unitarios/integración correspondientes?
3. [ ] ¿Se ha actualizado la documentación asociada?
4. [ ] ¿La suite de CI pasa sin errores?

Si falta algún check, deniega la aprobación y solicita las correcciones al agente responsable.
