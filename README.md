# 🧪 Automatización de Pruebas de API y Base de Datos: Creación de Usuarios

Este proyecto implementa una suite de pruebas automatizadas para el endpoint de creación de usuarios. Destaca por la integración de técnicas de **pruebas de caja gris (Grey-box testing)**, donde además de validar los códigos de respuesta del servidor (HTTP status codes), se realiza una verificación directa en la base de datos (tabla de usuarios) para garantizar la consistencia y persistencia real de la información.

---

## 🎯 Escenarios de Prueba Automatizados

El script `create_user_test.py` ejecuta **10 pruebas funcionales específicas** para validar las reglas de negocio del campo `firstName` (longitud permitida de 2 a 15 caracteres del alfabeto latino):

### ✅ Pruebas Positivas (Esperan HTTP 201, AuthToken y verificación en Base de Datos)
*   `test_create_user_2_letter_in_first_name_get_success_response`: Validación del límite inferior permitido (2 caracteres).
*   `test_create_user_15_letter_in_first_name_get_success_response`: Validación del límite superior permitido (15 caracteres).

### ❌ Pruebas Negativas (Esperan HTTP 400 Bad Request y mensajes de error específicos)
*   `test_create_user_1_letter_in_first_name_get_error_response`: Bloqueo por debajo del límite mínimo (1 carácter).
*   `test_create_user_16_letter_in_first_name_get_error_response`: Bloqueo por encima del límite máximo (16 caracteres).
*   `test_create_user_has_space_in_first_name_get_error_response`: Rechazo de espacios intermedios (`"A Aaa"`).
*   `test_create_user_has_special_symbol_in_first_name_get_error_response`: Control de caracteres especiales no permitidos (`"#AB&"`).
*   `test_create_user_has_number_in_first_name_get_error_response`: Bloqueo de números en el nombre de usuario (`"123"`).
*   `test_create_user_no_first_name_get_error_response`: Validación de payload cuando el parámetro `firstName` está ausente.
*   `test_create_user_empty_first_name_get_error_response`: Control cuando se envía el campo vacío (`""`).
*   `test_create_user_number_type_first_name_get_error_response`: Manejo de excepciones ante tipos de datos inválidos (un número entero `12`).

---

## 🛠️ Aspectos Técnicos Destacados (Estrategia QA)

*   **Validación Cruzada (API + DB):** La función `positive_assert` no solo valida la creación exitosa, sino que recupera activamente el estado de la base de datos utilizando `sender_stand_request.get_users_table()` para confirmar mediante conteo (`.count() == 1`) que el registro impactó correctamente en el almacenamiento del backend.
*   **Modularidad Avanzada:** Implementación de tres funciones de aserción especializadas (`positive_assert`, `negative_assert_symbol` y `negative_assert_no_firstname`) reduciendo la duplicación de código y aislando la lógica de los mensajes de error esperados.
*   **Gestión del Payload:** Uso de manipulación dinámica de diccionarios con `.pop("firstName")` para simular con precisión la ausencia de parámetros requeridos en la petición HTTP.

---

## 📁 Estructura del Proyecto

*   `data.py`: Contiene los payloads base y variables estáticas de prueba.
*   `sender_stand_request.py`: Centraliza las peticiones HTTP (`POST`, `GET`) para interactuar con la API y la base de datos.
*   `create_user_test.py`: Suite principal que aloja la lógica de pruebas y las aserciones con Pytest.

---

## 🚀 Cómo Ejecutar las Pruebas Localmente

### Prerrequisitos
*   Python 3.x instalado.
*   Librerías `pytest` y `requests` instaladas.

### Ejecución
Para correr las pruebas de este componente, abre tu terminal en la raíz del proyecto y ejecuta:

```bash
pytest create_user_test.py
```

---

## 📊 Impacto de Calidad (QA)

*   **Integridad de Datos:** Garantía de que los filtros de la interfaz se respetan estrictamente en la base de datos, evitando corrupción de registros.
*   **Aislamiento de Errores:** Cobertura del 100% de los mensajes de error definidos en la documentación técnica del backend, asegurando respuestas limpias hacia el cliente.
