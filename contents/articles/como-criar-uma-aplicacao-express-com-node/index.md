---
 O caminho informado, procura uma variável (:fruit) opcional (?) no caminho especificado "(endereço)/rotas/";
 A função de callback, possui três parâmetros, sendo o primeiro as [informações da requisição][7], o segundo o [objeto de resposta][8] e o terceiro e mágico atributo, a função next (vou explicar...);
 O objeto "req" trará entre outras informações, as ocorrências encontradas no objeto filho "params";
 O objeto "res" corresponde à resposta da requisição atual, onde o método ".send" envia uma mensagem ao ouvinte;
 A função next força o [Express][3] a processar a próxima rota correspondente a solicitação atual.