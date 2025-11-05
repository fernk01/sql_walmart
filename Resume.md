# Crear Base de datos
Perfecto 🔥 muy buena idea — ese dataset de **ventas de Walmart** en Kaggle viene en formato **CSV**, y sí, puedes importarlo fácilmente a **MySQL** para trabajar con consultas SQL.

Te explico paso a paso cómo hacerlo 👇

---

## 🧩 1. Descargar el dataset de Kaggle

1. Entra al link:
   👉 [https://www.kaggle.com/datasets/mikhail1681/walmart-sales](https://www.kaggle.com/datasets/mikhail1681/walmart-sales)

2. Haz clic en **“Download”** (necesitas tener una cuenta Kaggle y estar logueado).

3. Se descargará un archivo ZIP, por ejemplo:

   ```
   walmart-sales.zip
   ```

   que dentro contiene algo como:

   ```
   walmart.csv
   ```

4. Extrae ese archivo CSV y colócalo en una carpeta fácil de ubicar, por ejemplo:

   ```
   ~/Escritorio/qsl/walmart.csv
   ```

---

## 🏗️ 2. Crear una base de datos en MySQL

Abre MySQL desde la terminal:

```bash
mysql -u root -p
```

(luego pon tu contraseña)

Y dentro de MySQL ejecuta:

```sql
CREATE DATABASE walmart_db;
USE walmart_db;
```

---

## 🧱 3. Crear una tabla compatible con el CSV

Abre el CSV con un editor de texto o Excel para ver los nombres de las columnas.
Por ejemplo, este dataset suele tener columnas parecidas a:

| Date | Store | Dept | Weekly_Sales | Holiday_Flag | Temperature | Fuel_Price | CPI | Unemployment |

Entonces podrías crear una tabla así:

```sql
CREATE TABLE sales (
  Date DATE,
  Store INT,
  Dept INT,
  Weekly_Sales DECIMAL(10,2),
  Holiday_Flag TINYINT,
  Temperature FLOAT,
  Fuel_Price FLOAT,
  CPI FLOAT,
  Unemployment FLOAT
);
```

---

## 📥 4. Importar el archivo CSV a MySQL

Asegúrate de que MySQL pueda leer el archivo.
Usa la ruta absoluta (por ejemplo `/home/lu/Escritorio/qsl/walmart.csv`).

Luego ejecuta en MySQL:

```sql
LOAD DATA INFILE '/home/lu/Escritorio/qsl/walmart.csv'
INTO TABLE sales
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

> ⚠️ Nota: si ves un error tipo *"The MySQL server is running with the --secure-file-priv option"*, te explico cómo solucionarlo más abajo.

---

## 🧪 5. Verifica que se importó correctamente

```sql
SELECT COUNT(*) FROM sales;
SELECT * FROM sales LIMIT 10;
```

---

## ⚠️ 6. Si obtienes error de “secure-file-priv”

Este error significa que MySQL solo permite leer CSV desde una carpeta específica.
Para saber cuál es, ejecuta:

```sql
SHOW VARIABLES LIKE 'secure_file_priv';
```

Esto te mostrará algo como:

```
/var/lib/mysql-files/
```

Entonces copia tu CSV a esa carpeta:

```bash
sudo cp ~/Escritorio/qsl/walmart.csv /var/lib/mysql-files/
```

Y luego ajusta la ruta en el comando:

```sql
LOAD DATA INFILE '/var/lib/mysql-files/walmart.csv'
INTO TABLE sales
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

---

## ✅ 7. Listo

Ya tendrás tus datos de Walmart dentro de MySQL, listos para hacer consultas, gráficos o análisis.
Por ejemplo:

```sql
SELECT Store, AVG(Weekly_Sales) AS avg_sales
FROM sales
GROUP BY Store
ORDER BY avg_sales DESC
LIMIT 5;
```


# Resumen
Excelente 🔥👏 — ¡bien hecho! Ya importaste correctamente un dataset real de Kaggle en MySQL, lo cual es un paso grande si estás aprendiendo bases de datos y análisis de datos.

Vamos con un **resumen claro** de lo que hiciste, los **conceptos que aprendiste** y las **funciones más importantes que deberías dominar** 👇

---

## 🧩 RESUMEN DE LO QUE HICISTE

1. **Descargaste un dataset CSV** (`Walmart_Sales.csv`) desde Kaggle.
   → Este archivo contenía datos tabulares con columnas como `Store`, `Date`, `Weekly_Sales`, etc.

2. **Creaste una base de datos y una tabla** en MySQL:

   ```sql
   CREATE DATABASE walmart_db;
   USE walmart_db;
   CREATE TABLE sales (...);
   ```

3. **Usaste `LOAD DATA INFILE`** para importar el CSV dentro de MySQL:

   * Moviste el archivo a la carpeta segura `/var/lib/mysql-files/` (por la opción `--secure-file-priv`).
   * Ajustaste el formato de fecha con `STR_TO_DATE()` para convertir `DD-MM-YYYY` → `YYYY-MM-DD`.

4. **Verificaste los datos cargados** con:

   ```sql
   SELECT COUNT(*) FROM sales;
   SELECT * FROM sales LIMIT 5;
   ```

✅ Resultado: ahora tenés una tabla de ventas completa en MySQL, lista para análisis con consultas SQL.

---

## 📘 CONCEPTOS CLAVE QUE APRENDISTE

| Concepto                  | Qué significa                                                                   |
| ------------------------- | ------------------------------------------------------------------------------- |
| **Base de datos**         | Contenedor donde guardás todas tus tablas (como `walmart_db`).                  |
| **Tabla**                 | Estructura donde se almacenan los datos en filas y columnas (`sales`).          |
| **Campo / Columna**       | Cada tipo de dato dentro de la tabla (ej: `Store`, `Date`, `Weekly_Sales`).     |
| **Registro / Fila**       | Una entrada de datos completa (una venta).                                      |
| **Tipos de datos**        | Cómo se guardan los valores: `INT`, `DATE`, `FLOAT`, `DECIMAL`, `VARCHAR`, etc. |
| **Primary Key**           | Identificador único para cada fila (si lo tuvieras).                            |
| **Consulta SQL (SELECT)** | Forma de leer y analizar datos con comandos.                                    |
| **Importación de datos**  | Proceso de traer datos externos (como CSV) a MySQL.                             |
| **STR_TO_DATE()**         | Función que convierte texto en formato de fecha válido.                         |

---

## 🧠 FUNCIONES Y COMANDOS MYSQL QUE DEBERÍAS MANEJAR

### 🔹 Estructura y administración

```sql
SHOW DATABASES;
CREATE DATABASE nombre;
USE nombre;
SHOW TABLES;
DESCRIBE tabla;
DROP TABLE tabla;
```

### 🔹 Creación y manipulación de tablas

```sql
CREATE TABLE ...
ALTER TABLE ...
INSERT INTO ...
UPDATE ... SET ...
DELETE FROM ...
```

### 🔹 Consultas básicas

```sql
SELECT * FROM tabla;
SELECT columna1, columna2 FROM tabla WHERE condición;
SELECT DISTINCT columna FROM tabla;
```

### 🔹 Filtrado y ordenamiento

```sql
WHERE, AND, OR, NOT
ORDER BY columna ASC/DESC;
LIMIT 10;
```

### 🔹 Funciones de agregación

```sql
COUNT(), SUM(), AVG(), MIN(), MAX()
GROUP BY columna
HAVING condición
```

Ejemplo:

```sql
SELECT Store, AVG(Weekly_Sales) AS promedio
FROM sales
GROUP BY Store
ORDER BY promedio DESC;
```

### 🔹 Fechas y conversiones

```sql
STR_TO_DATE(campo, formato)
DATE_FORMAT(campo, formato)
YEAR(), MONTH(), DAY()
```

### 🔹 Joins (cuando empieces con varias tablas)

```sql
INNER JOIN, LEFT JOIN, RIGHT JOIN
```

### 🔹 Importar / exportar datos

```sql
LOAD DATA INFILE ...
SELECT ... INTO OUTFILE ...
```

---

## 🚀 Qué podrías hacer ahora

1. **Analizar los datos**:

   * Ventas promedio por tienda, por mes, por feriado, etc.
   * Ejemplo:

     ```sql
     SELECT MONTH(Date) AS Mes, AVG(Weekly_Sales) AS Promedio
     FROM sales
     GROUP BY Mes
     ORDER BY Mes;
     ```

2. **Crear vistas o reportes:**

   ```sql
   CREATE VIEW ventas_por_tienda AS
   SELECT Store, SUM(Weekly_Sales) AS total_ventas
   FROM sales
   GROUP BY Store;
   ```

# ¿Por qué usar una base de datos si puedo analizar el CSV directamente con pandas?

Excelente reflexión.
Efectivamente, **sí podés hacer análisis estadístico solo con un CSV**,
pero **usar una base de datos tiene ventajas enormes** dependiendo del contexto 👇

| Situación                      | CSV + Pandas                                                                   | Base de datos (MySQL, PostgreSQL, etc.)                                    |
| ------------------------------ | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| **Tamaño de los datos**        | Ideal si el dataset es pequeño o mediano (menos de unos cientos de MB).        | Ideal si son millones de filas o varios GB. Maneja memoria eficientemente. |
| **Persistencia**               | El CSV es solo un archivo: cada vez que cerrás Python, los cambios se pierden. | Los datos quedan guardados y se pueden consultar en cualquier momento.     |
| **Consultas complejas**        | Pandas puede hacerlo, pero las operaciones pueden ser lentas.                  | SQL está optimizado para filtrar, agrupar y unir datos masivamente.        |
| **Integración multiusuario**   | Solo vos accedés al archivo.                                                   | Varias apps o usuarios pueden acceder al mismo dataset al mismo tiempo.    |
| **Seguridad y control**        | CSV = texto plano (sin contraseñas ni permisos).                               | MySQL maneja usuarios, permisos y acceso seguro.                           |
| **Escalabilidad / producción** | CSV no es práctico para sistemas grandes.                                      | Las bases de datos son el estándar para entornos empresariales.            |

💬 En resumen:

* Si estás **explorando un dataset** o haciendo análisis rápido → usa **pandas con CSV**.
* Si estás **trabajando con datos grandes, múltiples fuentes, o querés reutilizar consultas** → **usa una base de datos**.

---

## 🚀 Recomendación práctica

Una estrategia muy usada en ciencia de datos es **combinar ambos mundos**:

1. Usar **MySQL** para almacenar, limpiar y filtrar los datos con SQL.
2. Conectar **Python** → traer solo la parte que necesitás a un **DataFrame de pandas**.
3. Hacer el **análisis estadístico, visualización y modelado** desde Python.

Ejemplo:

```python
query = """
SELECT Store, 
       AVG(Weekly_Sales) AS avg_sales, 
       MONTH(Date) AS mes
FROM sales
GROUP BY Store, mes;
"""
df = pd.read_sql(query, con=engine)
```

Luego:

```python
import seaborn as sns
sns.lineplot(data=df, x='mes', y='avg_sales', hue='Store')
```

