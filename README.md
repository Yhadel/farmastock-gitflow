# FarmaStock — CRUD de Medicamentos

Proyecto universitario para la práctica de **Scrum Poker** y **Sprint Planning**.
El incremento funcional final será un **CRUD de Gestión de Medicamentos** para FarmaStock.

> ✅ **Estado actual: CRUD completo.** Backend y frontend terminados y probados de punta a punta. Se puede **listar, crear, editar y eliminar** medicamentos desde la interfaz: la tabla se refresca sola tras cada cambio, y el borrado pide confirmación en un **modal propio** antes de ejecutarse (borrado lógico en la base de datos).

---

## 1. Descripción del proyecto

Aplicación web que permitirá **crear, listar, editar y eliminar** medicamentos de una farmacia (FarmaStock). Está dividida en dos partes:

- **Backend:** API REST con Node.js + Express, conectada a MySQL.
- **Frontend:** interfaz en React (Vite) que consume la API.

---

## 2. Tecnologías usadas

| Capa            | Tecnología            |
| --------------- | --------------------- |
| Frontend        | React.js + Vite       |
| Backend         | Node.js + Express.js  |
| Base de datos   | MySQL (`mysql2`)      |
| Control versión | Git + GitHub          |

---

## 3. Requisitos previos

Instala estas herramientas antes de empezar:

- **Node.js** (v18 o superior) → https://nodejs.org
- **MySQL** (v8 o superior) → https://dev.mysql.com/downloads/
- **Git** → https://git-scm.com

Verifica que estén instalados (PowerShell):

```powershell
node -v
npm -v
git --version
mysql --version
```

---

## 4. Cómo clonar el repositorio

```powershell
git clone https://github.com/DmeshellHeredia/farmastock-crud-medicamentos.git
cd farmastock-crud-medicamentos
```

---

## 5. Instalar dependencias del backend

```powershell
cd backend
npm install
```

> Esto instala Express, CORS, dotenv y mysql2. Volver a la raíz con `cd ..` cuando termines.

---

## 6. Instalar dependencias del frontend

```powershell
cd frontend
npm install
```

> Esto instala React y Vite. Volver a la raíz con `cd ..` cuando termines.

---

## 7. Crear la base de datos usando `schema.sql`

Desde la **raíz del proyecto** (PowerShell). Te pedirá la contraseña de MySQL:

```powershell
Get-Content backend/database/schema.sql | mysql -u root -p
```

Esto crea la base de datos `farmastock_db` y la tabla `medicamentos`.

> ⚠️ **PowerShell no soporta el operador `<`** (`mysql -u root -p < archivo.sql` da error).
> Usa `Get-Content archivo.sql | mysql ...` como arriba. Si prefieres CMD, sí funciona:
> `cmd /c "mysql -u root -p < backend/database/schema.sql"`

---

## 8. Insertar datos de prueba usando `seed.sql`

```powershell
Get-Content backend/database/seed.sql | mysql -u root -p
```

Esto agrega 3 medicamentos de ejemplo.

> 💡 Si prefieres una interfaz gráfica, puedes abrir y ejecutar ambos `.sql` desde **MySQL Workbench**.

---

## 9. Cómo configurar `.env`

El backend necesita un archivo `.env` con las credenciales de MySQL.

```powershell
cd backend
Copy-Item .env.example .env
```

Luego abre `backend/.env` y edita tus datos reales:

```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=tu_password
DB_NAME=farmastock_db
DB_PORT=3306
PORT=3001
```

*(Opcional)* El frontend también tiene un `.env.example` con `VITE_API_URL` si quieres cambiar la URL del backend.

---

## 10. Cómo correr el backend

```powershell
cd backend
npm run dev
```

- Servidor en: **http://localhost:3001**
- Prueba rápida: abre http://localhost:3001/ → debe responder un JSON.
- Endpoint del CRUD: http://localhost:3001/api/medicamentos

> `npm run dev` usa **nodemon** (reinicia solo al guardar). También existe `npm start`.

### Endpoints de la API (todos funcionando)

Base URL: `http://localhost:3001/api/medicamentos`

| Método | Ruta      | Descripción                        | Respuestas |
| ------ | --------- | ---------------------------------- | ---------- |
| GET    | `/`       | Listar medicamentos activos        | `200` |
| GET    | `/:id`    | Obtener uno por id                 | `200` · `404` |
| POST   | `/`       | Crear medicamento                  | `201` · `400` |
| PUT    | `/:id`    | Actualizar medicamento             | `200` · `400` · `404` |
| DELETE | `/:id`    | Eliminar (borrado lógico)          | `200` · `404` |

Campos del body para crear/actualizar: `nombre`, `categoria`, `precio`, `cantidad`, `fecha_vencimiento`, `proveedor`.

Ejemplo rápido (PowerShell):

```powershell
# Listar
curl http://localhost:3001/api/medicamentos

# Crear
curl -X POST http://localhost:3001/api/medicamentos `
  -H "Content-Type: application/json" `
  -d '{\"nombre\":\"Aspirina\",\"categoria\":\"Analgésico\",\"precio\":3.5,\"cantidad\":50,\"fecha_vencimiento\":\"2027-01-01\",\"proveedor\":\"Bayer\"}'
```

---

## 11. Cómo correr el frontend

En **otra terminal**:

```powershell
cd frontend
npm run dev
```

- App en: **http://localhost:5173**

---

## 12. Estado de cada parte

El CRUD está **completo**. Todo lo de abajo está implementado y probado.

### 🎨 Frontend — ✅ completo
> Listar, crear, editar y eliminar funcionan de punta a punta, probados en el navegador. La tabla se refresca sola tras cada cambio.
- [x] Listar medicamentos (`MedicamentoList.jsx`, conectado al backend) ✅
- [x] Formulario de registro con todos los campos (`MedicamentoForm.jsx`) ✅
- [x] Botón **Guardar** crea vía `createMedicamento` (`201`) ✅
- [x] Botón **Editar** funcional (carga el form, actualiza vía `updateMedicamento`) ✅
- [x] Mensajes de éxito / error + validación de campos ✅
- [x] La lista se actualiza automáticamente tras crear/editar/eliminar (estado compartido en `MedicamentoPage`) ✅
- [x] Botón **Eliminar** funcional: **modal de confirmación propio** + `deleteMedicamento` (borrado lógico) ✅

### ⚙️ Backend — ✅ completo
> El CRUD está implementado y probado de punta a punta (modelo + controlador + rutas). No queda nada pendiente por acá.

**Modelo (`medicamentoModel.js`):**
- [x] Listar (`findAll`) ✅
- [x] Obtener por id (`findById`) ✅
- [x] Crear (`create`) ✅
- [x] Actualizar (`update`) ✅
- [x] Eliminar / borrado lógico (`remove`) ✅

**Controlador (`medicamentoController.js`):**
- [x] Listar (`getMedicamentos` → `findAll`) ✅
- [x] Obtener por id (`getMedicamentoById` → `findById`) ✅
- [x] Crear (`createMedicamento` → `create`) ✅
- [x] Actualizar (`updateMedicamento` → `update`) ✅
- [x] Eliminar (`deleteMedicamento` → `remove`) ✅
- [x] Validaciones de campos obligatorios (400 si faltan) ✅
- [x] Códigos HTTP correctos (200 / 201 / 400 / 404 / 500) ✅

### 🗄️ Base de datos — ✅ listo
- [x] Tabla `medicamentos` creada (`schema.sql`) ✅
- [x] Conexión MySQL probada (`✅ Conexión a MySQL establecida` al arrancar) ✅
- [x] Datos de prueba insertados (`seed.sql`, 3 medicamentos) ✅

### 🧪 QA — ✅ backend y frontend probados
> Backend probado contra MySQL (5 endpoints, éxito y error) y frontend probado en el navegador (crear, editar con refresco automático, y el diálogo de confirmar eliminar).
- [x] Crear: `POST` → `201`/`400` (backend) + form en UI ✅
- [x] Listar: `GET` → `200` (backend) + tabla en UI ✅
- [x] Editar: `PUT` → `200`/`400`/`404` (backend) + edición en UI con refresco ✅
- [x] Eliminar: `DELETE` → `200`/`404` (backend) + modal de confirmación y borrado real en UI ✅
- [x] Validaciones (campos obligatorios, `precio: 0`) ✅

---

## 13. Flujo de trabajo en equipo con Git

> 🔒 **La rama `main` está protegida.** NO se puede hacer `git push` directo a `main`
> (GitHub lo rechaza). **Todo** cambio debe entrar por un **Pull Request** que debe ser
> **aprobado por el dueño del repositorio** (definido en `.github/CODEOWNERS`) antes de fusionarse.

Pasos que debe seguir cada integrante:

```powershell
# 1. Traer los últimos cambios de main antes de empezar
git checkout main
git pull origin main

# 2. Crear tu propia rama de trabajo
git checkout -b feature/nombre-de-tu-tarea

# 3. Trabajar, y ver el estado de tus cambios
git status

# 4. Agregar y confirmar tus cambios
git add .
git commit -m "feat: descripción de lo que hiciste"

# 5. Subir TU RAMA (nunca main) al repositorio
git push origin feature/nombre-de-tu-tarea
```

6. En GitHub, abre un **Pull Request** de tu rama hacia `main`.
7. Espera a que el **dueño del repo revise y apruebe** el PR.
8. Una vez aprobado, se fusiona a `main`. ✅

> ⚠️ Antes de poder subir ramas, cada integrante debe estar agregado como
> **colaborador** del repo (Settings → Collaborators).

---

## 14. Checklist para verificar que la base quedó funcionando

- [ ] `npm install` termina sin errores en `backend/` y `frontend/`.
- [ ] La base de datos `farmastock_db` y la tabla `medicamentos` existen.
- [ ] `backend/.env` está configurado con las credenciales correctas.
- [ ] `npm run dev` en backend muestra: `🚀 Servidor backend corriendo...` y `✅ Conexión a MySQL establecida`.
- [ ] http://localhost:3001/ responde con un JSON.
- [ ] `npm run dev` en frontend abre http://localhost:5173 y muestra la página de FarmaStock.

---

## Estructura del proyecto

```
farmastock-crud-medicamentos/
├── .github/
│   └── CODEOWNERS                       # Define quién debe aprobar los Pull Requests
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── db.js                    # Conexión a MySQL
│   │   ├── controllers/
│   │   │   └── medicamentoController.js # Capa HTTP: recibe y responde (CRUD completo)
│   │   ├── models/
│   │   │   └── medicamentoModel.js      # Capa de datos: consultas SQL (CRUD completo)
│   │   ├── routes/
│   │   │   └── medicamentoRoutes.js     # Rutas /api/medicamentos
│   │   ├── app.js                       # Configuración Express
│   │   └── server.js                    # Arranque del servidor
│   ├── database/
│   │   ├── schema.sql                   # Crea BD y tabla
│   │   └── seed.sql                     # Datos de ejemplo
│   ├── .env.example
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── MedicamentoForm.jsx      # Formulario crear/editar (funcional)
│   │   │   └── MedicamentoList.jsx      # Tabla + eliminar con modal de confirmación (funcional)
│   │   ├── pages/
│   │   │   └── MedicamentoPage.jsx      # Página principal
│   │   ├── services/
│   │   │   └── medicamentoService.js    # Llamadas a la API
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
├── .gitignore
└── README.md
```
### Mejoras realizadas

- Se documentó el proceso de consulta de medicamentos.

### Actualización de edición de medicamentos

Se añadió documentación sobre el proceso de edición de medicamentos dentro del CRUD, indicando que el usuario puede modificar la información de un medicamento existente y guardar los cambios para mantener actualizado el inventario.

### Actualización de eliminación de medicamentos

Se agregó documentación sobre el proceso de eliminación de medicamentos. Antes de eliminar un registro, el sistema solicita una confirmación al usuario para evitar eliminaciones accidentales y mantener la integridad de la información.

### Actualización de validación de datos

Se agregó documentación sobre la validación de los datos ingresados al registrar o editar medicamentos. El sistema verifica que los campos obligatorios estén completos y que la información ingresada sea válida antes de guardar los cambios en la base de datos.