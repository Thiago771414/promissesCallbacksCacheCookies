# Cache
O cache é um mecanismo de armazenamento temporário de dados frequentemente acessados, com o objetivo de melhorar o desempenho do sistema. Ele pode ser usado em diversos níveis, como hardware (por exemplo, cache de CPU) ou software (como o cache de aplicativos ou de API). Em relação ao cache de API, é uma técnica que permite armazenar temporariamente as respostas de chamadas de API para acelerar o acesso em requisições futuras idênticas ou semelhantes. O controle de cache em API é a capacidade de gerenciar e controlar o comportamento do cache, definindo configurações para determinar quais chamadas de API devem ser armazenadas em cache, por quanto tempo e como atualizar ou invalidar o cache quando os dados subjacentes mudam.

#Cookies
Cookies são pequenos arquivos de texto armazenados no dispositivo do usuário por um navegador da web. Eles são usados para armazenar informações específicas sobre o usuário e o comportamento do site. Os cookies são amplamente utilizados para rastrear sessões de usuário, armazenar preferências, lembrar carrinhos de compras, autenticar usuários e personalizar a experiência do usuário. Existem dois tipos principais de cookies: cookies de sessão, que são temporários e são apagados quando o navegador é fechado, e cookies persistentes, que permanecem no dispositivo do usuário por um período determinado ou até serem explicitamente excluídos. Os cookies são essenciais para muitas funcionalidades da web moderna, mas é importante garantir que sejam usados de forma ética e em conformidade com as regulamentações de privacidade e proteção de dados.

#Callbacks:
Callbacks são funções que são passadas como argumentos para outras funções e executadas posteriormente, geralmente quando uma operação assíncrona é concluída. No contexto de JavaScript, os callbacks são comumente usados para lidar com operações assíncronas, como leitura de arquivos, requisições de rede e outras tarefas que não são executadas instantaneamente. O callback é chamado de volta quando a operação é concluída ou quando ocorre algum erro.

Exemplo de callback em JavaScript:

````javascript
document.addEventListener('DOMContentLoaded', function() {
  carga('', function(mensagem) {
    document.getElementById('output').innerText = mensagem;
  });
});

function carga(n, callback) {
  fetch('https://brasilapi.com.br/api/pix/v1/participants')
    .then(response => response.json())
    .then(data => {
      const quantidadeBancos = data.length;
      const mensagem = `Hoje ${quantidadeBancos} bancos aceitam PIX`;
      callback(mensagem);
    })
    .catch(error => {
      console.log('Ocorreu um erro:', error);
    });
}
````
#Promises:
Promises são uma abstração mais avançada para lidar com operações assíncronas em JavaScript. Elas permitem que você lide com o resultado (valor resolvido) ou erro (rejeição) de uma operação assíncrona de maneira mais estruturada. Em vez de passar uma função de callback diretamente, você pode retornar uma Promise que representa o resultado da operação. Isso torna o código mais legível e evita o "Callback Hell" (encadeamento excessivo de callbacks).

Exemplo de Promise em JavaScript:
````javascript
document.addEventListener('DOMContentLoaded', function() {
  carga('')
    .then(mensagem => {
      document.getElementById('output').innerText = mensagem;
    })
    .catch(error => {
      console.log('Ocorreu um erro:', error);
    });
});

function carga(n) {
  return new Promise(function(resolve, reject) {
    fetch('https://brasilapi.com.br/api/pix/v1/participants')
      .then(response => response.json())
      .then(data => {
        const quantidadeBancos = data.length;
        const mensagem = `Hoje ${quantidadeBancos} bancos aceitam PIX`;
        resolve(mensagem);
      })
      .catch(error => {
        reject(error);
      });
  });
}
````
Callbacks e Promises são frequentemente usados em conjunto com caches e cookies para melhorar o desempenho e a experiência do usuário em aplicações web.

## Caches:
Callbacks e Promises podem ser usados para controlar o comportamento do cache em aplicações. Por exemplo, ao fazer uma requisição para uma API, podemos usar Promises para lidar com a resposta e decidir se os dados devem ser armazenados em cache. Se os dados ainda não estiverem em cache, podemos fazer uma requisição à API usando callbacks e, quando a resposta chegar, armazenar os dados em cache e usar uma Promise para resolver a requisição original. Em requisições futuras, podemos verificar o cache primeiro usando callbacks para buscar os dados diretamente, economizando tempo e recursos de rede.


## Cookies:
Callbacks e Promises também são úteis ao trabalhar com cookies. Por exemplo, ao implementar um sistema de autenticação baseado em cookies, podemos usar callbacks para verificar se o cookie de autenticação está presente no navegador do usuário e, se sim, validar as informações de autenticação. Usando Promises, podemos aguardar o resultado da validação antes de permitir que o usuário acesse determinadas partes do site ou execute certas ações.

Em geral, callbacks e Promises são poderosos mecanismos que podem ser aplicados em conjunto com caches e cookies para melhorar o desempenho e a funcionalidade de aplicações web, tornando o código mais organizado e legível ao lidar com operações assíncronas e gerenciamento de dados temporários.
