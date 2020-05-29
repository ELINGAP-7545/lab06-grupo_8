# lab06 Diseño de banco de Registro
## Estudiantes: 
Luis Eduardo Cardenas Ortiz   COD: 41458     CC:1077087937

Mateo Luzardo Jimenez         COD: 61063     CC:1026296907
# Introducción

en este trabajo que vamos a presentar una simulacion sobre el banco de registro el cual cuenta con 4 bits de salida y 2 de entradas, muchos circuitos digitales necesitan inicializarse antes de comenzar a trabajar normalmente. Su funcionamiento se divide en un estado de arranque, donde se inicializan los valores de los registros y un estado de régimen permanente donde se realiza la función para la que han sido diseñados. 

Para lograr esto debemos generar un circuito de inicializacion de 5000 giros por segundo " hay que tener en cuenta que este dato solo es necesario para la pagina web de laboratorios en linea el cual es LABSLAND" Al llegar el primer flanco de reloj, se captura el 1 y se saca por su salida, generando el flanco de subida para realizar la inicialización. Para el resto de ciclos de reloj esta señal siempre estará a 1

## Código Banco de Registro

```verilog
module BancoRegistro (      	 
  
//   #( Parametros
                  
parameter BIT_ADDR = 8,  //   BIT_ADDR Número de bit para la dirección
         
parameter BIT_DATO = 4,  //  BIT_DATO  Número de bit para el dato
	      
parameter   RegFILE= "src/Reg16.men")
	
(
    
input [BIT_ADDR-1:0] addrRa,
    
input [BIT_ADDR-1:0] addrRb,
    
	 
output [BIT_DATO-1:0] datOutRa,
   
output [BIT_DATO-1:0] datOutRb,
    
	 
input [BIT_ADDR:0] addrW,
   
input [BIT_DATO-1:0] datW,
    
	 
input RegWrite,
   
input clk,
   
input rst
);

// La cantidad de registros es igual a: 

localparam NREG = 2 ** BIT_ADDR;
  
//configuración del banco de registro 

reg [BIT_DATO-1: 0] breg [NREG-1:0];

assign  datOutRa = breg[addrRa];

assign  datOutRb = breg[addrRb];

always @(posedge clk) begin
	
if (RegWrite == 1)
    
breg[addrW] <= datW;
  
end

  
initial begin
	
//$readmemh(RegFILE, breg);

end

endmodule
```

## TestBench


```verilog
module TestBench;

// Inputs
	
reg [3:0] addrRa;
	
reg [3:0] addrRb;
	
reg [4:0] addrW;
	
reg [3:0] datW;
	
reg RegWrite;
	
reg clk;
	
reg rst;

// Outputs
	
wire [3:0] datOutRa;
	
wire [3:0] datOutRb;

	
// Instantiate the Unit Under Test (UUT)
	
BancoRegistro uut (
	
.addrRa(addrRa), 
	
.addrRb(addrRb), 
	
.datOutRa(datOutRa), 
	
.datOutRb(datOutRb), 
	
.addrW(addrW), 
	
.datW(datW), 
	
.RegWrite(RegWrite), 
	
.clk(clk), 
	
.rst(rst)
	
);

initial begin
	
// Initialize Inputs
	
addrRa = 0;
	
addrRb = 0;
	
addrW = 0;
	
datW = 0;
	
RegWrite = 0;
	
clk = 0;
	
rst = 0;

	
// Wait 100 ns for global reset to finish
	
#100;
  
for (addrRa = 0; addrRa < 8; addrRa = addrRa + 1) begin
	
#5 addrRb=addrRa+8;
	
$display("el valor de registro %d =  %d y %d = %d", addrRa,datOutRa,addrRb,datOutRb) ;
  
end
	
end
      
endmodule
```
## simulacion quartus
![simulacion quartus](https://github.com/ELINGAP-7545/lab06-grupo_8/blob/master/simulacion%20quartus.png)

## Descipción 
Se debe diseñar un banco de registro tal que:

* El banco de registro debe tener 16 registros de R/W.
* Permitir la lectura de 2 registros  simultáneamente 
* Permitir la escritura  de 1 registro, acorde a la señal de control regwrite
* Contar con señal de rst, la cual  ponga  todos los registros en un valor conocido.

![cn](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab07-BancosRgistro/doc/caja%20negra.png)

* Visualizar la información, en al menos dos display de 7 segmentos (información de cada registro leído).
* El ingreso de la información se debe hacer por medio de los interruptores.


**Opcional. Da mas puntos:**
* Parametrizar el tamaño de palabra de cada registro  y la cantidad de registro ( Por defecto =4 bits). Se recomienda leer el documento de este [link](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-884-complex-digital-systems-spring-2005/related-resources/parameter_models.pdf) .
* Pre cargar el banco de registro con información.  _Usar $readmenh_  (Investigar: "Initialize Memory in Verilog").

Entregables:

* Documentación
* Archivo `testbench` el cuál debe simular la escritura de 16 registros y 8 lecturas mas el rst, el resultado de la simulación debe visualizarse en diagrama de tiempo.
* Vídeo de la implementación.
* Código HDL de la solución.
.

 ![caja](https://github.com/Fabeltranm/SPARTAN6-ATMEGA-MAX5864/blob/master/lab/lab07-BancosRgistro/doc/banco%20registro.png)

