# NoheMer_V1.1
Antes de comenzar solo coloca la taza actual del Dolar$

Control Inventario
Características y Componentes Principales

Interfaz Dinámica y Responsiva:

Navegación por pestañas (Ventas e Inventario).

Soporte para modo claro y modo oscuro (con persistencia mediante localStorage).

Estilizado nativo optimizado para dispositivos móviles y de escritorio.

Gestión Multimoneda (USD / Bolívares):

Control manual e integración del tipo de cambio del Banco Central de Venezuela (BCV).

Conversión automática e instantánea de los montos totales de venta de USD a Bs.

Módulo de Ventas:

Selección de productos con cálculo de subtotal por cantidad en litros.

Formulario con soporte para datos de cliente (Nombre y Cédula de Identidad).

Historial de ventas con opción de búsqueda por cliente y exportación de datos a formato CSV.

Módulo de Inventario:

Control de existencia en stock por litros para cada producto.

Opciones para reabastecer stock de artículos existentes y registrar nuevos productos en el catálogo.

Persistencia y Respaldo de Datos:

Almacenamiento Local: Utiliza la librería Dexie.js (sobre IndexedDB) para guardar ventas e inventarios sin requerir conexión a un servidor backend.

Gestión de Respaldos: Incluye funcionalidades para crear copias de seguridad en formato JSON y restaurar datos desde un archivo.
