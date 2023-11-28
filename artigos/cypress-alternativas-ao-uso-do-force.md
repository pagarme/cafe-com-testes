<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Alternativas ao uso do {force: true} nos comandos do Cypress

Quando estamos escrevendo Testes e2e no Cypress, precisamos realizar ações, como: Clicar em um botão, digitar em um campo de texto, selecionar uma opção em um menu suspenso, etc. No entanto, podemos encontrar situações em que os elementos da página não estão visíveis ou habilitados para a interação. Por exemplo: um botão pode estar escondido por um modal, um campo de texto pode estar desabilitado até que uma condição seja satisfeita, uma opção em um menu suspenso pode estar fora da área visível da tela etc. Nesses casos, o Cypress irá falhar ao tentar interagir com esses elementos, pois ele segue as mesmas regras que um usuário real seguiria, ou seja, ele só irá interagir com elementos que estão visíveis e habilitados. Mas e se quisermos forçar a interação com esses elementos, mesmo que eles não estejam em um estado adequado para isso? É aí que entra o `{force: true}`.

O `{force: true}` é um argumento opcional que podemos passar para as ações do Cypress, como .click(), .type(), .select() etc. Ele faz com que o Cypress ignore as verificações de visibilidade e habilitação dos elementos e execute a ação de qualquer forma.

# Em quais cenários seria válida a utilização?

Isso pode ser útil em alguns cenários de teste, como:

1. Queremos testar o comportamento da aplicação quando um usuário tenta interagir com elementos inválidos ou inacessíveis;
2. Queremos acelerar o tempo de execução dos testes, evitando esperar que os elementos fiquem visíveis ou habilitados;
3. Queremos contornar algum bug ou limitação do cypress ou da aplicação que impede a interação normal com os elementos.

# Em quais cenários devemos evitar a utilização?

No entanto, o uso do {force: true} também tem seus riscos e desvantagens, como:

1. Podemos introduzir falsos positivos nos testes, pois estamos violando as regras de usabilidade e acessibilidade da interface do usuário;
2. Falsos negativos: o teste pode falhar se o elemento estiver visível ou acessível para o usuário real, mas não para o Cypress;
3. Inconsistências: o teste pode ter resultados diferentes dependendo do tamanho da tela, da velocidade da rede ou de outros fatores externos;
4. Podemos perder a cobertura dos testes, pois estamos pulando etapas importantes do fluxo de interação do usuário;
5. Podemos comprometer a confiabilidade dos testes, pois estamos dependendo de uma implementação interna do cypress que pode mudar ou falhar em versões futuras.

Por isso, é recomendável evitar o uso do {force: true} sempre que possível e buscar alternativas que garantam que o teste reflita o comportamento real do usuário.

## Quais alternativas podemos utilizar?

Algumas dessas alternativas são:

- Usar seletores mais específicos para localizar os elementos desejados;

```javascript
cy.get('#id-do-elemento'); 
```

- Usar comandos, como `.should('be.visible')` ou `.should('not.be.disabled')` para verificar as condições dos elementos antes de interagir com eles;

```javascript
cy.get('#meu-elemento').should('be.visible'); // verifica se o elemento indicado está visível

cy.get('#meu-botao').should('not.be.disabled').click(); // Valida que o elemento indicado não está desabilitado e simula o click do mouse
```

- Usar comandos, como `.scrollIntoView()` ou `.trigger('mouseover')` para tornar os elementos visíveis ou acessíveis;

```javascript
cy.get('#meu-elemento').scrollIntoView(); // Usado para simular o scroll em cima do elemento selecionado

cy.get('#meu-elemento').trigger('mouseover'); // Usado para simular o comportamento do mouse em cima do elemento selecionado
```

- Usando o método em JavaScript `.dispatchEvent()` para simular uma ação, no caso abaixo, disparamos um evento do tipo _'mouseover'_ simulanto o comportamento do usuário passando o mouse sobre o elemento.

```javascript
cy.get('#meu-elemento').then($checkbox => {  // Após localizado pelo cy.get() o elemento é passado para a função como parâmetro
        $checkbox[0].dispatchEvent(new Event('mouseover'))  // Dispara um evento no primeiro elemento do conjunto de elementos encontrados
        $checkbox.trigger('click') // Simula o comportamento do usuário clicando no elemento selecionado
})
```

- Também podemos utilizar o plugin [Cypress Real Events](https://github.com/dmtrKovalenko/cypress-real-events#cyrealhover) para simular comportamentos do usuário.

```javascript
cy.get("button").realClick({ position: "topLeft" }); // clica no canto superior esquerdo do botão
cy.get("body").realClick({ x: 100, y: 1240 }); // clica nas coordenadas x e y relativas a toda a janela
cy.get("button").realHover({ position: "bottomLeft" }); // passa o mouse sobre o canto inferior esquerdo do botão
```

Essas são algumas das alternativas ao uso do `{force: true}` no Cypress que podem tornar os testes mais confiáveis, legíveis e manuteníveis.


---
## Referências:
- [Documentação do Cypress](https://docs.cypress.io/api/commands/document)
- [.dispatchEvents()](https://javascript.info/dispatch-events)
