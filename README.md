# Proa-OSCAR
Este repositório foi feito com o propósito de treinar consultas no banco de dados noSQl Mongodb.
Este repositório, também, apresenta exercícios e as soluções que eu encontrei.

Atividades para trabalhar com o Oscar

1- Quantas vezes Natalie Portman foi indicada ao Oscar?
- db.registro.find(
    {nome_do_indicado: "Natalie Portman"})
    
2- Quantos Oscars Natalie Portman ganhou?
- db.registro.countDocuments(
    {nome_do_indicado: "Natalie Portman"}, 
    {vencedor: true})

3- Amy Adams já ganhou algum Oscar?
- db.registro.aggregate([
    {
            $match: {
                nome_do_indicado: "Emy Adams"
            },
    },
    {
        $project: {
            vencedor: true,
            condicao: {
                $cond: {
                    if: { $eq: ["$vencedor", true]},
                    then: 1,
                    else: 0
                }
            }
        }
    }
])

4- A série de filmes Toy Story ganhou um Oscar em quais anos?

- db.registro.aggregate([
        { 
            $match: {
            nome_do_filme: "Toy Story"
        }
    },
        {
            $project: {
                vencedor: 1,
               vencedor: { $cond: {
                    if: { $eq: ["$vencedor", true]},
                    then: "Ganhou",
                    else: "Não ganhou"
                }
            }
            }
        }
    ])

5- A partir de que ano que a categoria "Actress" deixa de existir? 

-  db.registro.aggregate([
    {
        $match: {
            categoria: "ACTRESS"
        }
    },

    {
    $sort:{
        ano_cerimonia: -1
    },
    },

    {
        $project: {
            ano_cerimonia: 1
        }
    },
    {
        $limit: 1
    }
])

6- O primeiro Oscar para melhor Atriz foi para quem? Em que ano?

- db.registro.aggregate([
    {
        $match: {
            categoria: "ACTRESS"
        }
    },

    {
        $sort: {
            vencedor: -1
        },
    },

    {
        $project: {
            ano_cerimonia: 1,
            nome_do_indicado: 1
        }
    },

    {
        $limit: 1
    }
])

7- Na campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.

- db.registro.updateOne({vencedor: "true"}, {$set: {vencedor: 1}})

  db.registro.updateOne({vencedor: "false"}, {$set: {vencedor: 0}})

8- Em qual edição do Oscar "Crash" concorreu ao Oscar?

 - db.registro.aggregate([
    {
    $match: {
        nome_do_filme: "Crash"
    },
},

{
    $project: {
        _id: 0,
        cerimonia: 1
    }
},
    {
        $limit: 1
    }
])

9- Bom... dê um Oscar para um filme que merece muito, mas não ganhou.

- db.registro.insertOne({
    ano_filmagem: 2019,
    ano_cerimonia: 2020,
    cerimonia: 92,
    categoria: "Best animation",
    nome_do_indicado: "Dean DeBlois",
    nome_do_filme: "How to train your dragon 3",
    vencedor: 1
})


10- O filme Central do Brasil aparece no Oscar?

-  db.registro.countDocuments({ nome_do_filme: /Central do Brasil/i })

11- Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser.

- db.registro.insertMany([
    {
    ano_filmagem: 2017,
    ano_cerimonia: 2018,
    cerimonia: 90,
    categoria: "ACTOR",
    nome_do_indicado: "Ansel Elgort",
    nome_do_filme: "Baby drive",
    vencedor: 1
    },
    
    {
        ano_filmagem: 2019,
        ano_cerimonia: 2020,
        cerimonia: 92,
        categoria: "Best soundtrack",
        nome_do_indicado: "Matthew Margeson",
        nome_do_filme: "Rocketman",
        vencedor: 1
    },

    {
        ano_filmagem: 2012,
        ano_cerimonia: 2013,
        cerimonia: 85,
        categoria: "ACTOR",
        nome_do_indicado: "Ryan Gosling",
        nome_do_filme: "Drive",
        vencedor: 1
    }
])


12 - Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?

- db.registro.aggregate([
    {
        $match: {
          ano_cerimonia: 2003
        }
    },
    {
        $project: {
            nome_do_filme: 1,
            nome_do_indicado: 1,
            diretor: "Roman Polanski"
        }
    },
    {
        $limit: 1
    }
])
