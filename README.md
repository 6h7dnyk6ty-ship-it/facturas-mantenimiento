[README (1).md](https://github.com/user-attachments/files/34991248/README.1.md)
# 🧾 Facturas · Mantenimiento

Aplicación web para el celular (se abre en el navegador, no se instala). Sirve para registrar las facturas del departamento.
Se toma la foto, el OCR lee los datos y luego la app pide el **centro de costo** y el **método de pago**.
Los datos quedan en Google Sheets y las imágenes en Drive. Cada persona ingresa con su usuario.

## Archivos
| Archivo | Dónde va |
|---|---|
| `index.html` | Repositorio de GitHub (GitHub Pages) |
| `Code.gs` | Apps Script de una hoja de cálculo de Google |

## Roles
| Rol | Puede |
|---|---|
| **Administrador** | Todo: facturas, crear/editar/desactivar usuarios, editar centros de costo y métodos de pago, eliminar facturas |
| **Registrador** | Registrar facturas, editar las que él registró, ver todas, exportar |
| **Contabilidad** | Solo consultar y exportar |

## Instalación (una sola vez, ~10 min)

### 1. Backend en Google
1. Con la cuenta de Workspace, cree una hoja de cálculo: **Facturas Mantenimiento**.
2. **Extensiones → Apps Script**, borre lo que haya, pegue todo `Code.gs` y guarde.
3. **Implementar → Nueva implementación → ⚙ Aplicación web**
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier usuario** *(el acceso real lo controla el usuario y la contraseña de la app)*
4. Autorice los permisos y copie la URL que termina en `/exec`.

### 2. Página en GitHub
1. Abra `index.html` y pegue la URL en la línea:
   ```js
   const API_URL = 'https://script.google.com/macros/s/XXXX/exec';
   ```
   *(Si la deja vacía, la app pide la URL en cada celular la primera vez.)*
2. Cree un repositorio (ej. `facturas-mantenimiento`) y suba `index.html`.
3. **Settings → Pages → Branch: `main` / root → Save**.
4. En 1–2 minutos queda en `https://SU-USUARIO.github.io/facturas-mantenimiento/`.

### 3. Primer ingreso
1. Abra la página. Como no hay usuarios, le pedirá **crear el administrador** (usted).
2. Vaya a **Más → Usuarios → Crear usuario** y cree el usuario de cada persona del equipo y de contabilidad.
3. Entréguele a cada uno su usuario y su contraseña inicial. Cada quien la cambia en **Más → Cambiar mi contraseña**.
4. En **Más → Centros de costo y métodos de pago**, ponga los centros de costo reales.

> 💡 En el celular, desde el menú del navegador use "Agregar a pantalla de inicio" para tener un acceso directo. No es una instalación, solo un atajo.

## Contabilidad
- Puede entrar a la app con un usuario de rol **Contabilidad**, filtrar por mes y exportar a Excel.
- O puede consultar directamente la hoja de cálculo (compártala en solo lectura). La pestaña **Facturas** tiene la columna **Mes**.
- Las imágenes quedan en la carpeta de Drive *Facturas Mantenimiento – Imágenes*. Compártala para que se abra el enlace "Ver imagen en Drive".

## Notas
- **Si cambia `Code.gs`:** Implementar → Administrar implementaciones → ✏️ Editar → Versión: *Nueva versión*. Si no lo hace, los cambios no se aplican. La URL no cambia.
- **Si alguien olvida la contraseña:** el administrador edita el usuario y le asigna una nueva.
- **Sin conexión:** la factura queda guardada en el celular, marcada como "pendiente", y se sube sola cuando vuelve la señal.
- **Contraseñas:** se guardan cifradas (hash con sal) en la hoja *Usuarios*. Las columnas de cifrado están ocultas; no las edite.
- **Sesión:** dura 7 días. Desactivar un usuario le cierra el acceso de inmediato.
- **OCR:** la primera lectura tarda más porque descarga el modelo de español (~10 MB). Revise siempre los datos antes de guardar.
