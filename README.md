# AlfaFit — operações de homologação

Repositório público mínimo: apenas este README e um workflow manual. Não contém aplicativo, executor privado, histórico de origem, dados, mídias ou backups.

**Estado inicial: desabilitado. Não executar sem autorização posterior específica.** O único evento é `workflow_dispatch`; `execute_backup=false` por padrão, e a execução real também exige `ALFAFIT_BACKUP_ENABLED=true`. Nenhum push dispara jobs.

O executor permanece na origem privada. O checkout exige um SHA completo aprovado e um token de Contents:read limitado àquela origem; o GITHUB_TOKEN deste repositório público não concede acesso ao privado. O token de origem fica exclusivamente no Environment protegido, sem fallback para credenciais mais amplas.

Antes de habilitar qualquer execução, configurar Environment `alfafit-homologacao-backup` com required reviewer autorizado, sem bypass de administradores e somente branch `main`. Não cadastrar credenciais enquanto essas proteções não estiverem comprovadas. O workflow valida a existência da revisão e o executor privado exige registro de aprovação real. Se quem dispara também for o único reviewer, a política de self-review precisa permitir sua aprovação explícita; para exigir pessoas distintas, cadastrar um segundo reviewer autorizado antes da execução.

Variáveis iniciais de repositório: `ALFAFIT_BACKUP_ENABLED=false`. Variáveis iniciais do Environment: `ALFAFIT_BACKUP_DESTINATION_READY=false` e `ALFAFIT_RECOVERY_KEY_CUSTODIAN_CONFIRMED=false`. Não preencher quotas ou evidências fictícias. Identificadores de origem, projeto, pasta e chave ficam somente nas configurações autorizadas do Environment; não em arquivos públicos.

Runner padrão Ubuntu, timeout e concurrency limitados; action de checkout fixada em SHA. Sem artifacts, cache ou uploads de backup ao GitHub. Exportação, restore offline, criptografia e custódia são responsabilidade do executor privado revisado. Nada aqui autoriza migrations, alterações de serviços hospedados, publicação ou contratação.

GitHub disponibiliza required reviewers para repositórios públicos e runners hospedados padrão gratuitos para workflows públicos. Isso não comprova disponibilidade ou limites atuais de uma conta específica. Quotas do serviço de origem e destino privado precisam ser verificadas antes de copiar qualquer dado.

Fontes: [Environments](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments), [execução manual](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manually-run-a-workflow), [cobrança Actions](https://docs.github.com/en/billing/concepts/product-billing/github-actions).

## Trava operacional adicional (06/11/2026)

O job `backup` possui `if: ${{ false }}` no workflow publicado. Esta trava de código impede a execução real mesmo que alguém altere inputs ou variáveis. Para removê-la, será necessária uma alteração revisada do workflow, **além** de Environment com required reviewers, bypass administrativo proibido, custódia, quotas, segredos vinculados de modo seguro e autorização separada. A leitura direta em 06/11/2026 confirmou `disabled_manually`, zero execuções, required reviewer, bypass administrativo proibido e branch `main`. O cadastro de variáveis e nomes de secrets não pôde ser confirmado: os endpoints retornaram HTTP403. Não executar o workflow.

## Revisão de preparação — sem execução

`ALFAFIT_BACKUP_ENABLED=false` pertence ao nível **Repository variables**: o job `policy` consulta esse valor antes de entrar no Environment. `ALFAFIT_BACKUP_DESTINATION_READY=false` e `ALFAFIT_RECOVERY_KEY_CUSTODIAN_CONFIRMED=false` pertencem ao Environment; são lidas pelo executor depois da aprovação. Nenhuma variável substitui a trava literal `if: ${{ false }}`.

O executor revisado exige também `ALFAFIT_DRIVE_TOKEN_EXPIRES_AT` no Environment, correspondente à validade do mesmo access token que vier a ser autorizado: pelo menos50min restantes no início do passo de backup e5min antes do upload. Não há refresh automático. Não emitir ou cadastrar credenciais enquanto a execução estiver proibida. O teste de upload feito por um cliente local não comprova quota, ACL completa ou validade da futura credencial no runner.

Esta branch só prepara correções para revisão; não modifica as proteções, não cadastra configurações e não executa o workflow.
