# components
Capa de presentación: JSX, estilos y eventos de UI. Recibe datos y callbacks por props o desde hooks.
Organizada por dominio (`products/`, `movements/`…) más `ui/` (genéricos) y `layout/`.
NO va aquí: llamadas a Supabase o `services`, reglas de negocio ni `useEffect` para cargar datos.
Importa solo de `hooks`, `types`, `utils` y `constants`.
