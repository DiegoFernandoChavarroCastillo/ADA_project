# services
Único lugar donde se consulta o muta Supabase (Postgres, Auth, Storage).
Cada módulo expone funciones puras async + un mapper que convierte filas de BD en tipos de dominio.
Los errores se normalizan con `shared/service-error.ts`; nunca se devuelve una respuesta cruda de Supabase.
NO va aquí: React, hooks, estado ni JSX.
