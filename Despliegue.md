# Despliegue de PokeDex (Angular) en Azure Static Web Apps

**Autor:** Saith Toscano Davila

## 1. Requisitos previos
- Cuenta Azure for Students y grupo `rg-pokedex` (ver [README.md](./README.md)).
- Cuenta de GitHub y Git.
- Node.js 18 LTS: `node -v`.
- VS Code.

## 2. Cambios realizados al código fuente original
| Archivo | Cambio | Motivo |
|---|---|---|
| `src/environments/environment.prod.ts` | `imagesPath` pasó de `/pokedex-angular/assets/images` a `/assets/images` | El original estaba pensado para GitHub Pages; en Azure la app vive en la raíz y las imágenes daban 404 |
| `src/index.html` | Se eliminó el script inline de "SPA para GitHub Pages" | Un script inline obliga a usar `unsafe-inline` en `script-src` y baja la seguridad |
| `src/404.html` | Eliminado | Solo servía para ese truco de GitHub Pages |
| `src/staticwebapp.config.json` | Nuevo | Encabezados de seguridad + redirección de rutas Angular a `index.html` |
| `angular.json` | Se agrega el config a `assets`; en producción `inlineCritical: false` | Angular inyecta un `onload` inline en los `<link>` de CSS que la CSP bloquearía y dejaría la página sin estilos |
| `package.json` | `prepare` pasó a `husky install \|\| true`; script `build:azure` | Evita fallos de husky en el servidor de CI |
| `.github/workflows/` | Se reemplazó el flujo de codecov por `azure-static-web-apps.yml` | Compila y publica en Azure |

## 3. Probar localmente
```bash
npm install
npm start
```
Abrir <http://localhost:4200>. Para probar la compilación de producción: `npm run build:azure` (queda en `dist/pokedex-angular`).

## 4. Subir a GitHub
1. Crear el repositorio **pokedex** en <https://github.com/new> (vacío).
2. En la carpeta del proyecto:
```bash
git init
git add .
git commit -m "PokeDex Angular para Azure Static Web Apps"
git branch -M main
git remote add origin https://github.com/<TU_USUARIO>/pokedex.git
git push -u origin main
```
> El primer push lanzará el workflow y fallará hasta crear el secreto del paso 6; es normal.

## 5. Crear la Static Web App en Azure
1. <https://portal.azure.com> → **Static Web Apps** → **Create**.
2. Pestaña **Basics**:
   - Suscripción: *Azure for Students*
   - Resource group: `rg-pokedex`
   - Name: `pokedex-saith` (único)
   - Plan type: **Free**
   - Region: la que ofrezca Azure
   - Deployment source: **Other**
3. **Review + create → Create**.
4. **Go to resource**: copiar la **URL** (`https://xxxx.azurestaticapps.net`).
5. En el recurso: **Overview → Manage deployment token** → copiar el token.



## 6. Conectar GitHub con Azure
1. En GitHub: repositorio `pokedex` → **Settings → Secrets and variables → Actions → New repository secret**.
2. Nombre: `AZURE_STATIC_WEB_APPS_API_TOKEN` · Valor: el token del paso anterior.
3. Pestaña **Actions** → seleccionar el flujo → **Run workflow** (o hacer un nuevo `git push`).
4. Esperar el ✔ verde. El flujo hace: `npm ci` → `ng build --configuration production` → publica `dist/pokedex-angular`.



Abrir la URL: debe cargar el listado de 151 Pokémon.

## 7. Encabezados de seguridad (`src/staticwebapp.config.json`)
| Encabezado | Función |
|---|---|
| Content-Security-Policy | Limita desde dónde se cargan scripts, estilos, imágenes y datos |
| Strict-Transport-Security | Fuerza HTTPS |
| X-Frame-Options | Evita clickjacking |
| X-Content-Type-Options | Evita MIME sniffing |
| Referrer-Policy | Limita la información de referencia |
| Permissions-Policy | Deshabilita APIs del navegador |
| Cross-Origin-Opener-Policy / Resource-Policy | Aíslan la página y sus recursos |

**Orígenes externos permitidos en la CSP** (los que realmente usa la app):
- `connect-src`: `beta.pokeapi.co` (GraphQL) y `pokeapi.co`.
- `img-src`: `raw.githubusercontent.com` (artwork oficial) y `assets.pokemon.com`.
- `style-src` / `font-src`: Google Fonts (`fonts.googleapis.com`, `fonts.gstatic.com`).
- `style-src 'unsafe-inline'`: Angular inserta estilos de componentes en línea. `script-src` se mantiene en `'self'` sin excepciones.

### Si algo no carga
1. F12 → **Console** → buscar `violates the following Content Security Policy directive`.
2. Agregar solo el origen bloqueado en `staticwebapp.config.json`, commit y push.

## 8. Verificación de seguridad
1. <https://securityheaders.com/> → pegar la URL → **Scan**.
2. Meta: **A+**.



### Auditorías adicionales (opcional, +5 puntos)
- **SSL Labs:** <https://www.ssllabs.com/ssltest/>
- **OWASP ZAP:** *Automated Scan* contra la URL pública y exportar el reporte.

## 9. Resultados
| Prueba | Resultado |
|---|---|
| URL pública | _pegar aquí_ |
| securityheaders.com | _A / A+_ |
| SSL Labs | _pegar nota_ |
