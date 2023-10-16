Fibonacci com recursão:

import sys

def fibonacci(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)

if len(sys.argv) != 2:
    print("Uso: python my_script.py <n>")
else:
    try:
        n = int(sys.argv[1])
        if n < 0:
            print("Por favor, insira um número inteiro positivo.")
        else:
            result = fibonacci(n)
            print(f"O {n}-ésimo valor da sequência de Fibonacci é: {result}")
    except ValueError:
        print("Por favor, insira um número inteiro positivo.")


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Consumindo a API Star Wars e salvando em arquivos JSON:

import requests
import json

# Função para obter os personagens que aparecem em 4 ou mais filmes
def get_popular_characters():
    url = "https://swapi.dev/api/people/"
    popular_characters = []
    
    while url:
        response = requests.get(url)
        data = response.json()
        for character in data["results"]:
            if len(character["films"]) >= 4:
                popular_characters.append(character)
        url = data["next"]
    
    with open("popular_characters.json", "w") as outfile:
        json.dump(popular_characters, outfile, indent=4)

# Função para obter os planetas com 5 ou mais moradores
def get_populous_planets():
    url = "https://swapi.dev/api/planets/"
    populous_planets = []
    
    while url:
        response = requests.get(url)
        data = response.json()
        for planet in data["results"]:
            if len(planet["residents"]) >= 5:
                populous_planets.append(planet)
        url = data["next"]
    
    with open("populous_planets.json", "w") as outfile:
        json.dump(populous_planets, outfile, indent=4)

get_popular_characters()
get_populous_planets()



Certifique-se de que você tem a biblioteca requests instalada para executar o segundo script. Você pode instalá-la com pip install requests. Ambos os scripts foram divididos em funções para facilitar a leitura e manutenção do código. Eles também criam arquivos JSON na raiz do projeto com os dados encontrados.
