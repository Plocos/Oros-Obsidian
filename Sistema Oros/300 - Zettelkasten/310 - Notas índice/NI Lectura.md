---
tags:
  - Nota/Índice
Nota índice:
---

> [!Cajón de notas índice]+
>```dataview
>table without id
>file.link as "🗃 Notas índice", length(file.inlinks) as "Número de notas vinculadas"
>from [[]]  AND #Nota/Índice AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```

> [!Cajón de proyectos]+
>```dataview
>table without id
>file.link as "🗃 Proyectos"
>from [[]]  AND #Proyecto AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```

> [!Cajón de notas permanentes]+
>```dataview
>table without id
>file.link as "🗃 Notas permanentes", length(file.inlinks) as "Número de notas vinculadas"
>from [[]] AND #Nota/Permanente AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```

> [!Cajón de notas literarias]+
>```dataview
>table without id
>file.link as "🗃 Notas literarias", length(file.inlinks) as "Número de notas vinculadas"
>from [[]] AND #Nota/Literaria  AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```

> [!Cajón de notas de referencia]+
>```dataview
>table without id
>file.link as "🗃 Notas de referencia", length(file.inlinks) as "Número de notas vinculadas"
>from [[]] AND #Nota/Referencia   AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```