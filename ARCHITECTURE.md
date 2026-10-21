# ARCHITECTURE.md — Especificación de Arquitectura de EcoTech Solutions

## 1. Contexto y problema
EcoTech Solutions desarrolla una aplicación de gestión de inventario sostenible (productos, categorías, movimientos de stock y métricas de sostenibilidad). El equipo es junior y tiende a mezclar lógica de negocio con componentes de UI, lo que genera errores recurrentes, código duplicado y cambios costosos.

## 2. Decisión arquitectónica
Adoptamos una arquitectura por capas (UI, lógica, servicios, tipos) sobre React 18 + Vite + TypeScript estricto, con Supabase como backend. Aplicamos explícitamente el principio de **Separación de Preocupaciones**: cada capa tiene una única razón para cambiar. Se eligió porque localiza los errores y permite que desarrolladores junior trabajen sin entender todo el sistema.

## 3. Estructura y responsabilidades de las capas
- `components/` y `pages/`: presentación e interacción; no conocen datos, así que la UI cambia sin riesgo para el negocio.
- `hooks/`: estado, validaciones y orquestación de casos de uso; concentran la lógica para que sea reutilizable y verificable.
- `services/`: único punto de acceso a datos; aíslan al resto del código de la base de datos.
- `types/`: contratos de dominio compartidos que reemplazan al `any`.
- `context/`: estado global transversal (sesión, notificaciones).
- `lib/supabase/`: cliente, variables de entorno y tipos generados.

## 4. Integración con Supabase (capa de servicios)
Toda la integración vive en `lib/supabase/` y solo `services/` la importa. Los servicios traducen filas de la base a tipos de dominio mediante mappers y normalizan los errores. Motivo: si el esquema o el proveedor cambian, se modifica un mapper en lugar de decenas de componentes. Las operaciones críticas, como registrar un movimiento de stock, usan una función RPC que actualiza movimiento y stock en una sola transacción, evitando inconsistencias. Las claves `service_role` nunca se exponen con prefijo `VITE_`.

## 5. Regla de dependencias
`components/pages → hooks → services → lib/supabase`

Las dependencias son unidireccionales y ningún componente importa el cliente de Supabase. Se eligió una dirección única porque impide ciclos y hace evidente, en la revisión de código, dónde se rompe una regla.

Ejemplo de flujo:

```text
MovementForm.onSubmit(data)
 → useRegisterMovement().register(data)    // valida y gestiona loading/error
 → movementService.registerMovement(data)  // llama a supabase.rpc(...) y mapea
 → Supabase (Postgres + RLS)
```

## 6. Cómo el plano evita la deuda técnica
Seguimos **Desarrollo Guiado por Especificaciones (SDD)**: cada módulo se describe en `docs/specs/` antes de programarse, con reglas, contratos y criterios de aceptación. Diseñar primero evita decisiones improvisadas, deja claro dónde va cada pieza y hace que los errores de arquitectura se detecten en la revisión, no en producción.

## 7. Escalabilidad
Cada módulo nuevo (por ejemplo, «proveedores») añade una carpeta por capa y un archivo de tipos, sin modificar lo existente. Esto reduce conflictos entre desarrolladores y mantiene constante el costo de cambio a medida que crece el producto.
