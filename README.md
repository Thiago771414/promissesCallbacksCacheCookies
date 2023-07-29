# Cache
O cache é um mecanismo de armazenamento temporário de dados frequentemente acessados, com o objetivo de melhorar o desempenho do sistema. Ele pode ser usado em diversos níveis, como hardware (por exemplo, cache de CPU) ou software (como o cache de aplicativos ou de API). Em relação ao cache de API, é uma técnica que permite armazenar temporariamente as respostas de chamadas de API para acelerar o acesso em requisições futuras idênticas ou semelhantes. O controle de cache em API é a capacidade de gerenciar e controlar o comportamento do cache, definindo configurações para determinar quais chamadas de API devem ser armazenadas em cache, por quanto tempo e como atualizar ou invalidar o cache quando os dados subjacentes mudam.

# Cookies
Cookies são pequenos arquivos de texto armazenados no dispositivo do usuário por um navegador da web. Eles são usados para armazenar informações específicas sobre o usuário e o comportamento do site. Os cookies são amplamente utilizados para rastrear sessões de usuário, armazenar preferências, lembrar carrinhos de compras, autenticar usuários e personalizar a experiência do usuário. Existem dois tipos principais de cookies: cookies de sessão, que são temporários e são apagados quando o navegador é fechado, e cookies persistentes, que permanecem no dispositivo do usuário por um período determinado ou até serem explicitamente excluídos. Os cookies são essenciais para muitas funcionalidades da web moderna, mas é importante garantir que sejam usados de forma ética e em conformidade com as regulamentações de privacidade e proteção de dados.

# Callbacks:
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
# Promises:
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

# Controle de cahce
você pode utilizar variáveis para armazenar a resposta da requisição da API e definir um tempo de validade para o cache. Dessa forma, você pode verificar se o cache ainda é válido antes de realizar uma nova requisição à API. Caso o cache esteja válido, você pode utilizar os dados armazenados em cache, caso contrário, você realiza uma nova chamada à API.

Aqui está um exemplo de como você pode fazer isso:
````javascript
document.addEventListener('DOMContentLoaded', function() {
  // Verifica se há dados em cache e se o cache está válido antes de fazer uma nova requisição
  const cacheKey = 'bancosCache';
  const cacheExpiry = 60 * 60 * 1000; // Tempo de validade do cache em milissegundos (1 hora)

  const cachedData = JSON.parse(localStorage.getItem(cacheKey));
  const currentTime = new Date().getTime();

  if (cachedData && currentTime - cachedData.timestamp < cacheExpiry) {
    // Utiliza os dados armazenados em cache
    exibirMensagem(cachedData.quantidadeBancos);
  } else {
    // Faz uma nova requisição à API e armazena os dados em cache
    carga(cacheKey);
  }
});

function carga(cacheKey) {
  fetch('https://brasilapi.com.br/api/pix/v1/participants')
    .then(response => response.json())
    .then(data => {
      const quantidadeBancos = data.length;
      // Armazena os dados em cache
      const cacheData = {
        quantidadeBancos: quantidadeBancos,
        timestamp: new Date().getTime()
      };
      localStorage.setItem(cacheKey, JSON.stringify(cacheData));

      exibirMensagem(quantidadeBancos);
    })
    .catch(error => {
      console.log('Ocorreu um erro:', error);
    });
}

function exibirMensagem(quantidadeBancos) {
  const mensagem = `Hoje ${quantidadeBancos} bancos aceitam PIX`;
  document.getElementById('output').innerText = mensagem;
}
````
Neste exemplo, os dados da resposta da API são armazenados no LocalStorage com um carimbo de data/hora (timestamp). Antes de fazer uma nova requisição à API, o código verifica se o cache ainda é válido com base no tempo de validade (cacheExpiry) estabelecido. Se o cache estiver válido, os dados armazenados em cache são utilizados; caso contrário, uma nova requisição à API é feita, e os dados são atualizados no cache. Isso permite que as informações sejam recuperadas de forma mais rápida em futuras visitas à página, reduzindo o tráfego na rede e melhorando a experiência do usuário.
