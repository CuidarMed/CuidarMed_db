# CuidarMed_DB

CuidarMed_DB es el contenedor de **bases de datos centralizado** para el ecosistema de microservicios de CuidarMed (`AuthMS`, `DirectoryMS`, `ClinicalMS`, `SchedulingMS`).  
Su objetivo es facilitar el despliegue y la inicialización de todas las bases de datos necesarias en un entorno de desarrollo o pruebas mediante Docker.

---

## 🗄 Contenido

- `Dockerfile`: Define la imagen de SQL Server 2022 y copia el script de inicialización.
- `docker-compose.yml`: Levanta el contenedor de SQL Server y lo conecta a la red `cuidarmed_new_net`.
- `init.sql`: Script de inicialización que crea las bases de datos iniciales.

---

## 📦 Bases de datos creadas por defecto

```sql
CREATE DATABASE AUTH;
CREATE DATABASE DIRECTORY;
GO
```
Estas bases de datos corresponden a los microservicios AuthMS y DirectoryMS.
Las demás bases (ClinicalDB, SchedulingDB) se pueden crear manualmente o mediante migraciones de los microservicios correspondientes.
---
## 🚀 Levantar la base de datos con Docker
1. Abrir VS Code
2. Clonar el repositorio
```bash
https://github.com/CuidarMed/CuidarMed_db.git
```
3. Abrir la terminal y crear la red de Docker:
```bash
docker network create --driver bridge --subnet 172.20.0.0/16 cuidarmed_new_net
```
4. Construir la imagen y levantar el contenedor:
```bash
docker-compose up --build
```
5. El contenedor expondrá SQL Server en el puerto 1433.
6.  Conectarse a la base de datos desde SQL Server Management Studio, Azure Data Studio o cualquier cliente SQL con:
- **Servidor:** localhost,1433
- **Usuario:** sa
- **Contraseña:** SecurePassword123!
---
## 🔧 Personalización
- Cambiar la contraseña de SA modificando la variable de entorno SA_PASSWORD en docker-compose.yml o Dockerfile.
- Agregar scripts adicionales de inicialización copiándolos al directorio docker-entrypoint-initdb.d/.
---
## 🌐 Red de Docker
El contenedor se conecta a la red externa **cuidarmed_new_net** para poder interactuar con los contenedores de los microservicios de CuidarMed (AuthMS, DirectoryMS, ClinicalMS, SchedulingMS).
---
## 📝 Notas
- Esta configuración está pensada para entorno de desarrollo o pruebas, no para producción.
- Para producción se recomienda un servidor SQL Server dedicado, con backups y seguridad configurada.
---
