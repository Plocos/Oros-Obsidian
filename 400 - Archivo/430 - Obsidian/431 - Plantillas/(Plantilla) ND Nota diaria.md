## {{title}}
<%* const currentMoment = moment(tp.file.title, "DD-MM-YYYY"); 
const hash = '# '; 
const slash = ' / '; 
const pipe = ' | '; 
const leftAngle = '❮ '; 
const rightAngle = ' ❯'; 
tR += leftAngle; 
tR += currentMoment.format('YYYY') + slash; tR += currentMoment.format('MMMM'); 
tR += rightAngle; tR += '\n'; 
tR += '\n'; tR += leftAngle;
currentMoment.add(-1,'days'); 
tR += '[[' + currentMoment.format('DD-MM-YYYY') + ']]' + pipe; currentMoment.add(1,'days'); 
tR += currentMoment.format('DD-MM-YYYY') + pipe; 
currentMoment.add(1,'days'); 
tR += '[[' + currentMoment.format('DD-MM-YYYY') + ']]'; 
currentMoment.add(-1,'days'); tR += rightAngle; %>

```dataviewjs
var tareas = dv.pages('"100 - Bandeja de entrada"').file.tasks

var trabajos = dv.pages('-"100 - Bandeja de entrada" and -"200 - Proyectos/+ Kanban proyectos" and -"400 - Archivo"').file.tasks
.where(t => {
	if(t.text.contains("```")){
		return false
	}
	//Descarta la tarea si ya esta completada
	if(t.completed == true){
		return false
	}
	return true
})
.sort(t => dv.date(t.due))

var results = tareas.concat(trabajos)

//var todaysDate = new Date(dv.current().file.name.toString())
//dv.el("p", todaysDate.toString())
var dateParts = dv.current().file.name.toString().split('-');
var year = parseInt(dateParts[2], 10);
var month = parseInt(dateParts[1], 10) - 1; // months are 0-based in JavaScript
var day = parseInt(dateParts[0], 10);

var todaysDate = new Date(year, month, day)

var late = []
var today = []
var future = []


if(results.length === 0) {
	dv.el("p", "No hay tareas existentes")
} else {
	results.forEach(r => {

	//Recupera el estado de cada una de las tareas para agruparlas y mostrarlo
	var state = r.section.toString().substring(r.section.toString().lastIndexOf(">") + 2, r.section.toString().lastIndexOf("]]"))

	//Descarta la tarea si ya esta en la columna hecho
	if(state.contains("Hecho")){
		//return false;
	}
	
	if(dv.current().file.name != "-"){
		var checkFecha = false
		r.outlinks.forEach(o => {
			var oString = o.toString()
			if(oString.contains("-20")){
				//Extrae la fecha del enlace
				var oDateParts
				if(oString.contains("@")){
					oDateParts = oString.substring(oString.lastIndexOf("@") + 1, oString.lastIndexOf("]]")).split('-');
				}else{
					oDateParts = oString.substring(oString.lastIndexOf("[") + 1, oString.lastIndexOf("]]")).split('-');
				}
				var oYear = parseInt(oDateParts[2], 10);
				var oMonth = parseInt(oDateParts[1], 10) - 1;
				var oDay = parseInt(oDateParts[0], 10);
				var oDate = new Date(oYear, oMonth, oDay)
				
				if(oDate < todaysDate){
					late.push(r)
					checkFecha = false
					return
				}else if(oDate == todaysDate){	
					today.push(r)
					checkFecha = true
					return
				}else{
					future.push(r)
					checkFecha = false
					return
				}
			}			
		})
		if(checkFecha == false){
			//return false
		}
	}})
}
dv.el("br")

dv.header(3, "Tareas atrasadas")
if(late.length === 0) {
	dv.el("p", "No quedan tareas atrasadas")
} else {
	var lastFile = ""
	var lastState = ""
	
	late.forEach(r => {
	//Recupera el archivo de cada una de las tareas para agruparlas y mostrarlo
	var file = "[[" + r.section.toString().substring(r.section.toString().indexOf("|") + 1, r.section.toString().indexOf(">") - 1) + "]]"
	
	//Recupera el estado de cada una de las tareas para agruparlas y mostrarlo
	var state = r.section.toString().substring(r.section.toString().lastIndexOf(">") + 2, r.section.toString().lastIndexOf("]]"))
	
	//Printeamos y cambiamos de file y refrescamos estados
	if(file != lastFile){
		dv.header(4, file)
		lastState = ""
	}
	lastFile = file

	//Printeamos y cambiamos de estado
	if(state != lastState){
		dv.header(5, state)
	}
	lastState = state
	
	dv.el("p", "- " + r.text)
	})
}
dv.el("br")

dv.header(3, "Tareas para hoy")
if(today.length === 0) {
	dv.el("p", "No hay tareas pendientes para hoy")
} else {
	var lastFile = ""
	var lastState = ""
	
	today.forEach(r => {
	//Recupera el archivo de cada una de las tareas para agruparlas y mostrarlo
	var file = "[[" + r.section.toString().substring(r.section.toString().indexOf("|") + 1, r.section.toString().indexOf(">") - 1) + "]]"
	
	//Recupera el estado de cada una de las tareas para agruparlas y mostrarlo
	var state = r.section.toString().substring(r.section.toString().lastIndexOf(">") + 2, r.section.toString().lastIndexOf("]]"))
	
	//Printeamos y cambiamos de file y refrescamos estados
	if(file != lastFile){
		dv.header(4, file)
		lastState = ""
	}
	lastFile = file

	//Printeamos y cambiamos de estado
	if(state != lastState){
		dv.header(5, state)
	}
	lastState = state
	
	dv.el("p", "- " + r.text)
	})
}
dv.el("br")

dv.header(3, "Tareas venideras")
if(future.length === 0) {
	dv.el("p", "No hay tareas por venir")
} else {
	var lastFile = ""
	var lastState = ""
	
	future.forEach(r => {
	//Recupera el archivo de cada una de las tareas para agruparlas y mostrarlo
	var file = "[[" + r.section.toString().substring(r.section.toString().indexOf("|") + 1, r.section.toString().indexOf(">") - 1) + "]]"
	
	//Recupera el estado de cada una de las tareas para agruparlas y mostrarlo
	var state = r.section.toString().substring(r.section.toString().lastIndexOf(">") + 2, r.section.toString().lastIndexOf("]]"))
	
	//Printeamos y cambiamos de file y refrescamos estados
	if(file != lastFile){
		dv.header(4, file)
		lastState = ""
	}
	lastFile = file

	//Printeamos y cambiamos de estado
	if(state != lastState){
		dv.header(5, state)
	}
	lastState = state
	
	dv.el("p", "- " + r.text)
	})
}
```

### 📙 Diario
Título:: 
Valoración:: 

##### Resumen de hoy:
Hora: {{time}}
Temas: 
Estado: 
Contenido:
