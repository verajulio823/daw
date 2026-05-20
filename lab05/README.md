# Laboratorio 05 : Base de datos
| Autores | Rol | Porcentaje |
| :--- | :--- | :---: |
| Richart Escobedo | Elaboración del modelo lógico DER | 100% |
| Richart Escobedo | Implementación del modelo físico PostgreSQL | 100% |
| Richart Escobedo | Implementación en Supabase | 100% |
| Richart Escobedo | Elaboración del informe | 100% |
| | **Total** | **100%** |

| Entregables | URL |
| :--- | :--- |
| Repositorio | https://github.com/rescobedoq/enrollments.git |
| Informe | https://github.com/rescobedoq/enrollments/blob/main/informes/DAW_lab05_bd.pdf |

# Descripción de la práctica
- Elaboración del modelo lógico DER.
- Implementación del modelo físico PostgreSQL.
- Implementación en Supabase - Backend como servicio disponible en la nube.
- Elaborar README.md.
  
# Entregables
- Informe de práctica en PDF (enviar en la tarea de Classroom). [DAW_lab05_bd.pdf]
- Repositorio de GitHub que contenga los archivos, anexos e imágenes de su investigación.

# Recomendaciones
- Las tablas deben estar en nomenclatura en inglés, con el nombre de la tabla en plural y en camelcase, y todas deben tener los atributos id, status, created, modified, created_id y modified_id.
- Los atributos de relaciones deben tener el nombre de la tabla principal seguido del '_id'
- Las tablas producto de relaciones N:M deben estar ordenadas alfabéticamente. (Ejemplo: students_courses)

- **Ejemplo para la tabla llamada estudiantes:**
  
| **students** |
| :--- |
| **id** |
| names |
| fatherSurname |
| motherSurname |
| gender |
| address |
| phone |
| note |
| user_id |
| **status** |
| **created** |
| **modified** |
| **created_id** |
| **modified_id** |

## Rúbrica de calificación[^1]
| ítem | Descripción | Puntaje |
| :--- | :--- | :---: |
| **DER** | Elaboración del modelo lógico DER. | 4 |
| **Modelo físico** | Implementación del modelo físico PostgreSQL. | 8 |
| **Supabase** | Implementación en Supabase. | 5 |
| **Informe** | El laboratorio tiene un README.md que detalla toda la práctica. | 3 |
| **Prueba[^2]** | Se tomaron en cuenta todas las consideraciones y recomendaciones, lo que evidencia un trabajo en equipo. | -0 |
|  | **Total** | **20** |

Si el docente solicita un video, debe cargarse en Youtube o Drive y sólo debe entregarse la URL pública, sin que se solicite login alguno. Es recomendable incluir la URL tanto en el README.md como en el informe.

[^1]: La autocalificación es obligatoria.
[^2]: El docente debe comprobar el cumplimiento de todas las consideraciones y recomendaciones, evidenciando el trabajo en equipo con responsabilidad y la práctica de la ética profesional, a fin de no aplicar ninguna penalidad.

## Referencias
- https://supabase.com/
- https://www.postgresql.org/docs/current/index.html
- https://help-sageestimating.na.sage.com/en-us/23_1/Content/geninfo/developing_a_database_coding_scheme.htm
- https://dev.to/ovid/database-naming-standards-2061
- https://stackoverflow.com/questions/976185/what-are-some-of-your-most-useful-database-standards
- https://medium.com/@hwa610787/database-design-and-code-standard-89c0a4beee17
