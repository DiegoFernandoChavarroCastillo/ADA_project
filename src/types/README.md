# types
Contratos de dominio compartidos entre capas (Product, StockMovement, DTOs de entrada).
Un archivo `*.types.ts` por módulo, re-exportado desde `index.ts`.
Pueden derivarse de `lib/supabase/database.types.ts` con `import type`, sin acoplar la UI a la BD.
NO va aquí: lógica, valores en runtime ni `any`.
