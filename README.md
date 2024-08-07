# Como Identificar e Obter o rqdata do hCaptcha
![](https://assets.capsolver.com/prod/images/post/2024-07-16/26d592a7-97e4-4e29-9d8e-cb7ce7fd8923.png)

O hCaptcha aumentou o nível de dificuldade ao exigir dados adicionais—rqdata—para validar suas tentativas. Se você perder essa peça crítica do quebra-cabeça, seus esforços podem ser em vão, levando a tokens inválidos e muita frustração. Mas não se preocupe, nós temos tudo o que você precisa.

Este guia o levará pelos passos para identificar e obter o rqdata do hCaptcha, garantindo que você possa resolver esses captchas como um profissional. Seja você um desenvolvedor, um entusiasta de segurança, ou apenas alguém cansado de clicar em imagens intermináveis, este artigo é para você.

Vamos dividir em quatro passos simples:
1. **Determinar se o rqdata é necessário**: Aprenda a usar ferramentas para verificar se o rqdata é necessário.
2. **Localizar o valor do rqdata**: Use ferramentas de desenvolvedor para encontrar e copiar o valor do rqdata.
3. **Usar o rqdata para resolver o hCaptcha**: Inclua o rqdata na sua solicitação de solução de captcha.
4. **Solucionar problemas comuns**: Aborde problemas comuns que você pode encontrar ao lidar com o rqdata.

Ao final deste artigo, você terá o conhecimento necessário para navegar pelas exigências do rqdata do hCaptcha com facilidade, transformando esses captchas irritantes em um passeio no parque.

## Passo 1: Determine se você precisa enviar o rqdata

Antes de começar a resolver o hCaptcha, você precisa primeiro determinar se precisa enviar o rqdata. A falha em enviar esses dados quando necessário resultará em um token inválido ou uma alta probabilidade de invalidação do token. Para determinar se o rqdata é necessário, você pode usar o recurso de detecção de captcha do [Capsolver](https://www.capsolver.com/). Você pode aprender mais sobre esta ferramenta aqui:

[Saiba mais sobre o detector de captcha do Capsolver](https://www.capsolver.com/blog/Extension/identify-any-captcha-and-parameters)

Quando o rqdata é necessário, a extensão do Capsolver exibirá um painel conforme mostrado abaixo:
![](https://assets.capsolver.com/prod/images/post/2023-11-21/7a1cf833-e03d-43c9-9519-590dcaad4816.png)

## Passo 2: Encontre onde obter este valor

Esta etapa cobre o processo de obtenção do valor do rqdata. Primeiro, abra a ferramenta Inspecionar Elemento (F12), navegue até a aba de rede e ative o hCaptcha. Um URL de solicitação aparecerá assim:

```
https://api.hcaptcha.com/getcaptcha/9928de2d-2800-4c58-be91-060e5a6aa117
```

Note que a parte 'sitekey' (9928de2d-2800-4c58-be91-060e5a6aa117) variará. Inspecionando o payload da solicitação POST, revela-se um parâmetro "rqdata", do qual você pode copiar o valor.
![](https://assets.capsolver.com/prod/images/post/2023-11-21/fe716b22-f15a-4ff0-92b4-b78d57d9bcc3.png)

Em seguida, na ferramenta inspecionar elemento, pressione CTRL + S para abrir o painel de pesquisa. Insira o valor do rqdata aqui, e a solicitação que gerou esse valor deve aparecer.
![](https://assets.capsolver.com/prod/images/post/2023-11-21/c0ffbca2-f8c7-4892-bc1d-961986adcfdc.png)

Em alguns casos, este método pode não funcionar se o valor estiver codificado em HTML, criptografado ou localizado em outro lugar. É aconselhável examinar o corpo da resposta de cada solicitação para localizar de onde este valor se origina.

Tenha em mente que este valor muda a cada vez. Portanto, é essencial raspá-lo novamente antes de cada envio de captcha para garantir que o token do captcha permaneça válido.

Ao especificar os dados no formato necessário, deve ser inserido da seguinte forma:

```json
 "enterprisePayload": {
      // Opcional, necessário se o site tiver HCaptcha Enterprise
      "rqdata": "[valor]"
    },
```

## Passo 3: Resolver o hCaptcha usando o rqdata

Depois de obter o rqdata, você precisa incluí-lo na sua solicitação de solução de captcha. Abaixo está um exemplo de código mostrando como incluir o rqdata na solicitação:

```python
import requests

url = 'https://api.hcaptcha.com/siteverify'

data = {
    'secret': 'your_secret_key',
    'response': 'captcha_response',
    'rqdata': 'obtained_rqdata'
}

response = requests.post(url, data=data)
result = response.json()

if result['success']:
    print('Captcha resolvido com sucesso!')
else:
    print('Falha ao resolver o captcha.')
```

Neste exemplo, `your_secret_key` é a sua chave secreta do hCaptcha, `captcha_response` é a resposta que você obtém do hCaptcha, e `obtained_rqdata` é o valor do rqdata que você obteve no Passo 2.

## Passo 4: Solucionar Problemas Comuns

### Problema 1: Valor do rqdata não encontrado
Se você não conseguir encontrar o valor do rqdata nas solicitações de rede, tente os seguintes passos:
1. Certifique-se de que você ativou corretamente o hCaptcha e que todas as solicitações de rede estão sendo registradas.
2. Verifique o corpo da resposta de todas as solicitações para garantir que você não perdeu nenhuma solicitação contendo o rqdata.
3. Tente repetir os passos em diferentes navegadores ou dispositivos para descartar problemas específicos do navegador.

### Problema 2: Valor do rqdata inválido
Se o valor do rqdata submetido for inválido, certifique-se de que:
1. O valor do rqdata é o mais recente, pois ele muda a cada vez.
2. Verifique se o valor do rqdata está corretamente codificado ou decodificado.
3. Certifique-se de que o valor do rqdata não está sendo modificado ao enviar a solicitação.

## Resumo

A chave para resolver com sucesso o hCaptcha é identificar e obter corretamente o valor do rqdata. Usando ferramentas de desenvolvedor e o recurso de detecção de captcha do Capsolver, você pode efetivamente encontrar e usar o rqdata para garantir a validade do token do captcha. Ao lidar com o hCaptcha, seja paciente e meticuloso para garantir que cada etapa seja executada corretamente.
