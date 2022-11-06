# secret-scanning-action

Essa Action apresenta uma maneira de rodar o [Gitleaks](https://github.com/zricethezav/gitleaks) no seu github action workflow.

O principal objetivo dessa action é realizar um scan procurando por secrets, credenciais ou outras informações sensíveis dentro dos commits de um pull-request específico, ou seja, o secret scanning ocorrerá somente dentro dos commits compreendidos no pull-request que está executando essa action.

> NOTE: É recomendado que você utilize essa action num workflow de pull-request, dessa forma é possível garantir um check de segurança antes do merge na branch default.

O scan utiliza o arquivo de [regexes](https://github.com/zricethezav/gitleaks/blob/master/config/gitleaks.toml) default do [Gitleaks](https://github.com/zricethezav/gitleaks) no seu github action workflow. 

## Inputs

| Name          | Required | Type   | Default value                    | Description                                              |
| ------------- | -------- | ------ | -------------------------------- | -------------------------------------------------------- |
| image        | true    | string | zricethezav/gitleaks:v8.15.0                | The docker image to run gitleaks scan.           |
| exit_code        | false    | string | '0'  | If different from 0, the job will be break if a leak were found.         |
| slack_webhook_url | true    | string |        ""                      | The URL of Slack Webhook.                     |
| github_token        | true    | string   |    ""                         | GitHub Token for API calls. |
| tag        | false    | string   | main                             | Deploy tag.               |
| slack_channel          | true    | string   | ""                             | slack channel id.                                 |

> NOTE: slack_webhook_url e github_token são informações sensíveis. Evite passar essas informações via texto plano. Uma boa alternativa é utilizar [Github Action Secrets](https://docs.github.com/en/rest/actions/secrets)


## Exemplo de uso:

> **NOTE:** Você deve utilizar um step de actions/checkout antes do step `secret-scanning-action`. Caso esteja usando `actions/checkout@v3` você deve especificar a quantidade de commits que será baixada (depth).
>
> Utilize a opção `fetch-depth` '0' para clonar o repositório inteiro. 

```yaml
- name: Checkout Application Repository
  uses: actions/checkout@v3
    with:
      fetch-depth: "0"

- name: Secret Scanning
  uses: igordevopslabs/secret-scanning-action@0.1.0
    with:
      slack_webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}
      slack_channel: 'SLCHEXEMP10'
      github_token: ${{ secrets.GITHUB_TOKEN }}
```

Essa action simplifica algumas opções de notificação em caso de secrets que foram enviadas ao repositório remoto por engano.
Se durante a execução da action nenhuma secret for detectad, os steps de notificação não serão executados.

### Notificações disponíveis:

  * Comentário no Pull Request atual onde o scan está sendo executado.
  * Alerta num canal específiico do Slack.

### Comentário no PR:
![imagem](https://user-images.githubusercontent.com/73206099/200149543-e599677c-7b32-444f-8156-5ada55858ea8.png)


### Notificação do Slack:
![imagem](https://user-images.githubusercontent.com/73206099/200149739-96e8b42a-0b43-47cf-a7e2-ea3d7661c115.png)


## Como remover uma secret do git history?

[GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) possui um artigo para o uso do [BFG Repo Cleaner](https://rtyley.github.io/bfg-repo-cleaner/).