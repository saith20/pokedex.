# PokeDex – Creación de la cuenta en la nube (Azure for Students)

**Autor:** Saith Toscano Davila  
**Plataforma:** Microsoft Azure (Azure for Students) · Servicio: **Azure Static Web Apps (plan Free)**  
**Aplicación:** PokeDex (Angular 14), código fuente entregado en el caso de estudio.

## ¿Por qué Azure Static Web Apps?
- PokeDex es una aplicación Angular que, una vez compilada, es 100 % estática (HTML/CSS/JS).
- El plan Free no consume el crédito de la suscripción.
- Entrega HTTPS automático y permite definir encabezados de seguridad en `staticwebapp.config.json`.

## Paso a paso: crear la cuenta Azure for Students

### 1. Correo institucional
Tener a la mano el correo de la universidad.

### 2. Registro
1. Ir a <https://azure.microsoft.com/free/students>.
2. Clic en **Activate now / Comenzar**.
3. Iniciar sesión con una cuenta Microsoft (o crearla con el correo institucional).
4. Verificar la condición de estudiante con el código enviado al correo institucional.
5. Completar el perfil (nombre, país, teléfono) y aceptar los términos.



### 3. Ingreso al portal
1. Ir a <https://portal.azure.com>.
2. En **Suscripciones**, confirmar que aparece *Azure for Students*.



### 4. Grupo de recursos
1. Buscar **Resource groups** → **Create**.
2. Suscripción *Azure for Students*, nombre `rg-pokedex`, región más cercana disponible.
3. **Review + create → Create**.



### 5. Cuenta de GitHub
Crear una cuenta en <https://github.com/signup> (si no existe) para el repositorio `pokedex`.

El proceso de despliegue continúa en [Despliegue.md](./Despliegue.md).
