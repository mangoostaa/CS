# Archivos de Counter-Strike 1.6

Los archivos provienen de `C:\Games\Counter-Strike 1.6\cstrike`.

- `models/weapons/v_ak47.mdl`, `v_m4a1.mdl`, `v_awp.mdl` y `v_deagle.mdl`: copias verificadas con SHA-256. En el juego están directamente en `cstrike/models`, sin el subdirectorio `weapons`.
- `gfx/env/dust2.wad`: WAD3 generado con las 44 texturas referenciadas por el mapa. Reúne las 15 texturas incluidas en el BSP y las 29 restantes de los WAD de la instalación. `texture-sources.json` registra el origen de cada textura; los datos se verificaron byte por byte.
- `textures/png`: las 44 texturas exportadas a PNG con VTFCmd de VTFLib 1.3.2, incluidas las 15 de `de_dust2_generated.wad`. Se conservaron sus nombres, dimensiones y colores originales; cada PNG se verificó píxel por píxel. El informe está en `textures/conversion.json`.
- `maps/de_dust2.bsp`: copia del mapa original, GoldSrc BSP versión 30.
- `maps/de_dust2.map`: reconstrucción editable realizada con Half-Life Unified SDK Map Decompiler 1.0.0.0, estrategia Tree.
- `maps/de_dust2.vmf`: conversión real del MAP a VMF mediante Sledge.Formats. Se verificó su lectura, 1.624 sólidos, 9.990 caras, 100 entidades, identificadores únicos y alineación de texturas. La diferencia máxima de distancia de los planos durante la conversión fue de 0,001221 unidades. El informe está en `maps/de_dust2.verification.json`.
- `maps/de_dust2.blend`: escena editable de Blender 4.5 con el BSP original importado mediante SourceIO 5.5.4, materiales preparados con PNG, iluminación de día y cámara de vista general. Las imágenes están empaquetadas dentro del archivo. Para continuar la edición, abre este archivo con Archivo > Abrir en Blender.
- `maps/de_dust2.glb`: mapa exportado como glTF 2.0 binario, con 39 objetos de malla, 9.932 triángulos, 34 texturas incrustadas y 12 luces. Se reimportó en Blender y se verificaron el número de triángulos y las dimensiones, sin diferencias de escala. No necesita archivos de textura externos.
- `maps/de_dust2-preview.png`: vista general renderizada después de reimportar el GLB real. `maps/de_dust2-glb.verification.json` contiene la verificación de exportación.

No existen `cstrike.pak` ni `gfx/env/dust2.wad` en esta instalación; los modelos ya estaban sueltos y el WAD se ensambló a partir de los recursos reales del mapa.

BSPSource no admite GoldSrc. Se utilizó el [decompilador de Half-Life Unified SDK](https://github.com/twhl-community/HalfLife.UnifiedSdk.MapDecompiler/releases/tag/V1.0.0.0) y después se convirtió el formato del archivo de mapa.

El BSP no conserva los pinceles originales completos: el mapa decompilado puede contener geometría reconstruida que necesite ajustes. El VMF conserva las entidades y los nombres de texturas de GoldSrc; no es un port listo para compilar en Counter-Strike: Source. Los WAD y los MDL siguen siendo formatos GoldSrc y, para usarlos en Source, requieren convertir materiales y modelos. El campo `wad` del MAP conserva las rutas históricas del BSP: al editarlo en Hammer de GoldSrc o J.A.C.K., añade `gfx/env/dust2.wad` desde esta carpeta a la configuración de texturas.

Las herramientas y el conversor utilizado están en `C:\Users\Noxi-PC\Downloads\CS16-Tools`.

Para crear el GLB se utilizó directamente el BSP original de GoldSrc, que SourceIO puede importar, conservando sus superficies y coordenadas UV. SourceIO no importa VMF. Se quitaron de la geometría visible las caras de herramientas como SKY, CLIP y AAATRIGGER. La escala es de 0,0254 metros por unidad GoldSrc. Las luces se exportaron mediante `KHR_lights_punctual`; el programa que abra el GLB debe admitir esa extensión para mostrarlas. La iluminación ambiental del World de Blender está guardada en el BLEND, pero no forma parte del estándar GLB y debe configurarse en el visor o motor de destino. SourceIO quedó activado en las preferencias guardadas; si Blender ya estaba abierto, reinícialo para que cargue ese cambio.

Para exportar PNG, VTFCmd utiliza `-exportformat png`; `-format` selecciona el formato interno VTF. La conversión del WAD se realizó extrayendo primero las imágenes TGA, pasando por VTF RGBA8888 sin compresión y exportando a PNG. En las texturas de dimensiones no potencia de dos se añadió margen temporal y después se recortó, evitando redimensionar sus píxeles. El paquete VTFLib está en `C:\Users\Noxi-PC\Downloads\vtflib132-bin.zip`.
