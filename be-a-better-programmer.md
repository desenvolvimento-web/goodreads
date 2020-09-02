Autor: Lucas M. Dutra ([@terremoth](https://github.com/terremoth))
Original: https://www.facebook.com/groups/desenvolvimentoweb/permalink/3030248403700245

# BE A BETTER PROGRAMMER
Me deu cegueira ao ler esse código:
https://gist.github.com/anonymous/ea4ac8ac720569df926f0b220627789e

Para quem escreve códigos assim, vai aí muitas dicas valiosas:

- Evite usar a função "mail()" do PHP - motivos: https://stackoverflow.com/questions/4565066/why-shouldnt-i-use-phps-mail-function, ao invés, prefira usar o PHPMailer que já tem classe de SMTP dentro: https://github.com/PHPMailer/PHPMailer

- Não usar short tags no PHP (<?), nem mesmo a de echo (<?= )e sim a completa: <?php . O motivo é simples: nem todo servidor aceita short tag, nem todo servidor vai deixar você mexer no php.ini para habilitar. Imagine que sua aplicação tenha sito toda escrita com short tags e o servidor não aceite, vc terá que mudar em todos os arquivos para voltar a funcionar.

- Não misturar PHP com HTML. A separação de uso e interesses deve ser respeitada, o código fica sujo, difícil de dar manutenção. Procure no mínimo colocar o php num arquivo e o HTML no outro, CSS em outro e JavaScript a mesma coisa - separe tudo.

- Não acesse variáveis globais diretamente: ($_POST, $_GET, $_COOKIE etc), o motivo é simples: há funções do *próprio* PHP que já sanitizam/limpam indesejos que possam vir na string, na tentativa de realizar um ataque na sua aplicação (ex: SQL Injection e XSS) - como a filter_input (https://www.php.net/manual/pt_BR/function.filter-input.php), é sério, aprendam a usar essa merda e parem de usar "$_" na aplicação de vocês;

- Não criem campos hidden para verificas se "houve um post na página" como foi feito com a variávei $subm - se quiser valide com dados reais que serão usados em algum lugar, ou use Rotas para entrar dentro de controllers - se chegar lá, é porquê houve requisição;

- Não misture línguas diferentes nas nomenclaturas como feitos em $name e $telefone (inglês e português...);

- Não crie nomenclaturas difíceis de serem lidas e entendidas: $__to, $__sj, ao invés disso, prefira usar nomes concisos e entendíveis: $subject e $to (PELO MENOS);

- Não instanciem HTMLs inteiros numa variável, PELO AMOR DE DELS ($html = "<!DOCTYPE html> ...);

- Não use esse tipo de exit caso algo que você não quer ocorra: exit("<h3>Sem meta/header inclusões, por favor.</h3>"); iria mostrar uma página em branco apenas com o H3, isso não é bom para experiência do usuário, prefira fazer esse tipo de tratamento via javascript.

- Prefira usar UTF-8 como default charset em todas as suas aplicações do que usar ISO-8859-1 como em "<meta charset=\"iso-8859-1\">", o motivo também é simples: a cadeia de caracteres que UTF-8 abrange é muito (tipo muito mesmo) maior, e você consegue alocar diferentes tipos de caracteres e linguas na sua página - caso você queira saber um pouco mais, dê uma lida em https://dutrahacking.blogspot.com/2014/12/como-resolver-problemas-de-charset.html;

- Não use chamadas de funções dentro de iterações: como em "for ($i = 0; $i < sizeof($attach); $i++):", as comumente usadas são sizeof e count, toda vida que vai validar se $i é menor que o sizeof, ele re-entra na função sizeof. Qual o problema? Não é errado mas a iteração fica muito mais lenta (teste por si mesmo) do que se você tivesse instanciado tudo numa variável: $attachSize = sizeof($attach); e depois:
"for ($i = 0; $i < $attachSize); $i++):"

- Não use iniciais de nomenclaturas não-padronizadas (minúsculas e maiúsculas, por exemplo como visto em $body e depois $Size. Pick one, padronize sua aplicação.

- Não chame nem confunda base64 com criptografia, base64 é só uma codificação de caracteres baseado em operações bitwise para que os caracteres estejam dentro da tabela ascii e sejam aceitos em diversas aplicações sem se preocupar com o charset delas (ao meu ver esse é o principal motivo usado), visto em $cript = base64_encode($fread);

- Ao invés de usar for/endfor, if/endif, use chaves, fica mais curto e limpo o código e os editores de texto e IDE's conseguem achar mais facilmente e dar suporte a abrir e fechar a estrutura caso precise de uma melhor leitura do código;

- Você não precisa fechar a tag do php ?> nos seus arquivos que só contenham php, seriously;

- Use chaves sempre quando usar if's e else's etc, isso garante melhor legibilidade, qualidade e manutenção do código - tipo } else echo 'entrou aqui';

- Tente não acoplar muitas estruturas de seleção e repetição (3 ou mais) num mesmo lugar, isso dificulta a leitura do código, dificulta a manutenção e significa que o processo/lógica de negócio está começando a se tornar complexa - tente separar os processos em funções e chame as funções onde começar a complexidade para melhorar o código e a vida de quem vai programar.

PHP já tem diversos problemas, tenta não problematizar o problema.
