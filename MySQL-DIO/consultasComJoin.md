# Claúsula JOIN
## Objetivos
* Entender o INNER JOIN e OUTTER JOIN
* Entender as claúsulas LEFT e RIGTH
* Unir várias consultas diferentes com UNION

### INNER JOIN

#### É comum nos depararmos com estruturas de banco de dados que possuem relacionamentos, seja ele um para um, um para muitos ou muitos para muitos. 
#### Nesses cenários, existem momentos nos quais é necessário realizar consultas mais específicas e trazer para o usuário os resultados desses relacionamentos. 
### Ex:
#### Imagine que temos um cenário onde existem as tabelas treinador, pokemon e pokemon_treinador onde a tabela pokemon_treinador representa uma ligação de muitos para muitos
#### Ou seja, um treinador pode ter vários pokemons e um pokemon pode pertencer a vários treinadores
#### Para que seja mostrado ao usuário quais pokemons ele detem, é necessário realizar uma consulta com a clausula JOIN
#### Temos que o ID do usuário seja 1
```sql
    SELECT treinador.nome, pokemon.nome
    FROM pokemon_treinador
    INNER JOIN pokemon ON pokemon_treinador.fk_pokemon = pokemon.id
    INNER JOIN treinador ON pokemon_treindaor.fk_treinador = treinador.id
    WHERE treinador.id = 1
    
```

