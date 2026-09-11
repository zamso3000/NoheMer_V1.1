# NoheMer — Sistema de Registro de Ventas e Inventario

Aplicación web local de un solo archivo (`index.html`) para el registro de ventas,
control de inventario de productos líquidos (litros) y generación de tickets, tanto
para WhatsApp como para impresión en papel.

> Funciona **sin internet** y **sin servidor**: el código se guarda todo en un solo
> archivo HTML que se abre directamente en el navegador (Chrome o Edge recomendados).

---

## 1. Cómo se usa

1. Abra `index.html` haciendo doble clic (protección `file://`).
2. Al iniciar aparece la ventana **Configuración de la Empresa**:
   - Nombre de la empresa o distribuidor.
   - RIF o Cédula (opcional).
   - Dirección (opcional).
   - Teléfono de contacto (opcional).
   - **Tasa del Dólar en Bs.** — se usa para calcular los totales en bolívares.
   - Pulse **"Guardar y Continuar"**.
3. Por primera vez, el navegador preguntará **dónde guardar el archivo JSON** que
   guarda todas las ventas (recomendado: carpeta *Descargas*, pulse *Guarde*/*Save*).
   A partir de entonces el archivo se actualiza automáticamente después de cada venta,
   cambio de inventario, respaldo o borrado de historial.

El reloj en la cabecera muestra la **fecha y hora actual en vivo** para saber que el
sistema está activo y en espera.

---

## 2. Pestañas

| Pestaña | Función |
| --- | --- |
| **Ventas** | Registrar una venta nueva (formulario principal). |
| **Buscar Cliente** | Buscar el historial de un cliente, reenviar o imprimir sus tickets y exportar a CSV. |
| **Inventario** | Ver productos y stock; añadir stock, crear, **editar** o **eliminar** productos. |
| **Copia de Seguridad** | Crear o restaurar respaldos completos. |
| **Historial de Ventas** | Leer y revisar el registro de ventas desde el archivo JSON. |

---

## 3. Registrar una venta (pestaña *Ventas*)

- Ingrese el **nombre del cliente** (obligatorio) y su **cédula** (opcional).
- Seleccione la **fecha de la venta**.
- Elija el **tipo de pago**: Pago Móvil, Transferencia, Zelle, Efectivo, Punto de Venta u Otro.
- Agregue los **productos de la venta**: seleccione el producto, ingrese la cantidad en
  litros y pulse **"Agregar"**. Puede agregar varios productos a la misma venta.
  - Los productos también se pueden agregar **manualmente en la pestaña Inventario**.
- El total en **USD** y en **Bs.** (usando la tasa del día) se recalcula en vivo.
- Pulse **"Registrar Venta"**. Al guardar:
  1. El stock del inventario se descuenta automáticamente.
  2. Las ventas se guardan en la **base de datos local** (IndexedDB) y en el **archivo JSON**.
  3. Aparece un menú para **enviar el ticket por WhatsApp**, **imprimirlo** o **continuar**.

### El ticket
- Incluye los datos de la empresa (recibo de compra, nombre, RIF/Cédula, dirección y
  teléfono), el cliente, la fecha, la lista de productos, el tipo de pago, los totales
  en USD y Bs. y la tasa usada.
- Se envía por WhatsApp como mensaje de texto y/o se imprime en una impresora térmica
  o de papel.

### Otros botones de la pantalla de ventas
- **Limpiar Formulario**: borra el formulario para comenzar una venta nueva.
- **Borrar Historial**: borra **toda** la base de datos local (ventas e inventario) y
  actualiza el JSON (requiere confirmación).
- **Cerrar App**: cierra la pestaña o ventana del navegador.

---

## 4. Buscar Cliente

- Escriba el **nombre** (o parte de él) y pulse **"Buscar Historial"**.
- Se muestran las ventas de ese cliente con sus totales.
- Por cada venta puede:
  - **Ver el ticket** o **reenviarlo por WhatsApp**.
  - **Imprimir** el ticket.
  - **Eliminar** la venta (también se actualiza el JSON).
- **"Exportar a CSV"**: descarga todas las ventas en un archivo CSV (abre con Excel)
  que incluye las columnas Cliente, Cédula, Fecha, Tipo de Pago, Producto, Cantidad,
  Precio (USD), Subtotal (USD), Total (USD), Total (Bs.) y Tasa BCV.

---

## 5. Inventario

- **Tabla de inventario**: muestra Producto, Precio (USD), Stock (Litros) y Acción.
- Por cada producto:
  - **Editar** ✏️: permite cambiar el nombre, el precio en USD y el stock en litros.
  - **Eliminar** 🗑️: borra el producto del stock (las ventas ya registradas **no**
    se alteran).
- **Añadir Stock a Producto Existente**: sumar litros a un producto sin cambiar demás datos.
- **Añadir Nuevo Producto al Sistema**: crea un producto nuevo pidiendo su precio en USD
  (el stock inicial es 0; luego se le añade stock).

> Al editar o eliminar productos, el JSON en *Descargas* se actualiza automáticamente.

---

## 6. Copia de Seguridad

- **Crear Respaldo**: descarga un archivo `.json` con **todo** (empresa, tasa, inventario
  y ventas). Guarde este archivo fuera del equipo (USB, correo, nube) como respaldo.
- **Restaurar Respaldo**: pulsa el botón, selecciona el archivo `.json` anterior y la
  app reconstruye el inventario y el historial en la base de datos local.

---

## 7. Historial de Ventas

- Muestra las ventas registradas en el **archivo JSON** de *Descargas* (el botón
  **"Actualizar"** relee el archivo).
- **"Cargar desde JSON"**: sirve para elegir un archivo JSON distinto.
- Si el archivo aún no existe (primera vez), la app lo muestra desde la base de datos local.
- Cada venta puede **reenviarse por WhatsApp** o **imprimirse**.

---

## 8. Datos y archivos

### Archivo JSON de ventas (en *Descargas*)
Se llama `registro_<empresa>_ventas.json` y contiene:

```json
{
  "empresa": { "nombre": "...", "rif": "...", "direccion": "...", "telefono": "..." },
  "tasaDolar": 36.50,
  "ultimaActualizacion": "2026-09-11T12:00:00.000Z",
  "inventario": [ { "producto": "...", "precio": 1.50, "stock": 20.5 } ],
  "ventas": [
    {
      "id": 1,
      "empresa": "...", "empresaRif": "...", "empresaDireccion": "...", "empresaTelefono": "...",
      "cliente": "...", "cedula": "...", "fecha": "2026-09-11", "tipoPago": "Efectivo",
      "items": [ { "productId": "...", "nombreProducto": "...", "cantidad": 1, "precio": 1.50, "subtotal": 1.50 } ],
      "totalUsd": 1.50, "totalVes": 54.75, "tasaBcv": 36.50
    }
  ]
}
```

### Dónde se guardan los datos
| Dato | Lugar |
| --- | --- |
| Nombre de empresa, RIF, dirección, teléfono, tasa y tema | `localStorage` del navegador |
| Ventas e inventario | Base de datos local **IndexedDB** (`NohemerVentasDB`) |
| Ruta del archivo JSON en *Descargas* | IndexedDB (se vuelve a pedir si se borran los datos del sitio) |
| Respaldos JSON | Archivos `.json` que usted descarga |

> Los datos viven **dentro de este navegador/equipo**. Limpiar los *Datos del sitio*
> del navegador borrará la base local; por eso se recomienda crear respaldos con
> frecuencia.

---

## 9. Notas técnicas

- Librería **Dexie.js** (CDN de unpkg) para la base de datos IndexedDB.
- El guardado automático en *Descargas* usa la **File System Access API** de Chrome/Edge.
- Funciona 100% sin conexión (excepto la primera carga de Dexie desde el CDN).
- Tema claro/oscuro con el botón 🌙 de la cabecera (se recuerda entre sesiones).
- Se recomienda **Chrome u Opera** para el guardado automático del JSON en Descargas.
```