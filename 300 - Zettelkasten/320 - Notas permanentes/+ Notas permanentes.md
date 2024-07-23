> [!Cajón de notas permanentes]+
>```dataview
>table without id
>file.link as "🗃 Notas permanentes", length(file.inlinks) as "Número de notas asociadas"
>from #Nota/Permanente AND !"500 - Archivo/540 - Obsidian"
>sort file.name asc
>```