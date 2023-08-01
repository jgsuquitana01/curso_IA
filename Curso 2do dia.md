SEGUNDO DIA:



Python Básico:

- Python es un lenguaje de programación interpretado, de alto nivel, de propósito general y de código abierto. Fue creado a finales de la década de 1980 por Guido van Rossum y se ha vuelto extremadamente popular debido a su enfoque en la legibilidad del código y su facilidad de aprendizaje.

Algunas de las características distintivas de Python son: 
1. **Legibilidad del código:**  Python enfatiza la claridad y la simplicidad del código, lo que facilita su lectura y comprensión, lo que a su vez promueve la colaboración entre desarrolladores. 
2. **Sintaxis clara y sencilla:**  La sintaxis de Python es intuitiva y cercana al lenguaje natural, lo que facilita el desarrollo rápido y la escritura de programas concisos. 
3. **Versátil y de propósito general:**  Python se puede utilizar para una amplia variedad de tareas, desde desarrollo web y científico hasta automatización, inteligencia artificial y análisis de datos. 
4. **Orientado a objetos:**  Python admite la programación orientada a objetos, lo que permite organizar el código en clases y objetos para reutilización y modularidad. 
5. **Interpretado e interactivo:**  Python es un lenguaje interpretado, lo que significa que el código se ejecuta línea por línea en lugar de ser compilado previamente. Esto permite una mayor flexibilidad y facilidad en el desarrollo. Además, Python también ofrece un modo interactivo que permite probar fragmentos de código de manera inmediata. 
6. **Amplia comunidad y bibliotecas:**  Python cuenta con una comunidad activa y numerosas bibliotecas que facilitan el desarrollo de diversas aplicaciones y resuelven una amplia gama de problemas, lo que lo convierte en una opción popular para muchos proyectos.



EJERCICIOS EN CLASES:



```Python
print ("hola mundo")

def multiply_by_two(x):
    """This function multiplies
        an input number by 2"""
    return x * 2

help(multiply_by_two)
#muestra la informacion de ayuda de la funcion

def apply_func_twice(func, arg):
    """This function applies 'func' twice to 'arg'"""
    return func(func(arg))

def add_five(x):
    return x + 5 

print(apply_func_twice(add_five, 10))

def greet(person):
    return f"Hello, {person}!"

def repeat(func, times, arg):
    for _ in range(times):
        print(func(arg))

repeat(greet, 3 ,"OpenIA")

import random
signs = ['Libra', 'Aries', 'Cancer', 'Tauro']
forecast  = ['Encontraras el amor', 'Perderas a una persona importante', 'Ganaras un juego', 'Te van a robar hoy']

def horoscope():
  signs_ingresados = input("Ingresa tu signo: ")
horoscope()
print(random.choice(forecast))


num = 5
factorial = 1
while num > 0:
  factorial *= num
  num -= 1

print(factorial)


s = " Hello, Mars!"
print(s.lower())
print(s.upper())
print(s.split(", "))


planets =[
    'Mercury', 'Venus', 'Earth', 'Mars', 'Jupiter', 'Saturn', 'Uranus', 'Neptune'
]

planet_initials = {
    planet: planet[0]
    for planet in planets
}
print(planet_initials)

```


TAREA FINAL DEL SEGUNDO DIA:

Realizar un sorteo donde se muestre el numero ganador de la lotería y que el animal escogido del sorteo puede conseguir otro boleto de forma gratuita  

```Python
import random

aniamls  = ['Perro', 'Gato', 'Delfin', 'Tiburon', 'Jirafa']

numbers = [1, 2, 3, 4]

  
def lottery():

    lottery = input("Ingresar su numero de loteria: ")

lottery()

tamano_lista = 4

rango_minimo = 1

rango_maximo = 4

  
lista_aleatoria = [random.randint(rango_minimo, rango_maximo) for _ in range(tamano_lista)]

  
if lottery == lista_aleatoria:

    print("Ganaste la loteria")

else:

  print("Lo sentimos esta es la lista de numeros de loteria que ganaron: ", lista_aleatoria)

  
def animal():

    animal = input("Ingrese su Animal: ")

animal()

  
lista_animal =

  
if animal == lista_animal:

     print("Ganaste otro boleto")

else:

     print("Lo sentimos el animal ganador es: ", random.choice(aniamls))

```




