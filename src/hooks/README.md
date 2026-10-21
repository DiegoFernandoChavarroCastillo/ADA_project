# hooks
Capa de lógica: estado, loading/error, validaciones y orquestación de servicios.
Un hook por caso de uso, agrupado por dominio (`useRegisterMovement`, `useProducts`).
NO va aquí: JSX, SQL ni el cliente de Supabase. Los datos se piden siempre a `services`.
Devuelve a la UI datos ya listos: `{ data, isLoading, error, acciones }`.
