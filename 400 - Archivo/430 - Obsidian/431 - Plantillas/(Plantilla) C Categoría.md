---
Pertenece a:
tags:
Query: ""
---

Búsqueda: `INPUT[text(placeholder(Nombre archivo)):Query]`

```dataviewjs
var query = dv.current().file.frontmatter.Query

var results = dv.current().file.inlinks.where(r => {
	//Extra el nombre del inlink a traves de su ruta
	var name = "" + r.path.substring(r.path.lastIndexOf("/") + 1, r.path.lastIndexOf("."))
	
	if(query != null){
		if(!name.contains(query)){
			return false
		}
		return true
	}
	return true
})

//Crea un array y lo rellena con el tipo de cada archivo encontrado usando el path
var typeOfResults = []

results.forEach(r => {
	if(r.path.contains("200 - Proyectos") && !r.path.contains("210 - Acciones")){
		typeOfResults.push("Proyecto")
	}else if(r.path.contains("310 - Categorías")){
		typeOfResults.push("Categoría")
	}else if(r.path.contains("320 - Notas")){
		typeOfResults.push("Nota")
	}
})

//Queda por añadir el numero de inlinks del inlink
if(results.length === 0) {
	dv.el("p", "No hay resultados que mostrar")
} else {
	dv.table(["Nombre", "Tipo"], results.map((r, index) => [r, "Tipo: " + typeOfResults[index].toString()]))
}

```