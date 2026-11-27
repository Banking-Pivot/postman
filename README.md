# Colecciones Postman del sistema bancario

Pruebas de las APIs del sistema bancario. Todas las peticiones van por el API Gateway y cada una comprueba el código HTTP de la respuesta y, en los errores, el código del cuerpo.

## Qué hay

| Archivo | Contenido |
|---|---|
| `banking-business-rules.postman_collection.json` | Colección general: todas las reglas de negocio, una carpeta por dominio y una por regla, con un ejemplo guardado de cada respuesta. |
| `services/auth-security` | Inicio de sesión, roles, tokens de servicio y bloqueo por contraseñas incorrectas. |
| `services/customer-crud` | Clientes: tipos, perfiles, documento único y desactivación. |
| `services/product-catalog` | Catálogo de productos: términos por familia y quién puede abrir cada producto. |
| `services/account-operations` | Cuentas: producto según el cliente, titulares y firmantes, comisiones, plazo fijo, transferencias y cierre. |
| `services/credit-operations` | Créditos y tarjetas de crédito: límites, consumos, pagos, vencimiento y mora. |
| `services/credit-payment` | Pago de créditos desde una cuenta y límites del pago con tarjeta de débito. |
| `services/debit-cards` | Tarjetas de débito: cuentas asociadas, cuenta principal, bloqueo y límite de tarjetas. |
| `services/yanki-wallet` | Monedero Yanki: recargas, pagos por celular, asociación a una tarjeta y límites. |

La colección general reúne las ocho de `services/`. Para una demostración basta con ella; las de `services/` sirven para probar un solo servicio.

## Cómo usarlas

1. Levanta el sistema completo. La guía está en el README del repositorio [infra](https://github.com/Banking-Pivot/infra).
2. Importa la colección en Postman y ejecútala completa, o una carpeta de dominio completa. Dentro de un dominio las carpetas dependen del orden: cada una usa los clientes, cuentas y tarjetas que crearon las anteriores.

Con el Postman CLI:

```bash
postman collection run banking-business-rules.postman_collection.json
```

No hace falta un entorno de Postman. Las colecciones guardan sus valores en variables de colección:

| Variable | Valor por defecto | Para qué |
|---|---|---|
| `baseUrl` | `http://localhost:8080` | Dirección del API Gateway. |
| `username` | `admin` | Usuario con el que la colección inicia sesión sola. |
| `password` | La de desarrollo local | Debe coincidir con `AUTH_ADMIN_PASSWORD` del archivo `.env` de infra. |

Cada corrida crea clientes, cuentas y operaciones nuevas, así que se puede repetir sobre la misma base de datos.

## Reglas de mora

La carpeta «Mora con el reloj adelantado» de créditos se salta sola en una corrida normal, porque necesita créditos vencidos. Para ejecutarla, arranca `credit-service` con `CREDIT_CLOCK_OFFSET=40d` y pon en `true` la variable `cre_clockOffsetActive` de la colección general (`clockOffsetActive` en `services/credit-operations`). Los pasos están en el README de infra.

## Mantenimiento

Las colecciones de `services/` son copia de las que viven en la carpeta `postman/` de cada servicio, junto al código que prueban. Cuando una cambia, se copia aquí y se vuelve a generar la colección general, que no se edita a mano.
