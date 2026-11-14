# Reglas de Seguridad de Firestore (`distribuidoresfnc`)

Este archivo (`firestore.rules`) protege la base de datos **`frescura-admin`** para evitar que una distribuidora pueda manipular desde el navegador sus puntos, comisiones o el estado de sus pedidos.

## Qué protege
1. **`distribuidores/{uid}`:**
   - Al registrarse, exige iniciar con `puntos_disponibles == 0`, `puntos_historicos == 0` y `comision_pagada == false`.
   - Al actualizar, la distribuidora solo puede editar sus datos de perfil, meta o consumir puntos (`puntos_disponibles <= valor anterior`); **nunca** puede sumarse puntos ni listar a otras distribuidoras.
2. **`pedidos_dist/{pedidoId}`:**
   - Solo permite crear pedidos propios en estado `'pendiente'` y con `total >= 70000`.
   - Solo permite editar un pedido mientras siga en estado `'pendiente'`. Una vez confirmado o anulado por el administrador, queda bloqueado.
3. **`canjes/{canjeId}`:**
   - La distribuidora solo puede crear solicitudes de canje propias en estado `'pendiente'`.
4. **`admin/{docId}` y `recompensas/{recId}`:**
   - Lectura permitida a distribuidoras autenticadas; escritura exclusiva para administradores.

## Cómo activarlas en Firebase Console (3 pasos)
1. Entra a **Firebase Console** → proyecto **`frescura-admin`** → **Authentication → Users** y copia el **User UID** de tu cuenta de administrador.
2. Ve a **Firestore Database → Data** → crea (si no existe) la colección **`admins`** y agrega un documento cuyo **Document ID** sea exactamente tu **User UID** (con un campo simple, por ejemplo `activo: true`).
3. Ve a **Firestore Database → Rules**, pega el contenido de [`firestore.rules`](firestore.rules) y haz clic en **Publish**.
