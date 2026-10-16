# Libro de Faenas

Registro diario de las labores que hace el contratista con su gente en la viña.

- **Inicio del día:** se indica cuántas personas trajo el contratista. El máximo del día es personas × 8 horas.
- **Labores:** labor (del catálogo), cuartel, forma de pago (al día o al trato, con avance logrado), personas y horas.
- **Historial** por mes y **Resumen** por período, con descarga en CSV.

## Cómo está hecho

- Página estática (`index.html` + `config.js`), sin paso de build. Se publica en Vercel tal cual.
- Datos y usuarios en Supabase, proyecto `libro-faenas`:
  - `jornadas`: un registro por fecha, con la dotación y las labores del día (JSON).
  - `labores_catalogo`: catálogo de labores, copiado desde `vinedo-control`.
  - `miembros`: emails habilitados para usar la app. Solo quienes estén aquí pueden ver o editar datos (RLS).

## Agregar a una persona (por ejemplo, el contratista)

1. Supabase → proyecto **libro-faenas** → Authentication → Users → **Add user** → *Create new user*. Ingresa su email y una contraseña, y marca **Auto Confirm User**.
2. Supabase → SQL Editor:

   ```sql
   insert into public.miembros (email, nombre) values ('correo@ejemplo.cl', 'Nombre');
   ```

Para quitarle el acceso: `delete from public.miembros where email = 'correo@ejemplo.cl';`

Se recomienda desactivar el registro abierto de usuarios: Authentication → Sign In / Providers → desactivar **Allow new users to sign up**. Aunque alguien lograra crearse una cuenta, no vería datos si no está en `miembros`.

## Publicar en Vercel

1. En Vercel: **Add New → Project** → importa este repositorio de GitHub.
2. Framework Preset: **Other**. No hay comando de build ni carpeta de salida que configurar.
3. **Deploy**. Cada cambio que se suba a la rama `main` se publica solo.
