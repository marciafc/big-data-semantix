# Hive - Seleção de Tabelas

```

docker-compose up -d

docker exec -it hive-server bash
beeline -u jdbc:hive2://localhost:10000

show databases;


# 1. Selecionar os 10 primeiros registros da tabela nascimento pelo ano de 2016

use marcia;

show tables;
+-------------+
|  tab_name   |
+-------------+
| nascimento  |
| pop         |
+-------------+



select * from nascimento where ano=2016 limit 10;
+------------------+------------------+------------------------+-----------------+
| nascimento.nome  | nascimento.sexo  | nascimento.frequencia  | nascimento.ano  |
+------------------+------------------+------------------------+-----------------+
| Emma             | F                | 19471                  | 2016            |
| Olivia           | F                | 19327                  | 2016            |
| Ava              | F                | 16283                  | 2016            |
| Sophia           | F                | 16112                  | 2016            |
| Isabella         | F                | 14772                  | 2016            |
| Mia              | F                | 14415                  | 2016            |
| Charlotte        | F                | 13080                  | 2016            |
| Abigail          | F                | 11747                  | 2016            |
| Emily            | F                | 10957                  | 2016            |
| Harper           | F                | 10773                  | 2016            |
+------------------+------------------+------------------------+-----------------+



# 2. Contar a quantidade de nomes de crianças nascidas em 2017

select count(nome) as qtd from nascimento where ano=2017;

+--------+
|  qtd   |
+--------+
| 32469  |
+--------+


# 3. Contar a quantidade de crianças nascidas em 2017

# sum da frequência
select sum(frequencia) as qtd from nascimento where ano=2017;

+----------+
|   qtd    |
+----------+
| 3546301  |
+----------+




# 4. Contar a quantidade de crianças nascidas por sexo no ano de 2015

select sexo, sum(frequencia) as qtd from nascimento where ano=2015 group by sexo;

+-------+----------+
| sexo  |   qtd    |
+-------+----------+
| F     | 1778883  |
| M     | 1909804  |
+-------+----------+


# 5. Mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo

select ano, sexo, sum(frequencia) as qtd from nascimento group by ano, sexo order by ano desc;

+-------+-------+----------+
|  ano  | sexo  |   qtd    |
+-------+-------+----------+
| 2017  | M     | 1834490  |
| 2017  | F     | 1711811  |
| 2016  | M     | 1889052  |
| 2016  | F     | 1763916  |
| 2015  | M     | 1909804  |
| 2015  | F     | 1778883  |
+-------+-------+----------+


# 6. Mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo com o nome iniciado com ‘A’

select ano, sexo, sum(frequencia) as qtd from nascimento where nome like 'A%' group by ano, sexo order by ano desc;

+-------+-------+---------+
|  ano  | sexo  |   qtd   |
+-------+-------+---------+
| 2017  | M     | 185566  |
| 2017  | F     | 308551  |
| 2016  | M     | 191854  |
| 2016  | F     | 324185  |
| 2015  | M     | 194722  |
| 2015  | F     | 329690  |
+-------+-------+---------+



# 7. Qual nome e quantidade das 5 crianças MAIS nascidas em 2016

select nome, max(frequencia) as qtd from nascimento where ano=2016 group by nome order by qtd desc limit 5;

+---------+--------+
|  nome   |  qtd   |
+---------+--------+
| Emma    | 19471  |
| Olivia  | 19327  |
| Noah    | 19082  |
| Liam    | 18198  |
| Ava     | 16283  |
+---------+--------+



# 8. Qual nome e quantidade das 5 crianças MAIS nascidas em 2016 do sexo masculino e feminino

select nome, max(frequencia) as qtd, sexo from nascimento where ano=2016 group by nome, sexo order by qtd desc limit 5;

+---------+--------+-------+
|  nome   |  qtd   | sexo  |
+---------+--------+-------+
| Emma    | 19471  | F     |
| Olivia  | 19327  | F     |
| Noah    | 19082  | M     |
| Liam    | 18198  | M     |
| Ava     | 16283  | F     |
+---------+--------+-------+


# E para os 5 mais nascidos do sexo 'F' e os 5 mais nascidos do sexo 'M'
select nome, max(frequencia) as qtd, sexo from nascimento where ano=2016 group by nome, sexo order by qtd desc limit 10;

+-----------+--------+-------+
|   nome    |  qtd   | sexo  |
+-----------+--------+-------+
| Emma      | 19471  | F     |
| Olivia    | 19327  | F     |
| Noah      | 19082  | M     |
| Liam      | 18198  | M     |
| Ava       | 16283  | F     |
| Sophia    | 16112  | F     |
| William   | 15739  | M     |
| Mason     | 15230  | M     |
| James     | 14842  | M     |
| Isabella  | 14772  | F     |
+-----------+--------+-------+


```