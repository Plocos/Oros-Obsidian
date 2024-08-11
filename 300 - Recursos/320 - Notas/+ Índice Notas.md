---
Query: ""
---
# Índice de Notas

Búsqueda: `INPUT[text(placeholder(Nombre nota)):Query]`

```dataviewjs
var query = dv.current().file.frontmatter.Query

var results = dv.pages('"300 - Recursos/320 - Notas" and -"300 - Recursos/320 - Notas/+ Índice Notas"').file.where(r => {
	if(query != null){
		if(!r.name.toLowerCase().contains(query.toLowerCase())){
			return false;
		}
	}
	if(r.frontmatter.Fuente == null){
		r.frontmatter.Fuente = ""
	}else{
	r.frontmatter.Fuente = "Fuente: " + r.frontmatter.Fuente
	}
	return true;
})

dv.header(4, "Resultados")
if(results.length === 0) {
	dv.el("p", "No hay resultados que mostrar")
} else {
	dv.table(["Notas", "Fuente"], results.map(r => ["![[" + r.name + "]]", "Fuente: " + r.frontmatter.Fuente]))
}

//TEORICAMENTE ESTO FORMATEA
dv.container.classList.add("cv-jobs");

```