var app = {
	nombre:"Actualizacion y Recargos de Impuestos",
	usuario:{nombre:"",empresa:"",email:""},
	ban_guardandoDatos:0,
	calcular_recargos:function(recargos){
		var porcentaje_recargos = 0;
		var contador ;
		if($("#fechaI_dia").val() < $("#fechaII_dia").val() ){
			porcentaje_recargos = 2.26;
			contador = recargos.length - 1;
		}else{
			contador = recargos.length ;
		}
		for(var i = 0; i < contador ; i++){
			porcentaje_recargos = parseFloat(porcentaje_recargos+parseFloat(recargos[i].por));
		}
		
		return porcentaje_recargos;
	},
	makeCalc:function(){
		var user = $("#registro_usuarioNombre").val();
		var password = $("#registro_usuarioEmail").val();
	
		var fecha_limite = $("#fechaI_anio").val()+"-"+$("#fechaI_mes").val()+"-"+$("#fechaI_dia").val()
		var fecha_pago = $("#fechaII_anio").val()+"-"+$("#fechaII_mes").val() +"-"+$("#fechaII_dia").val()

		$.ajax({
			url:"http://solucionorganizacional.mx/calculadora_app/server.php",
			type:"GET",
			dataType: "text",
			data:{c:1,fecha_limite:fecha_limite,fecha_pago:fecha_pago,user:user,password:password},
			success:function(d){
				var datos = d.split("--||--");
				var inpcs = JSON.parse(datos[0]);
				var recargos_data = JSON.parse(datos[1]);
				var inpcI = inpcs[0];
				var inpcII = inpcs[1];
				var inpcI_label = app.getMonthName(inpcI.fecha);
				var inpcII_label = app.getMonthName(inpcII.fecha);

				var factor_actualizacion = (parseFloat(inpcII.valor/inpcI.valor)-1).toFixed(5);
				factor_actualizacion = String(factor_actualizacion);
				factor_actualizacion = factor_actualizacion.substring(0, 6);
				factor_actualizacion = parseFloat(factor_actualizacion);
				

				var recargosR = parseFloat(app.calcular_recargos(recargos_data));
				var importe = parseFloat($("#importe").val());
				var actualizacion = parseFloat(importe*factor_actualizacion);
				var recargos = ((recargosR/100)*(importe+actualizacion));
				var total =  Math.round(parseFloat((importe+actualizacion)+recargos)).toFixed(2);	
				
				$("#desp_inpcI").html(parseFloat(inpcI.valor).toFixed(4));
				$("#desp_inpcII").html(parseFloat(inpcII.valor).toFixed(4));
				$("#mesann_i").html(inpcI_label);
				$("#mesann_ii").html(inpcII_label);
				$("#desp_factorActualizacion").html((parseFloat(factor_actualizacion)));
				$("#desp_actualizacion").html(Math.abs(parseFloat(actualizacion)).toFixed(4));
				$("#desp_recargos").html(parseFloat(recargos).toFixed(4));
				$("#recargos_por").html( (parseFloat(recargosR).toFixed(2)) + "%");
				$("#desp_total").html("$"+total);
				
			}
		});
	},
	getMonthName:function(date){
		var dateSplit = date.split("-");
		var abreviaturaMes = "AAA";
		var abreviaturaAnn = "00";
		switch(dateSplit[1]){
			case "01":
				abreviaturaMes = "ENE";
			break;
			case "02":
				abreviaturaMes = "FEB";
			break;
			case "03":
				abreviaturaMes = "MAR";
			break;
			case "04":
				abreviaturaMes = "ABR";
			break;
			case "05":
				abreviaturaMes = "MAY";
			break;
			case "06":
				abreviaturaMes = "JUN";
			break;
			case "07":
				abreviaturaMes = "JUL";
			break;
			case "08":
				abreviaturaMes = "AGO";
			break;
			case "09":
				abreviaturaMes = "SEP";
			break;
			case "10":
				abreviaturaMes = "OCT";
			break;
			case "11":
				abreviaturaMes = "NOV";
			break;
			case "12":
				abreviaturaMes = "DIC";
			break;
		}
		//abreviaturaAnn = dateSplit[0].substring(2, 4);
		return abreviaturaMes+"-"+dateSplit[0];
	}
};

var widthWin,heightWin;
$(document).ready(function(){
	resolution();
	$("#calc_01").click(function(){
		loginOK();
		//app.loadDates();
	});
	$("#makeCalc").click(function(){
		app.makeCalc();
	});
	//Registro usuario
	//localStorage.removeItem("terminosCondiciones");
	
	
	var local_terminosCondiciones = localStorage.getItem("terminosCondiciones");
	if(local_terminosCondiciones === null){
		$("#terminos_condiciones").css("display","block");
	}else{
		$("#registro").css("display","block");
	}
	
	$("#terminos_boton").click(function(){
		localStorage.setItem("terminosCondiciones","true");
		$("#terminos_condiciones").css("display","none");
		$("#registro").fadeIn();
	});
	$("#registro_boton").click(function(){
		if(app.ban_guardandoDatos == 0){
			app.ban_guardandoDatos = 1;
			$("#registro_boton").val("Un momento...");
			
			var usuarioNombre = $("#registro_usuarioNombre").val();
			var usuarioEmail = $("#registro_usuarioEmail").val();
			
			if(usuarioNombre != ""){
				$("#registro_usuarioNombre_div").css("background","rgb(255, 255, 255)");
				if(usuarioEmail != ""){
					$("#registro_usuarioEmail_div").css("background","rgb(255, 255, 255)");
					//AJAX guardar datos en servidor.
					var user = $("#registro_usuarioNombre").val();
					var password = $("#registro_usuarioEmail").val();				
					$.ajax({
						url:"http://solucionorganizacional.mx/calculadora_app/server.php",
						type:"GET",
						dataType: "text",
						data:{c:0,user:user,password:password},
						success:function(d){
							if(d == "0"){
								alert("Usuario y/o contraseña invalidos.");
								app.ban_guardandoDatos = 0;
								$("#registro_boton").val("Continuar");
							}else if(d == "-0"){
								alert("Membresia caducada");
								$("#registro_boton").val("Continuar");
								app.ban_guardandoDatos = 0;
							}else{
								var datosUsuario = JSON.parse(d);
								app.usuario.nombre = datosUsuario[1];
								app.usuario.empresa = datosUsuario[2];
								$("#registro").css("display","none");
								$("#lista_calculadoras").fadeIn();
								app.ban_guardandoDatos = 0;
								$("#registro_boton").val("Continuar");
							}
						}
					});
				}else{
					$("#registro_boton").val("Continuar");
					$("#registro_usuarioEmail_div").css("background","#FFDDDD");
					app.ban_guardandoDatos = 0;
				}
			}else{
				$("#registro_boton").val("Continuar");
				$("#registro_usuarioNombre_div").css("background","#FFDDDD");
				app.ban_guardandoDatos = 0;
			}
		}
	});
	
	$("#container").css("display","block");
});

function resolution(){
	widthWin = window.innerWidth;
	heightWin = window.innerHeight;
	$("#container").css("width",widthWin+"px");
	$("#container").css("height",heightWin+"px");
	$(".sx").css("width",widthWin+"px");
	$(".sxII").css("width",(widthWin*2)+"px");
	$(".sxIII").css("width",(widthWin*3)+"px");
	//APP:Actualizacion y Recargos de Impuestos
	$(".select").css("width",((widthWin/2)-8)+"px");
	$(".inputCantidad").css("width",((widthWin/2)-20)+"px");
	$(".input_registro").css("width",(widthWin-12)+"px");
	$("#registro_boton").css("width",(widthWin-10)+"px");
	//***
}
function loginOK(){
	$("#container_con").css("margin-left","-"+widthWin+"px");
}
function compartir(red){
	var msg = "Calculadora de "+app.nombre+" ¡No te quedes sin ella! ¡Adquiérela YA!";
	if(red == "tw"){
		window.plugins.socialsharing.shareViaTwitter(msg, null /* img */, 'http://www.solucionorganizacional.mx/app');
	}else if(red == "fb"){
		window.plugins.socialsharing.shareViaFacebook(msg, null /* img */, 'http://www.solucionorganizacional.mx/app', function(){}, function(errormsg){});
	}else if(red == "em"){
	
		var msgEmail = "";
		
		var fechaI = $("#fechaI_dia").val()+"-"+$("#fechaI_mes").val()+"-"+$("#fechaI_anio").val();
		var fechaII = $("#fechaII_dia").val()+"-"+$("#fechaII_mes").val()+"-"+$("#fechaII_anio").val();
		var cantidad = $("#importe").val();
		
		msgEmail += "<b>Datos ingresados</b> <br><br>";
		msgEmail += "<b>Fecha Lim. Pago:</b> "+fechaI+"<br>";
		msgEmail += "<b>Fecha de Pago:</b> "+fechaII+"<br>";
		msgEmail += "<b>Cantidad:</b> "+cantidad+"<br><br>";

		
		var inpci = $("#desp_inpcI").html();
		var inpcii = $("#desp_inpcII").html();
		var inpci_fecha = $("#mesann_i").html();
		var inpcii_fecha = $("#mesann_ii").html();
		var actualizacion = $("#desp_actualizacion").html();
		var fActualizacion = $("#desp_factorActualizacion").html();
		var actualizacion = $("#desp_actualizacion").html();
		var recargos = $("#desp_recargos").html();
		var totalpagar = $("#desp_total").html();
		
		msgEmail += "<b>Desglose de cálculo</b> <br><br>";
		msgEmail += "<b>INPC 1</b> "+inpci_fecha+"<b>:</b> "+inpci+"<br>";
		msgEmail += "<b>INPC 2</b> "+inpcii_fecha+"<b>:</b> "+inpcii+"<br>";
		msgEmail += "<b>F. Actualización:</b> "+fActualizacion+"<br>";
		msgEmail += "<b>Actualización:</b> "+actualizacion+"<br>";
		msgEmail += "<b>Recargos:</b> "+recargos+"<br>";
		msgEmail += "<b>Total a pagar:</b> "+totalpagar+"<br><br><br>";
		msgEmail += "FUNDAMENTO:<br>";
		msgEmail += "ACTUALIZACIÓN Y RECARGOS DE PAGOS EXTEMPORANEOS CON FECHA DE VENCIMIENTO 17 DE CADA MES<br>";
		msgEmail += "CÓDIGO FISCAL DE LA FEDERACION ART. 17 A Y ART. 21";
		window.plugins.socialsharing.shareViaEmail(msgEmail, "Actualización y Recargo de Impuestos", null, null, null, ["http://www.solucionorganizacional.mx/calculadora_app/banner-shared.jpg"], function(){}, function(errormsg){});
	}
}