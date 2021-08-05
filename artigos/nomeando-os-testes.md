<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Nomeando os nossos testes

Em testes normalmente nos deparamos com o dilema entre as abordagens DRY e DAMP. DRY significa Don't Repeat Yourself, que é o que fazemos normalmente quando estamos desenvolvendo para ter mais reutilização e menos repetição. DAMP significa Descriptive and Meaningful Phrases, onde o que é mais importante é a legibilidade, mesmo que isso signifique ser um pouco mais repetitivo. Quando falamos principalmente do nome dos nossos testes é importante ser mais DAMP, de forma que ao ler os describes e its nós já saibamos o que aquele teste faz sem precisar investir muito tempo lendo todo o código de teste.

Isso pode ser melhor alcançado se os testes falarem no nível de requisitos e incluírem 3 partes:

1. O que está sendo testado? Por exemplo, o método validateUserEmail

2. Sob que circunstâncias e cenário? Por exemplo, é utilizado um e-mail já cadastrado

3. Qual é o resultado esperado? Por exemplo, deve retornar um erro informando que o e-mail já foi fornecido

## Exemplos
```javascript
describe('Validate User Register Rules', () => {
  describe('validateUserEmail', () => {
    it('should throw an error with existing email address given', () => {

    })
    it('should throw an error with null', () => {
      
    })
    it('should throw an error with an email without @', () => {
      
    })
    it('should not throw an error with valid email address that doesn\'t exist already', () => {
      
    })
  })
})
```
Uma dica de como validar se os nomes dos testes estão seguindo o princípio DAMP é deixar apenas os describes e its visíveis e tentar interpretar o que os testes fazem apenas utilizando essas informações sem olhar os detalhes da implementação.
