<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Boas práticas no uso de mocks

Quando estamos implementando testes unitários ou de integração utilizamos dublês de testes para garantir a independência dos nossos testes, isolar aplicações externas e tornar o teste assertivo na validação de pequenos trechos de código.

Porém tão importante quanto implementar dublês de testes para termos testes assertivos é implementá-los da maneira correta. Devido a isso listamos abaixo algumas boas práticas no uso de mocks.

## Restaure o estado no final do teste

Quando estamos implementando dublês de testes estamos alterando o comportamento da aplicação em relação ao que foi implementado originalmente para podermos validar algum trecho de código de forma assertiva.

Como esse dublê de teste faz sentido apenas para aquele teste precisamos garantir que os dublês utilizados sejam limpos ao final do teste para não impactar os outros testes.

Por isso procure entender como restaurar o estado da aplicação ao final do teste de acordo com a biblioteca e linguagem que está utilizando.

No exemplo abaixo, com [Jest](https://www.npmjs.com/package/jest), é utilizado `afterEach(() => jest.clearAllMocks())`.

```javascript
jest.mock('../src/promiseTestModule')

describe('Mock', () => {
  afterEach(() => jest.clearAllMocks())

  test('mockResolvedValue() - Resolvendo uma Promise e mockando um módulo', async () => {
    axios.get.mockResolvedValueOnce({
      data: {
        usuarios: [
          {
            nome: 'Fulano da Silva',
            email: 'fulano.teste@qa.com'
          }
        ]
      }
    })

    const user = await src.getUserByEmail('fulano.teste@qa.com')

    expect(user.email).toBe('fulano.teste@qa.com')
  })
})
```

> O exemplo acima foi retirado do repositório [Dublês de teste com Jest](https://github.com/PauloGoncalvesBH/dubles-de-teste-com-jest/blob/b6386701d91b6ef512d82ace77b307034c215517/tests/mock.test.js#L7).

## Evite uso excessivo de stub

Embora dublê de teste seja uma ótima solução para tornar os testes mais independentes, o seu uso excessivo é uma armadilha na eficiência dos testes.

É preciso atentar com a quantidade de stubs implementados, pois dependendo da forma e quantidade que foi implementado o teste pode tornar menos eficaz, pois podemos acabar alterando o comportamento do trecho de código que vamos validar, ao invés de alterar o comportamento de integrações que esse código possui.

O resultado é que, ao invés de validarmos o comportamento de um trecho de código isolado, estaremos é validando se o dublê de teste está funcionando. E isso acaba mascarando possíveis bugs que podem ser introduzidos futuramente.

## Teste o comportamento, não a implementação

Ao testar como uma classe/função foi implementada estamos tornando o nosso código resistente contra refatoração, pois este estará suscetível a falsos negativos. Falsos negativos reduzem a confiança do time nos testes automatizados.

Para ler mais sobre esse tópico acesse o nosso material ["Teste o comportamento, não a implementação"](./teste-comportamento-nao-implementacao.md) e veja com mais detalhes.
