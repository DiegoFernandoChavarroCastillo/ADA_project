# context
Estado global transversal: sesión de usuario (`AuthContext`) y notificaciones.
El consumo se hace mediante hooks (`useAuth`), nunca con `useContext` directo en componentes.
NO va aquí: estado propio de un módulo (va en su hook) ni consultas directas a Supabase.
Los providers usan `services` para acceder a datos.
