# secret-scanning-action

Essa Action apresenta uma maneira de rodar o [Gitleaks](https://github.com/zricethezav/gitleaks) no seu github action workflow.


## Inputs

| Name          | Required | Type   | Default value                    | Description                                              |
| ------------- | -------- | ------ | -------------------------------- | -------------------------------------------------------- |
| image        | false    | string | zricethezav/gitleaks:latest                | The docker image to run gitleaks scan.           |
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
      github_token: ${{ secrets.GH_TOKEN }}
      slack_channel: 'SLCHEXEMP10'
```

## Como remover uma secret do git history?

[GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) possui um artigo para o uso do [BFG Repo Cleaner](https://rtyley.github.io/bfg-repo-cleaner/).