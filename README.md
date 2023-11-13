# Montecarlo

import random
import matplotlib.pyplot as plt
import numpy as np
import statistics as stats
import scipy
from scipy import stats #### calcular la probabilidad inversa es stats.norm.ppf(probabilidad, promedio, desviacion standar)   es igual a excel
import pandas as pd
import math
from math import log
from matplotlib import colors
from matplotlib import cm
from matplotlib.ticker import PercentFormatter
import re
from math import sqrt


def inicio():
    print()
    print()
    print()
    print()
    print ("Bienvenido a la matriz prototipo de montecarlo")
    
    filas = input(
        "introduce el numero de filas,estas corresponderan a las repeticiones de montecarlo) :")
    num_format = re.compile(r'^\-?[1-9][0-9]*$')
    it_is = re.match(num_format,filas)
    if it_is:
        
        filas = int(filas)
    else:
        print
        (" Ha ingresado una opcion incorrecta, por favor ingrese numeros enteros!")
        print()
        return inicio()
    
        



    columnas =(input("introduce el numero de columnas: (variables del experimento, las 3 primeras corresponden a los parametros de GAB, por ende, SI desea ingresar 4 sales, debera ingresar un total de 7 variables) : "))
    num_format = re.compile(r'^\-?[1-9][0-9]*$')
    it_is = re.match(num_format,columnas)
    if it_is:
        print("True")
        columnas = int(columnas)
    else:
        print(" Ha ingresado una opcion incorrecta, por favor ingrese numeros enteros!")
        print()
        return inicio()

    return filas, columnas










def entrada(filas,columnas):
    matriz = []
    casa =[]
    for i in range (3):#fila
        matriz.append([])
        for j in range (columnas):
        #print (matriz) muestra los valores que se van agregando a la matriz, lo dejo fuera para que se vea mas ordenada
            if i ==0:
                print("ingrese el nombre de la variable")
                valor = str(input("fila {}- columna {} :".format(i+1,j+1)))
                matriz[i].append (valor)
                casa.append(valor)
            if i==1:
                print("ingrese el promedio de la variable")
                valor = (input("fila {}- columna {} :".format(i+1,j+1)))
                num_format = re.compile(r"[-+]?\d*\.?\d+")
                it_is = re.match(num_format,valor)
                if it_is:
                    
                    valor = float(valor)
                    matriz[i].append (valor)
                else:
                    print(" Ha ingresado mal los datos, por favor ingrese numero entero o decimal correctamente!")
                    print()
                    print("Debera ingresar los datos de las sales desde el principio")
                    print()
                    return entrada(filas,columnas)
                
            if i==2:
                print("ingrese la desviacion de la variable")
                valor = (input("fila {}- columna {} :".format(i+1,j+1)))
                num_format = re.compile(r"[-+]?\d*\.?\d+")
                it_is = re.match(num_format,valor)
                if it_is:
                    
                    valor = float(valor)
                    matriz[i].append (valor)
                else:
                    print(" Ha ingresado mal los datos, por favor ingrese numero entero o decimal correctamente!")
                    
                    print()
                    print("Debera ingresar los datos de las sales desde el principio")
                    print()
                    return entrada(filas,columnas)
    print("su matriz con los datos ingresados es la siguiente:")

    print (matriz)#####dejar esto como comentario?? (1)

            
        
#####################################################################

    print() #se usa este print para separar el espacio entre la matrices

### aqui se le pone corchetes  a la matriz
    for fila in matriz:
        print ("[", end = " ")
        for elemento in fila:
            print ("{}".format(elemento), end=" ")
        print("]")
    return matriz 




def cortador(matriz,columnas):    ####################DE AQUI SE OBTIENEN LOS DATOS PARA LA REGRESION

##muestra la matriz con los nombres de las sales
    contador_de_sales= 3 ##################    COMO SON 3 VARIABLES INVOLUCRADAS EN LA ECUACION DE GAB, SE DEBE DESCARTAR LOS 3 PRIMEROS PARAMETROS CORRESPONDIENTES PROPIAS DE LAS ECUCACION.
    lista_de_nombres_de_variables =[]                                                   
    lista_de_sales = []
    for elemento in matriz[0]:
        lista_de_nombres_de_variables.append(elemento)

#print(lista_de_nombres_de_variables)
    print()
    while contador_de_sales <= len(lista_de_nombres_de_variables)-1:
        lista_de_sales.append(lista_de_nombres_de_variables[contador_de_sales])
        contador_de_sales = contador_de_sales +1
    #print(lista_de_sales,"lista con la sales involucradas")


    #contador de posiciones, le pone un numero a cada columna de la sal, para facilitar su selecion. Mediante la entrada de "columna".
    i = 1
    posicion = []
    while i <= (columnas-3):
        posicion.append(i)
        i = i+1
    print()
    return lista_de_sales, posicion 




def aleatorio(filas,columnas):
    matriz2 = np.random.rand(filas,columnas)
    print (matriz2)
    print()
    print("Esta es la matriz aleatoria")
    return matriz2


def probainversa(matriz,filas,columnas,matriz2):
    l =0 #### contador asociado las dimenciones de la matriz

    matriz3 = []
    for j  in range(columnas):
        l =l+1
        for i in range (filas):
            proba = matriz2[i][j]
            #print(proba)
        
            #proba1 =np.array(proba,dtype = np.float32) ####dejar esto como comentario?(2)
    
            nuevo = stats.norm.ppf(proba,matriz[1][l-1],matriz[2][l-1])
            matriz3.append(nuevo)
            #print ("probabilidad = ",proba,"promedio =",matriz[1][l-1],"desviacion = ", matriz[2][l-1],"su probabilidad inversa es:",nuevo)    #para que se demore menos  ###dejar esto como comentario?? (3)
    return matriz3    
    print()
    #print(matriz3,"esta es la matriz 3 y aqui se calculo la probabilidad inversa") DEJO ESTO COMO COMENTARIO PARA QUE SE VEA MÁS ORDENADA
    



def almacenaje(matriz3,filas,columnas):
    matriz4 = []
    ñ = 0
    #print("aqui se almacenan los datos de probabilidad inversa")
    for i in range(columnas):
        matriz4.append([])
        for j in range(filas):
            beta = matriz3[ñ] 
            ñ =ñ+1
            matriz4[i].append(beta)                           
    print()

    #for fila in matriz4:
      #  print("[", end = " ")
       # for elemento in fila:
        #    print("{:8.2f}".format(elemento), end= " ")
        #print("]")

    print()
    tras = np.transpose(matriz4)
    #print(tras)
    print()
    print("esta es la matriz original(la de arriba) pero traspuesta")
    print()
################################################### programa que captura las sales de la matriz
    print("Matriz correspondiente a todos los valores ingresados, en el siguiente paso de eliminaran las constantes correspondites a la ecuacion de Gab, dejando solo las sales")

    vacio = []
    contador = 3
   # alfa = len (tras[0])
    while contador < (len(tras[0])):
        columna = [fila[contador]for fila in tras]
        vacio.append(columna)
        contador = contador+1
    #print(vacio)
    ###################################
    #print(vacio[1][1])
    #print()
    #for i in vacio:
    #    for j in i:
    #        print (j)
################################
    #print()
    #for fila in vacio:
       # print("[", end = " ")
        #for elemento in fila:
         #   print("{:8.2f}".format(elemento), end= " ")
        #print("]")

    print()
    #print(vacio,"vacio")
    print()
    print("este es el tamaño de tras",len(tras))



    print()
    print()
#print(vacio,"vacio")
    print()
    return vacio,matriz4


def gab(vacio,matriz4):


    print()

    print()
    print("se obtiene la matriz ya lista para los operaciones, correspondiente a la matriz que solo contiene la Aw de las sales")
    print ()
    lo = np.transpose(vacio)
    print(lo,"matriz transpuesta")
    print()
    promediow= np.mean(lo, axis = 0)
    print()
    print("los valores maximos de  Aw para cada sal son:", promediow,"estos valores corresponden al eje X del grafico")
    print()
    print()
    print("se obtiene la matriz ya lista para los operaciones, ESTA MATRIZ CORRESPONDE A LA MATRIZ de AW") ###Esto no lo agregue
    print ()
    y = np.transpose(matriz4)
    #print(y)
    print()
    l =0
    print()

    matriz5 = []
    for i  in range(len(lo)):
        l =l+1
        for j in range (len(lo[0])):
            sal = lo[i][j] ############ la transpuesta va si o si
            mono = (matriz4[0][l-1]*matriz4[1][l-1]*matriz4[2][l-1]*sal)/((1-matriz4[2][l-1]*sal)*((1-matriz4[2][l-1]*sal+matriz4[1][l-1]*matriz4[2][l-1]*sal)))
            matriz5.append(mono)
            #print ("sal = ",sal,"mono =",matriz4[0][l-1],"ca = ",matriz4[1][l-1],"k: ",matriz4[2][l-1],"humedad",mono  )# se omite para que se demore menos (4)
    print("Aqui esta la matriz 5 la cual son las iteraciones con la ecuacion de Gab")
    
            
    print()
    #print(matriz5)
    #print("la matriz de arriba corresponde a los valores de la Monocapa")
    print()
    return matriz5,lo,promediow
    



def conversor(matriz5,lo,filas,columnas):
    alfa = (len(lo[0]))

    matriz6 = []########pasar de lista a matriz
    ñu = 0
    print("aqui se almacenan los datos de los valores de la monocapa")
    for i in range(len(lo)):
        matriz6.append([])
        for j in range(alfa):
            beta = matriz5[ñu]
            ñu =ñu+1
            matriz6[i].append(beta)  

    matriz = np.array(matriz6).reshape(filas,columnas-3)
    print(matriz)                         
    print()
    #print(matriz6)
    print()

    print("matrices con humedades calculadas  g de agua/ 100 g de solido seco")
    print()
    #for fila in matriz6:
        #print("[", end = " ")
        #for elemento in fila:
           # print("{:8.8f}".format(elemento), end= " ")
        #print("]")
    print()##pasa de lista a matriz
    #print("El señor guru")
    return matriz6


def estadistica(matriz6,promediow):
    maximos= np.amax(matriz6, axis = 0)
    print()
    print("los valores maximos de monocapa para cada sal son:", maximos)
    print()
    minimos = np.amin(matriz6, axis = 0)
    print("los valores minimos de monocapa cada sal son:",minimos)
    print()
    promedio = np.mean(matriz6,axis = 0)#############################
    #print("el promedio de las sales es: ", promedio,"g de agua/ 100 g de solido seco")
    print()
    promedio2 = (promedio*0.01)
    #print("el promedio de las sales en g de agua/ g de solido seco es : ",promedio2,"estos valores corresponden al eje X de la regresion")###################
    print()
    desviacion = np.std(matriz6,axis = 0)
    #print("la desviacion de cada sal es: ", desviacion)
    print()
    #print("se tienen los siguientes valores")
    print()
    #print("el promedio de las sales en g de agua100 g de solido seco es : ",promedio,"estos valores corresponden al eje y de la regresion")
    print()
    #print("los valores promedio de cada sal son:", promediow,"estos valores corresponden al eje x del grafico")
    print()
    return matriz6,promedio,promedio2







def regresion(lista_de_sales,posicion,promediow,promedio,promedio2):
    matriz_de_sales = np.array(lista_de_sales).reshape(len(lista_de_sales),1)###################reshape: redimensiona la lista  a una matriz
    matriz_posicion = np.array(posicion).reshape(len(posicion),1)

    variablex = np.array(promediow)
    variabley = np.array(promedio/100)  ### se divide en 100 para que de en g/100 g solido seco
    tamaño = len(variablex)
    sumax= sum(variablex)
    sumay = sum(variabley)
    sumacuadradox =sum(variablex*variablex)
    sumacuadradoy = sum(variabley*variabley)
    sumaxy = sum(variablex*variabley)
    promediox = sumax/tamaño
    promedioy = sumay/tamaño
    sigmax = np.sqrt(sumacuadradox/tamaño-promediox**2)
    sigmay =np.sqrt(sumacuadradoy/tamaño-promedioy**2)
    sigmaxy = sumaxy/tamaño-promediox*promedioy
    r2 = (sigmaxy/(sigmax*sigmay))**2
    print()
   
    matrizx = np.array(variablex).reshape(tamaño,1)
    #print(matrizx,"matriz con las variables y  g de agua/ 100 g de solido seco")
    print()
    matrizy = np.array(variabley).reshape(tamaño,1)
    #print(matrizy,"matriz con la variable   promedio aw, componente X")
    print()
    #print( "g de agua/           Aw")
    #print("g de solido seco")
    print()
    #print(matriz_de_sales,"esta es la matriz con las sales")

    print()
    penultimamatriz = np.concatenate((matriz_posicion,matriz_de_sales,matrizx,matrizy),1) ## #### el numero 1 indica que el orden sera por colummnas. De lo contrario por defecto se ordenara po filas.
    print("")
    print ("Esta matriz corresponde a las sales involucradas-Los valores de AW  de cada sal - Y los valores promedio de g de agua / g de solidos secos (m) (De izquierda a Derecha)")
    print(penultimamatriz)
    print()
    
 
    








#############################################################################################   datos estadisticos de toda la matriz

    pendiente = (sumaxy*tamaño-sumax*sumay)/(tamaño*sumacuadradox-sumax*sumax)            
    intercepto = promedioy -pendiente*promediox                                                   
    #print ("la pendiente es :",pendiente, "y su intercepto es:",intercepto)
    #print()
    print("la ecuacion de la recta es:","y = ",pendiente,"X +",intercepto)                         
    #print()
    plt.plot(variablex, variabley,label = 'regresion lineal')
    plt.plot(variablex,pendiente*variablex+intercepto, label= "ajuste")
    plt.xlabel(" AW")                                                    
    plt.ylabel("g de agua/ g de solido seco")
    plt.title(" / Aw (actividad de agua) /g de agua/ g de solido seco")                              
    plt.grid()####muestra las regillas
    plt.legend()
    plt.show()
    #######################################################################################################################################################################################
    #print("")
    #print(" a continuacion se muestra la matriz g de agua g de solido seco vs AW")
    #print("")
    variablexx = np.array(promedio2)
    variableyy = np.array(promediow)
    tamaño = len(variablexx)
    sumaxx= sum(variablexx)
    sumayy = sum(variableyy)
    sumacuadradoxx =sum(variablexx*variablexx)
    sumacuadradoyy = sum(variableyy*variableyy)
    sumaxy2 = sum(variablexx*variableyy)
    promedioxx = sumaxx/tamaño
    promedioyy = sumayy/tamaño
    sigmaxx = np.sqrt(sumacuadradoxx/tamaño-promedioxx**2)
    sigmayy =np.sqrt(sumacuadradoyy/tamaño-promedioyy**2)
    sigmaxy2 = sumaxy2/tamaño-promedioxx*promedioyy
    r22 = (sigmaxy2/(sigmaxx*sigmayy))**2
    print("el ajuste de la ecuacion es de: ",r22)
    #print()
    matrizxx = np.array(variablexx).reshape(tamaño,1)
    #print(matrizxx,"matriz con las variables X  g de agua/  g de solido seco")
    print()
    matrizyy = np.array(variableyy).reshape(tamaño,1)
    #print(matrizyy,"matriz con la variable   promedio aw, componente Y")
    print()
    #print( "g de agua/           Aw")
    #print("g de solido seco")
    #matrizfinal = np.concatenate((matriz_de_sales,matrizyy,matrizxx),1)
    print("")
    #print ("Esta matriz corresponde valores  de Aw vs promedio de g de agua / g de solidos secos asociados a cada sal en particular")
    print()
    #print(matrizfinal)
  

    
    print("A continuacion selecione la actvidad del agua(AW) a utilizar,esta  se encuentra en el eje X del grafico, asi como los datos del eje Y (g de agua/ g de solido seco) ")
    print("Guiese por la grafica obtenida anteriormente")
    print()
    #print(type(y))###esta es la matriz m
    print()
   # print(type(matriz6))# esta es la matriz con actividades de agua (aw)   #################Esto de aca es solo para ver a que "tipo" pertenece cada matriz, se debe cambiar.
    print()
    #print(type(matriz4))
    print()
    return penultimamatriz
    #print("otro tipo de regresion")
    #variablex = variablex.reshape((-1,1))
    #variabley = variabley.reshape((-1,1))
   # model= LinearRegression() 
   # model.fit(variablex,variabley)
   # coefr = model.score(variablex,variabley)
   # print("el coeficiente de determinacion es:",coefr)
    #print()
    #print("el valor de la pendiente es;",model.coef_)
   # print()
   # print("el valor del intercepto  es;",model.intercept_)
   # print()
    
    
    
    
    
    




def variablex():
    h = []
    a = input("ingrese el valor de la coordenada  X: ")
    num_format = re.compile(r'[0-9.]+$') #(r'^\-?[0-9][0-9]*$')
    it_is = re.match(num_format,a)
    if it_is:
        print("True")
        a = float(a)
        h.append(a)
        return h
    else:
        print(" Ha ingresado mal los datos, por favor ingrese numero entero o decimal correctamente!")
        print()

        return variablex()
    


def variabley():
    l = []
    b = input("ingrese el valor de la coordenada Y: ")
    num_format = re.compile(r'[0-9.]+$')
    it_is = re.match(num_format,b)
    if it_is:
        print("True")
        b = float(b)
        l.append(b)
        return l
    else:
        print(" Ha ingresado mal los datos, por favor ingrese numero entero o decimal correctamente!")
        print()

        return variabley()

def variables(x,y):
    print("¿desea agregar otro par ordenado?: ")
    print("1)si")
    print("2)no")
    c = int(input("ingrese su opcion: "))
    if c ==1:
        d = input("agregue la coordenada X :")
        num_format = re.compile((r'[0-9.]+$'))
        it_is = re.match(num_format,d)
        if it_is:
            print("True")
            d = float(d)
            
        else:
            print(" Ha ingresado mal los datos, por favor ingrese numero entero o decimal correctamente!")
            print()

            return variables(x,y)
        e = input("agregue la coordenada Y :")
        num_format = re.compile(r'[0-9.]+$')
        it_is = re.match(num_format,e)
        if it_is:
            print("True")
            e = float(e)

        else:
            print(" Ha ingresado mal los datos, por favor ingrese numero entero o decimal correctamente!")
            print()

            return variables(x,y)
        
        
        
        
        
        
        x.append(d)
        y.append(e)
        variables(x,y)
    elif c == 2 :
        print("Su coordenadaen X es",x,"su coordenada en el eje Y es",y)
        return x         
        
    else:
        print("ingrese una opcion valida: ")

        

    
    
def regresion2(x,y,penultimamatriz):
    ejex = np.array(x)
    ejey = np.array(y)
    tamaño = len(ejex)
    sumatoriaejex = sum(ejex)
    sumatoriaejey=sum(ejey)
    promediox= sumatoriaejex/tamaño
    promedioy=sumatoriaejey/tamaño
    sumax2= sum(ejex*ejex)
    sumay2= sum(ejey*ejey)
    sumaxy= sum(ejex*ejey)
    pendiente = (sumatoriaejex*sumatoriaejey-tamaño*sumaxy)/(sumatoriaejex**2-tamaño*sumax2)
    intercepto = promedioy-pendiente*promediox
    sigmax = np.sqrt(sumax2/tamaño- promediox**2 )
    sigmay = np.sqrt(sumay2/tamaño- promedioy**2 )
    sigmaxy = sumaxy/tamaño - promediox*promedioy
    r2 = (sigmaxy/(sigmax*sigmay))**2

    print("la ecuacion de la recta es: y =",pendiente,"X+", intercepto,'y su ajuste es',r2)
    print()
    plt.plot(ejex,ejey,'o',label = 'regresion lineal')
    plt.plot(ejex,pendiente*ejex+intercepto,label = 'ajuste')
    plt.xlabel('x')
    plt.ylabel('y')
    plt.title(' Aw VS  (g agua/g solido vs)')
    plt.grid()
    plt.legend(loc = 4)
    plt.show()
    print()
    print("a continuacion, ingrese los datos termodinamicos")
    awinicial = float(input("ingrese la Aw inicial:"))
    mi = awinicial*pendiente+intercepto
    print()
    perdida = float(input("ingrese la perdida de humedad en %:"))
    mc = (1-perdida)*mi
    print()
    equilibrio = float(input("ingrese la Aw de equilibrio:"))
    awequ = equilibrio*pendiente+intercepto
    logaritmo = log((awequ-mi)/(awequ-mc))
    print(logaritmo)
    print()
    print("a continuacion se calculara el valor de permencia, este es el wvptr/ gradiente de presion de vapor ")
    print("para ello deberas agregar el valor del WVTR y luego la presion de vapor")
    wvtr = float(input("ingrese el valor de wvtr en g/m2 *dia:"))
    print()
    gradiente = float(input("ingree el valor de la gradiente de presion de  vapor en mmhg:"))
    permencia = wvtr/gradiente
    print()
    area = float(input("ingrese el area del envase en m2:"))
    print()
    peso = float(input("ingrese el valor del peso inicial em gramos"))
    contenido_humedad_base_seca = mi
    contenido_inicial_de_humedad_base_humeda = contenido_humedad_base_seca/(1+contenido_humedad_base_seca)
  
    contenido_inicial_solidos_base_humeda = 1-contenido_inicial_de_humedad_base_humeda
    ws = contenido_inicial_solidos_base_humeda*peso
    
    presion = float(input("ingrese la presion de vapor de la isoterma selecionada en mmhg:"))
    fraccion = (area/ws)*(presion/pendiente)*permencia
    print()
    print(fraccion)
    dias = logaritmo/fraccion
    meses = dias/30
    print("la duracion del producto es",dias,"dias, lo que es lo mismo que decir",meses, 'meses de duracion')
    print()
    print("Este valor corresponde a un valor numerico determinista, por lo tanto, hay que aplicar test de confianza para estimar un valor más real")
    print("A continuacion, se calculara mediante MONTECARLO iteraciones para la vida util del alimento mediante las sales de interes asociadas a una actividad de agua Aw")
    print("ingrese la posicion desde la cual quiere empezar a trabajar, posicion desde la 1- 10")
    print()
    print(penultimamatriz)
    print()
    return penultimamatriz,awinicial,perdida, equilibrio,wvtr,gradiente,permencia,area,peso,presion
    
    
def selector(matriz6,lo):
    
###AQUI SE VAN a extraer LOS G DE AGUA/SOLIDO SECO DE INTERES
#Matriz6 = matriz g de agua/ 100 g solidos seco.
    selector_solidos = []
    contador = int(input("ingrese su opcion desde la cual quiere trabajar:"))  ##############Este dato debera ser entregado por el usuario
    contador2=contador-1
    contador3 = contador2
    contador4= contador-1
    #alfa = len (matriz6[0])
    while contador2 < (len(matriz6[0])):
        columna = [fila[contador2]for fila in matriz6]
        selector_solidos.append(columna)
        contador2 = contador2+1

    print()



    lo1 = (np.transpose(selector_solidos)/100)    #### la matriz debe estar simplificado por 100  Esta corresponde  al EJE Y
#print(lo1,"esta es la matriz transpuesta")

    print()
    #for fila in lo1:
        #print("[", end = " ")
        #for elemento in fila:
            #print("{:8.8f}".format(elemento), end= " ")
            #print("]")
            #print()


    #print(contador3)
    #print()




    selector_aw = [] 
   
    while contador3 < (len(lo[0])): ### no puede ser menor o igual
        columna = [fila[contador3]for fila in lo]
        selector_aw.append(columna)
        contador3 = contador3+1
#print(selector_solidos,"esta es la matriz "y" pero con los aw de agua selecionados con n =3 por lo tanto, deberia entregar 7 columnas")
    print()



    lo2 = np.transpose(selector_aw)
#print(lo1,"esta es la matriz transpuesta")

    #print()
    #for fila in lo2:
        #print("[", end = " ")
        #for elemento in fila:
            #print("{:8.8f}".format(elemento), end= " ")
            #print("]")

    print()
    return contador,contador4,lo1,lo2
    


def regresiones_multiples(contador,contador4,lo1,lo2,filas,
                          columnas,awinicial,perdida, equilibrio,
                          wvtr,gradiente,permencia,area,peso,presion):
    #tamaño = contador-1
    resultado_final_meses =[]
    resultado_final_dias = []
    sonmc= []
    sonme=[]
    sonmi=[]
    peso_inicial = peso
    valormi=[]
    valormc =[]
    valorme =[]
    valorx =[]
    valory=[]
    
    
    #print(contador)###bien
    #print(contador4)##bien
    #print(lo1)#bien
    #print()
    #print(lo2)#bien
    #print(filas)#bien
    #print(columnas)#bien
    #print(awinicial)#
    #print(perdida)#
    #print(equilibrio)#
    #print(wvtr)#
    #print(gradiente)##
    #print(permencia)
    #print(area)##
    #print(ws)#
    #print(presion)#
    negativo = []
    
    #humedadcritica = []
    #humedadequilibrio =[]
    print("Esta es la humedad inicial",awinicial)
    print("Esta es la humedad de equilibrio",equilibrio)
    #pendientes = []
    #interceptos = []
    #dias_criticos = []
    dias_indeterminados = []
   
    #alfa =[]
    q = []
    f = []
    listax=[]
    listay = []
    for i in range(filas):  ## numero de filas
        for j in range(columnas-3-contador4):############### numero de columnas ######## tener ojo con los contadores a utilizar 
            q.append( lo2[i][j])                  
            f.append(lo1[i][j])
        
        x = np.array(q)
        y =np.array(f)
        tamaño =len(x)
        sumax = sum(x)
        sumay = sum(y)
        sumax2 = sum(x*x)
        sumay2 = sum(y*y)
        sumaxy = sum(x*y)
        promediox = sumax/tamaño  #### el tamaño esta difinido por el largo de la fila (en ese caso, por la logitud del vector
        promedioy =sumay/tamaño

        pendiente = ((sumax*sumay) -tamaño*sumaxy)/(sumax**2-tamaño*sumax2)              #
        intercepto = float(promedioy -pendiente*promediox)
        
        #print(pendiente,"este es la pendiente")
        #print(intercepto,"este es el intercepto")
        mi= pendiente*awinicial+intercepto
        mc = (1-0.1)*mi
        me =pendiente*equilibrio+intercepto
        mi2= mi/(1+mi)
        ms = 1-mi2
        ws= ms*peso_inicial
        argumento =(me-mi)/(me-mc)
        if argumento <0:
            negativo.append(argumento)
            valormi.append(mi)
            valorme.append(me)
            valormc.append(mc)
            valorx.append(x)
            valory.append(y)
            dias_indeterminados.append(argumento)
            
            argumento = 0.00000000001
        else:
            True
            
        logaritmo = log(argumento)
        fraccion = permencia*(area/ws)*(presion/pendiente)
        dias = logaritmo/fraccion
        resultado_final_dias.append(dias)
        meses = dias/30
        resultado_final_meses.append(meses)
        
        
        listax.append(pendiente)
        listay.append(intercepto)
        resultado_final_meses############ matriz la cual contiene la vida util del alimento expresado en meses
        q = []
        f =[]
    #print(listax,"lista con pendientes") ##Esta lista contiene las pendientes de la matriz 
    print()
    #print(listay,"lista con interceptos")## coleccion de interceptos de la matriz
    print()
    #print(len(listax))
    print()
    #print(len(listay))
    print()
    print("La siguiente matriz contiene todos los posibles resultados de vida util, a diferenicia del calculado anteriormente, se hace otra iteracion por MC")
    print("para encontrar valores más reales del producto, mediante el uso de rango de confianza estadistica")
    print()
    print()
    #print(resultado_final_dias,"resultado final en dias")
    print()
    maximo_dias= np.amax(resultado_final_dias, axis = 0)
    minimo_dias= np.min(resultado_final_dias, axis = 0)
    print("el maximo de dias calculados es",maximo_dias,"y el minimo de dias es",minimo_dias)
    #print(resultado_final_meses,"resultado final en meses")
    print()
    maximo_meses= np.amax(resultado_final_meses, axis = 0)
    minimo_meses= np.min(resultado_final_meses, axis = 0)
    print("el maximo de meses calculados es",maximo_meses,"y el minimo de meses es",minimo_meses)
    print()
   

    
    lista_final = []
    for elemento in resultado_final_dias:
        if elemento >0 :
            lista_final.append(elemento)
    
    tamaño_lista_final = len(lista_final)        
    tamaño_dias_final = len(resultado_final_dias)
    tamaño_dias_indeterminados = len(dias_indeterminados)
    tamaño_dias_negativos = tamaño_dias_final-tamaño_dias_indeterminados-tamaño_lista_final
    tamaño = len(lista_final)
    suma = sum(lista_final)
    promedio = suma/tamaño
    print("El promedio de dias de duracion es de:",promedio)
    print("Dias validos",tamaño_lista_final)
    print("Estos son los dias indeterminados:",tamaño_dias_indeterminados)
    print("Estos son los dias negativos:",tamaño_dias_negativos)
    dias_sin_usos = tamaño_dias_indeterminados+tamaño_dias_negativos
    porcentaje = ((dias_sin_usos)/len(resultado_final_dias))*100
    print("Este es el porcentaje de dias sin usos",porcentaje)
    
    
    return maximo_dias,lista_final
        
       
   
        
def ordenar (maximo_dias,resultado_final_dias,filas):
    filas = str(filas)
    eleccion = int(input("elija los intervalos de dias por los que quiere contar: "))
    acumulador = []
    lista_dias = []
    lista=[]
    cantidad = 0
    inicio = 0
    final = eleccion
    while final <= maximo_dias+10:
        for elemento in resultado_final_dias:
            if elemento>=inicio and elemento < final:
                cantidad = cantidad+1
        lista.append(cantidad)
        acumulador.append(inicio)
        acumulador.append(final)
        lista_dias.append(acumulador)
        
        #print("la cantidad de dias entre [",inicio,",",final,'[ es de',cantidad)
        #print(lista)
        final = final+eleccion
        inicio = inicio+eleccion
        cantidad =0
        acumulador = []
    print()
    #print('esta es la lista con los dias',lista_dias)
    #lista_dias
    #lista
    print()
    df_dias = pd.DataFrame(list(zip(lista_dias,lista)), columns = ['INTERVALO_DE_DIAS','FRECUENCIA'])
    #print(df_dias)
    print()
    df_dias = df_dias.dropna()#elimina los valores nulos de data frame,
    #los valores nulos son distintos a los 0, ya que 0 es un numero entero.
    #print(df_dias)
    print()
    #print(df_dias.isnull().sum())  ##
    print()
    df_dias = (df_dias.loc[(df_dias != 0).all(axis=1), :])
    tamaño = len(resultado_final_dias)###########################################################################
    frecuencia = df_dias["FRECUENCIA"]
    #print(frecuencia,"esta es la frecuencia")
    frecuencia_relativa = frecuencia*100/tamaño
    #print(frecuencia_relativa)
    print()
    df_dias["frecuencia_relativa(%)"] = df_dias["FRECUENCIA"]/tamaño
    acumulador_de_frecuencia = 0
    frecuencia_acumulada = []
    frecuencia_relativa_valores = df_dias["frecuencia_relativa(%)"].values #### se tienen que pasar a valores los datos del data frame para poder iterarlos
    #print(df_dias)
    
    
    for elemento in frecuencia_relativa_valores:
         acumulador_de_frecuencia= (acumulador_de_frecuencia+(elemento))
         frecuencia_acumulada.append(acumulador_de_frecuencia)
    #print(frecuencia_acumulada)

    df_dias["frecuencia_relativa_acumulada"] = frecuencia_acumulada ### SE AÑADE UNA NUEVA COLUMNA AL FINAL DEL DATA FRAME
    print()
    
    
    
    #print(df_dias)
    #df_dias.to_csv('datos_dias2.csv',index =False) ### lo exporta sin los indices en la primera columna
    df_dias.to_csv('datos_dias2.csv')
    print()
    coordenadas = np.arange(len(df_dias["FRECUENCIA"]))## SE CREA UN RANGO CON EL TAMAÑO DE LA LISTA DE VALORES QUE SE DISTRIBUYE UNIFORMEMENTE

    ancho = 0.3
    
    grafico = plt.figure()
    ejex = grafico.add_subplot(1,1,1)### crea un objeto en dos dimensiones
    ejex.set_title('DIAGRAMA DE PARETO PARA'+filas+" SIMULACIONES")
    #ejex.bar(df_dias.index,df_dias["FRECUENCIA"],color = "C0")
    ejex.bar(coordenadas,df_dias["FRECUENCIA"],ancho)## index nos indica que tomara los datos de los indices asignados, en este caso no me sirve el indice
    ejex.set_ylabel("FRECUENCIA")
    
    

    ##CAMBIE el indice por los intervalos de dias, el cual es la columna 2

    

    #ejex.tick_params(axis = "y",colors = "C0")
    #ejex2.tick_params(axis = "y", colors = "C1")
    ejex.set_xticks(coordenadas)
    ejex.set_xticklabels(df_dias["INTERVALO_DE_DIAS"],rotation = 75 ) ###AQUI SE SELECIONA EL NOMBRE DEL EJE X ASI COMO LA INCLINACION DE LAS ETIQUETAS
    ejex2 = ejex.twinx()
    ejex2.plot(coordenadas,df_dias["frecuencia_relativa_acumulada"]*100,color = "C1",marker="D",ms=5)## se repite el nombre del eje X ya que es un eje comun a ambos.DIBUJA 
    ejex2.yaxis.set_major_formatter(PercentFormatter())
    ejex.tick_params(axis = "y",colors = "C0")
    ejex2.tick_params(axis = "y", colors = "C1")
    ejex.set_ylabel("FRECUENCIA")
    ejex2.set_ylabel("FRECUENCIA ACUMULADA")
    ejex.set_xlabel("INTERVALOS DE DIAS CORRRESPONDIENTE A LA  VIDA UTIL")
    


    plt.show()
    print()
    #print("Calculo de intevalo de confianza")
    print("guru")
    #print(resultado_final_dias)
    print()
    
    arreglo = np.array(resultado_final_dias)
    #print(stats.norm.interval(confidence  = 0.05,####Intervalo de confianza del 95%
    #loc = np.mean(arreglo),scale == arreglo.std(ddof=1)/sqrt(arreglo.size)
    
    
    promedio = np.mean(arreglo)
    print("Este es el promedio", promedio)
    
    
    
    
    print()
    print("Gracias por utilizar el programa, hasta luego!!!!!!!!")








inicio2 = inicio()

nuevo_inicio = list(inicio2)

continuacion = entrada(nuevo_inicio[0],nuevo_inicio[1])

matriz_1 = cortador(continuacion,nuevo_inicio[1])

matriz_aleatoria = aleatorio(nuevo_inicio[0],nuevo_inicio[1])

matriz_2 = probainversa(continuacion,nuevo_inicio[0],nuevo_inicio[1],matriz_aleatoria)


matriz_3 = almacenaje(matriz_2,nuevo_inicio[0],nuevo_inicio[1])

matriz_4 = gab(matriz_3[0],matriz_3[1])

matriz_5 = conversor(matriz_4[0],matriz_4[1],nuevo_inicio[0],nuevo_inicio[1])

matriz_6 = estadistica(matriz_5,matriz_4[2])

matriz_7 = regresion(matriz_1[0],matriz_1[1],matriz_4[2],matriz_6[1],matriz_6[2])

x = variablex()

y = variabley()

analisis = variables(x,y)

matriz_8 = regresion2(x,y,matriz_7)





matriz_9 = selector(matriz_5,matriz_4[1]) #########revisar esta

matriz_10 = regresiones_multiples(matriz_9[0],matriz_9[1],
                       matriz_9[2], matriz_9[3],
                       nuevo_inicio[0],nuevo_inicio[1],matriz_8[1],matriz_8[2],
                       matriz_8[3],matriz_8[4],matriz_8[5],matriz_8[6],
                       matriz_8[7], matriz_8[8],matriz_8[9])




matriz_11 = ordenar(matriz_10[0],matriz_10[1],nuevo_inicio[0])
