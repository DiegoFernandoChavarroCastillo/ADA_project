# lib/supabase
Única integración con Supabase: `client.ts` (singleton), `env.ts` (valida VITE_*) y `database.types.ts`.
Regenerar tipos con `npx supabase gen types typescript --project-id <id> > src/lib/supabase/database.types.ts`.
Solo `services/` importa de aquí. NO exponer el cliente a components, pages ni hooks.
NO va aquí: consultas de dominio ni lógica de negocio.
