## Cómo ejecutar

Abre el archivo `index.html` directamente en el navegador. No se requiere instalación ni compilación.

Como alternativa, si tienes Node.js y npm instalados, ejecuta desde la raíz del repositorio:

```sh
npx serve .
```

Abre en el navegador la URL local que indique el servidor. No es necesario instalar dependencias del proyecto.

## Comprobaciones manuales — AJU-7

No hay infraestructura de tests automatizados. Ejecutar estas comprobaciones en Safari iOS y en un navegador de escritorio; registrar navegador/versión y resultado. Esta lista no implica que se hayan ejecutado.

1. **Vacío y edición:** al abrir, comprobar «Footer Fijo» y `0/80`. Escribir `a`: la vista previa debe ser `a` y el contador `1/80`. Borrar: vuelve al estado inicial. Escribir, pegar y borrar en distintas posiciones: el footer siempre refleja el valor aceptado.
2. **Límites:** pegar `1234567890` siete veces y después `123456789`: debe indicar `79/80`. Escribir `0`: `80/80`, todavía sin error. Intentar escribir `X`: el valor no cambia, el contador pasa a rojo y aparece `!`. Repetir el intento: no debe repetirse el anuncio accesible. Borrar un carácter: `79/80`, sin rojo ni aviso. Volver a llenar e intentar excederlo debe anunciar de nuevo el límite.
3. **Pegado y selección:** en vacío, pegar nueve copias de `1234567890`: solo se aceptan ocho (`80/80`, estado de límite). Con ese valor, seleccionar los primeros diez caracteres y pegar `ABCDEFGHIJKL`: se aceptan `ABCDEFGHIJ` y se conservan los otros 70 caracteres. Reemplazar la selección por menos caracteres y comprobar que se elimina el estado de límite. En `79/80`, intentar insertar dos letras en medio: se acepta una y se conserva el resto del texto.
4. **Unicode e IME:** pegar 80 copias de `😀`: `80/80`, sin cortar ningún emoji; intentar otro activa el límite. Cada emoji simple cuenta como un carácter; los grafemas compuestos pueden contar como varios puntos de código. Con un teclado japonés o chino, componer y confirmar texto tanto por debajo del máximo como junto al límite: no se debe interrumpir la composición; al confirmarla quedan como máximo 80 puntos de código. Borrar y volver a componer también debe funcionar.
5. **Texto literal:** pegar `<img src=x onerror=alert(1)>`: debe verse literalmente en el footer, sin crear imágenes, ejecutar código ni abrir alertas.
6. **Layout:** a 320 px de ancho, pegar 80 letras `W`. La vista previa ocupa una línea y termina con puntos suspensivos; el contador y `!` siguen visibles. El footer mide 50 px, no crece ni provoca scroll horizontal. Comprobar también texto con espacios y texto de derecha a izquierda, y aumentar el tamaño de texto del sistema.
7. **Accesibilidad:** con VoiceOver/Safari (y, si está disponible, NVDA), enfocar el input: se anuncia su etiqueta y ayuda del máximo. Navegar al footer: el texto aceptado completo debe estar disponible aunque esté truncado visualmente; el contador debe leerse como «n de 80 caracteres». La vista previa no debe anunciarse con cada pulsación. Intentar exceder 80: un único aviso cortés de límite alcanzado por cada entrada en ese estado; el estado también se distingue por `!`, no solo por color.
8. **Regresión iOS:** en un dispositivo real, abrir/cerrar el teclado, escribir, desplazar la página y cambiar la orientación con y sin teclado. El footer debe seguir sobre el teclado sin cambiar su altura. Repetir con el input vacío y lleno.
9. **Alternativa sin visualViewport:** en un navegador/perfil sin esa API (o antes de recargar, mediante una anulación de entorno de pruebas), repetir edición y redimensionado. El footer debe continuar anclado al borde inferior calculado con `window.innerHeight`.
