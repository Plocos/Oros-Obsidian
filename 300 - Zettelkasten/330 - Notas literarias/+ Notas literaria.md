> [!Cajón de notas literarias]+
>```dataview
>table without id
>file.link as "🗃 Notas literarias", length(file.inlinks) as "Número de notas asociadas"
>from #Nota/Literaria  AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```