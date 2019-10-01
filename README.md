"# IA-2" 
#genetico
import numpy
import random

def Parejas(N):
    Aleatorio = random.sample(range(int(N/2),N),int(N/2))
    Pareja = {}
    for i in range(int(N/2)):
        Pareja[i] = Aleatorio[i]
        Pareja[Aleatorio[i]] = i
    return Pareja

def Inicializacion(N):
    global Individuos
    global Mejor
    Individuos = {}
    for i in range(N):
        Individuos[i] = numpy.random.choice(range(1,N+1),N,replace=True)
    print('---Inicializacion----')
    Mostrar(N)
	
def Seleccion(N):
    print('---Seleccion----')
    Pareja = Parejas(N)
    print('Parejas',Pareja)
    for k,v in Pareja.items():
        if Idoneidad(Individuos[k]) >= Idoneidad(Individuos[v]):
            Individuos[v] = Individuos[k]
    Mostrar(N)
            
def Idoneidad(Tablero):
    Atacadas = 0
    for i in range(len(Tablero)):
        for j in range(i + 1,len(Tablero)):
            if Tablero[i] == Tablero[j]:
                Atacadas += 1
            Dif = j - i
            if Tablero[i] == Tablero[j] - Dif or Tablero[i] == Tablero[j] + Dif:
                Atacadas += 1
    return (N*(N-1)/2-Atacadas)

def Cruce(N):
    print('-----Cruce ------')
    Pareja = Parejas(N)
    print('Parejas',Pareja)
    item = 0
    for k,v in Pareja.items():
        if item % 2 == 0:
            Punto = random.randint(1,N-2)
            print('punto',Punto)
            Hijo1 = []
            Hijo2 = []
            Padre1 = Individuos[k]
            Padre2 = Individuos[v]
            Hijo1.extend(Padre1[0:Punto])
            Hijo1.extend(Padre2[Punto:])
            Hijo2.extend(Padre2[0:Punto])
            Hijo2.extend(Padre1[Punto:])
            Individuos[k] = Hijo1
            Individuos[v] = Hijo2
        item = item+1
    Mostrar(N)

def Mostrar(N):
    for i in range(N):
        print(Individuos[i],'f(x)=',Idoneidad(Individuos[i]))

N = 8
Inicializacion(N)
Seleccion(N)
Cruce(N)



    
# logica
from kanren import Relation,fact, facts, var, run, conde
 
Progenitor = Relation()
Hombre = Relation()
Mujer = Relation()
 
#Hechos
fact(Hombre, "Homero")
fact(Hombre, "Bart")
fact(Hombre, "Abraham")
fact(Hombre, "Clancy")
fact(Hombre, "Kang")
fact(Mujer, "Jackeline")
fact(Mujer, "Mona")
fact(Mujer, "Marge")
fact(Mujer, "Paty")
fact(Mujer, "Selma")
fact(Mujer, "Maggie")
fact(Mujer, "Lisa")
facts(Progenitor, ("Homero", "Bart"),
                  ("Marge", "Bart"))
facts(Progenitor, ("Homero", "Lisa"),
                  ("Marge", "Lisa"))
facts(Progenitor, ("Kang", "Maggie"),
                  ("Marge", "Maggie"))
facts(Progenitor, ("Abraham", "Homero"),
                  ("Mona", "Homero"))
facts(Progenitor, ("Clancy", "Marge"),
                  ("Jackeline", "Marge"))
facts(Progenitor, ("Clancy", "Selma"),
                  ("Jackeline", "Selma"))
facts(Progenitor, ("Clancy", "Paty"),
                  ("Jackeline", "Paty"))
 
#Reglas

def Abuela(X, Y):
    Z = var()
    return conde((Progenitor(X, Z), Progenitor(Z, Y), Mujer(X)))

 
#Tarea:  Madre, Padre, Esposos, Herman@s
 
#####################################################
X = var()
Y = var()
Z = var()
#1- El abuela de Bart
res = run(0, X, Abuela(X, "Bart"))
print(res)

