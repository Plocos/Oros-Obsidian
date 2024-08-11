---
Query: ""
---
# Índice de Categorías

Búsqueda: `INPUT[text(placeholder(Nombre categoría)):Query]`

```dataviewjs
var query = dv.current().file.frontmatter.Query

var results = dv.pages('"300 - Recursos/310 - Categorías" and -"300 - Recursos/310 - Categorías/+ Índice Categorías"').file.where(r => {
	if(query != null){
		if(!r.name.toLowerCase().contains(query.toLowerCase())){
			return false;
		}
	}
	return true;
}).sort(k => k.inlinks.length, 'desc')


dv.list(results.text)
dv.paragraph(results.text)

dv.header(4, "Resultados")
if(results.length === 0) {
	dv.el("p", "No hay resultados que mostrar")
} else {
	dv.table(["Categorías", "Notas asociadas"], results.map(r => [r.link, "Notas asociadas: " + r.inlinks.length]));
}

```
