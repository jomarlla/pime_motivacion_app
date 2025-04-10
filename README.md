# Aplicación Web para la Observación Sistemática de la Motivación del Alumnado

Esta aplicación ha sido desarrollada en el marco de un Proyecto de Innovación y Mejora Educativa (PIME) de la Universitat Politècnica de València. Su objetivo es facilitar el registro sistemático de indicadores de motivación del alumnado durante sesiones prácticas, utilizando una herramienta ágil, no intrusiva y basada en evidencias.

## 🧾 Información del PIME

- **Título completo del PIME:** Mantener la motivación del estudiantado todo el curso, a través de la implementación de nueva metodología docente (aprendizaje basado en proyectos) y mejorando las estrategias de aprendizaje actuales y gestión del tiempo, para crear una infraestructura de datos espacial.
- **Duración:** Bienal (2024–2026)
- **Centro:** Escuela Técnica Superior de Ingeniería Geodésica, Cartográfica y Topográfica (ETSIGCT)
- **Universidad:** Universitat Politècnica de València (UPV)
- **Responsable del proyecto:** Dr. José Carlos Martínez Llario
- **Colaboradores:**
  - Dra. Eloína Coll Aliaga
  - Dr. Gaspar Mora Navarro
- **Asignaturas implicadas:** Infraestructuras de Datos Espaciales (IDE) y Sistemas de Información Geográfica Avanzado (SIGA)
- **Contacto:** jomarlla@upv.es | ecoll@upv.es | joamona@upv.es

### Objetivos del PIME

- Estudiar el efecto de la mejora de los materiales en la motivación, la gestión del tiempo y la comprensión del estudiantado.
- Evaluar el impacto de la implementación del aprendizaje basado en proyectos (ABP) en la satisfacción y motivación del alumnado.
- Desarrollar un sistema de observación sistemática y cuantitativa que permita una evaluación continua del estado motivacional.

## 🚀 Funcionalidades principales

- **Carga de estudiantes**: permite importar un fichero `.txt` con la lista de estudiantes agrupados por clase.
- **Selección de fecha, grupo y estudiante**: contextualiza cada observación y activa el checklist.
- **Checklist interactivo**: presenta 10 indicadores organizados en 5 dimensiones motivacionales:
  - Cognitiva
  - Volitiva
  - Afectiva
  - Social
  - Autonomía
- **Botones cíclicos** para registrar la observación de cada ítem: `Sí`, `No` o sin marcar.
- **Cálculo automático del porcentaje de motivación observada**, en función de los ítems registrados.
- **Almacenamiento local (localStorage)**: cada observación queda registrada en el navegador junto con metadatos (fecha, estudiante, grupo).
- **Exportación a CSV o SQL**: permite generar ficheros compatibles con análisis estadísticos o bases de datos.
- **Importación de registros previos**: facilita la continuidad en la recogida de datos a lo largo del curso.

## ⚠️ Consideraciones de uso

- La aplicación **almacena los datos localmente (localStorage)** del navegador. Esto significa que:
  - Los datos **no se comparten entre navegadores distintos** (por ejemplo, Chrome y Firefox).
  - Si se borra la caché del navegador, **los datos se perderán**.
  - Se recomienda **utilizar siempre el mismo navegador** y, a ser posible, la misma carpeta local si se trabaja desde un entorno web offline.

## 📄 Formato del fichero de estudiantes

- El fichero debe tener formato `.txt` delimitado por tabulaciones (`\t`).
- Cada línea debe incluir:

  ```
  NOMBREGRUPO<TAB>NOMBRE APELLIDOS
  ```
  Ejemplo:
  ```
  4GRPL1	María González López
  4GRPL2	Juan Torres Sánchez
  ```

- Es importante mantener este formato para el correcto funcionamiento del desplegable de grupos y estudiantes.
- **Nota:** Los nombres de los grupos deben contener los identificadores `PL1` o `PL2` para que la aplicación los detecte y clasifique correctamente.

## 🛠 Tecnologías utilizadas

- HTML5 + CSS3 (formato responsivo)
- JavaScript puro (sin frameworks externos)
- API de almacenamiento local (localStorage)

## 📤 Exportación e Importación

- **Exportar**:
  - A formato `.csv` para abrir en hojas de cálculo o SPSS.
  - A formato `.sql` para insertar en una base de datos PostgreSQL o similar.

- **Importar**:
  - Archivos `.json` generados por la misma aplicación en sesiones anteriores.

## 🧪 Ejemplos de consultas SQL útiles

Una vez exportados los datos a una base de datos, puedes utilizar las siguientes consultas para el análisis:

### Motivación media por grupo
```sql
SELECT grupo, AVG(motivacion_pct) AS motivacion_media
FROM regmot
GROUP BY grupo;
```

### Evolución de la motivación por fechas
```sql
SELECT fecha, AVG(motivacion_pct) AS motivacion_media
FROM regmot
GROUP BY fecha
ORDER BY fecha;
```

### Comparación entre versiones de una misma observación
```sql
SELECT estudiante, fecha, version, motivacion_pct
FROM regmot
ORDER BY estudiante, fecha, version;
```

### Media por dimensión (indicadores agrupados de dos en dos)
```sql
SELECT 
  AVG(CASE WHEN indicador_1_resp = '1' THEN 1 ELSE 0 END) AS cognitiva_1,
  AVG(CASE WHEN indicador_2_resp = '1' THEN 1 ELSE 0 END) AS cognitiva_2,
  AVG(CASE WHEN indicador_3_resp = '1' THEN 1 ELSE 0 END) AS volitiva_1,
  AVG(CASE WHEN indicador_4_resp = '1' THEN 1 ELSE 0 END) AS volitiva_2,
  AVG(CASE WHEN indicador_5_resp = '1' THEN 1 ELSE 0 END) AS social_1,
  AVG(CASE WHEN indicador_6_resp = '1' THEN 1 ELSE 0 END) AS social_2,
  AVG(CASE WHEN indicador_7_resp = '1' THEN 1 ELSE 0 END) AS autonomia_1,
  AVG(CASE WHEN indicador_8_resp = '1' THEN 1 ELSE 0 END) AS autonomia_2,
  AVG(CASE WHEN indicador_9_resp = '1' THEN 1 ELSE 0 END) AS afectiva_1,
  AVG(CASE WHEN indicador_10_resp = '1' THEN 1 ELSE 0 END) AS afectiva_2
FROM regmot;
```

## 📚 Licencia

Este software se distribuye bajo licencia [Creative Commons - Atribución 4.0 Internacional (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). Se permite su uso, modificación y redistribución con fines educativos y de investigación, siempre que se cite adecuadamente la autoría original.

## 📫 Contacto

Para sugerencias o mejoras, puedes escribir a través del repositorio o contactar con el equipo del proyecto PIME:
- José Carlos Martínez Llario: jomarlla@upv.es
- Eloína Coll Aliaga: ecoll@upv.es
- Gaspar Mora Navarro: joamona@upv.es
