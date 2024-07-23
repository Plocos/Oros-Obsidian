> [!Cajón de notas de referencia]+
>```dataview
>table without id
>file.link as "🗃 Notas de referencia", length(file.inlinks) as "Número de asociadas"
>from #Nota/Referencia   AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```