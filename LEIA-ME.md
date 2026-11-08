# Obra Fácil — hospedagem com Pix e aprovação manual

Pacote 3.0 para configurar e validar a hospedagem. Não está publicado. Não inclui APK comercial: o APK precisa receber a URL definitiva e ser testado no Android após a publicação.

## O que funciona neste pacote

- Cadastro e login com e-mail/senha, senhas protegidas com scrypt e sessões por cookie HttpOnly.
- Teste de 30 dias contado no servidor; R$ 29,90 em compra única.
- QR Code e Pix Copia e Cola com a chave fornecida; confirme favorecido e valor no banco antes de começar a divulgar. O nome curto do QR é CLEBERSON DA SILVA AMORIM; o banco mostra o titular real da chave. Não foi feita consulta bancária para validar a titularidade.
- Cliente informa pagador e identificação do pagamento. Isso não libera o acesso.
- Administrador confere o crédito no Nubank e libera ou bloqueia a conta; histórico de ações e verificação de referência duplicada entre contas.
- Sistema de obras com clientes, projetos, colaboradores, pagamentos, relatórios, contratos, recibos assinados e anexos PDF. Dados criptografados no navegador. Backup local exportável.

## Limites desta entrega

Os dados das obras não são sincronizados entre aparelhos. Limpar os dados do navegador sem exportar o backup pode causar perda de dados. A senha do backup é necessária para restaurar. Não existe recuperação de senha por e-mail nesta entrega. Google Drive, envio automático de e-mail e confirmação bancária automática não estão ativados. A licença exige conexão com o servidor. A aplicação web não oferece proteção absoluta contra cópias ou modificações do código pelo usuário.

## 1. Enviar ao GitHub

1. Baixe e extraia o ZIP. Não envie o ZIP fechado: o GitHub não extrai seu conteúdo.
2. Abra https://github.com/clebersonamorim22-droid/obra-facil e escolha Add file > Upload files (ou uploading an existing file).
3. Envie o conteúdo extraído, preservando as pastas public e fonte-web. Na raiz do repositório devem aparecer package.json, server.mjs e render.yaml. Não pode ficar tudo dentro de uma pasta extra ObraFacil_Hospedagem.
4. Clique em Commit changes. Mantenha o repositório privado.
5. Para enviar pastas, prefira arrastá-las na página pelo computador. O envio de pastas pelo seletor de arquivos do Android pode não preservar a estrutura. Se não tiver computador, informe essa limitação antes de enviar arquivos soltos.

Não envie dados de clientes, arquivos .env, senhas, banco SQLite ou chaves de assinatura Android ao GitHub.

## 2. Configurar a Render pelo painel

1. Em https://dashboard.render.com escolha New + > Web Service.
2. Conecte seu GitHub diretamente à Render. Autorize apenas o repositório obra-facil.
3. Selecione o repositório e a branch principal (normalmente main).
4. Preencha:

| Campo | Valor |
|---|---|
| Name | obra-facil (ou outro nome disponível) |
| Runtime / Language | Node |
| Root Directory | deixar vazio |
| Build Command | npm install --omit=dev |
| Start Command | npm start |
| Health Check Path | /health |

5. Escolha um plano pago compatível com disco persistente. Consulte e aprove o custo no próprio painel. Não use o armazenamento temporário do plano gratuito para guardar as licenças.
6. Em Advanced/Disks adicione um disco de 1 GB com Mount Path `/var/data/obra-facil`.
7. Cadastre estas variáveis em Environment:

| Variável | Valor |
|---|---|
| NODE_VERSION | 24.22.1 |
| NODE_ENV | production |
| DATA_DIRECTORY | /var/data/obra-facil |
| PUBLIC_URL | endereço HTTPS exato atribuído ao serviço, sem barra no final |
| ADMIN_PASSWORD | senha exclusiva, escolhida por você, com pelo menos 12 caracteres |

Não coloque senha em PUBLIC_URL. Não crie ADMIN_PASSWORD no GitHub. Ela deve existir somente no campo protegido da Render.

Se o endereço definitivo ainda não aparecer antes da criação, configure-o depois de criar o serviço e use Save, rebuild, and deploy. Uma inicialização sem essas configurações falha intencionalmente.

8. Clique em Create Web Service. Acompanhe Logs/Deploys. O serviço deve ficar Live.
9. Abra a URL HTTPS. Em `/health` deve aparecer `{"ok":true}`.

O arquivo render.yaml é uma alternativa para implantação por Blueprint; não é necessário utilizá-lo se seguir os campos acima. Nenhuma contratação foi feita por este pacote.

## 3. Acesso do administrador

Abra a URL do serviço, clique em Entrar ou criar conta e use:

- E-mail: clebersonamorim22@gmail.com
- Senha: o valor definido por você em ADMIN_PASSWORD.

Não cadastre uma nova conta com esse e-mail: ela é criada pelo servidor na primeira inicialização. Depois de entrar, na aba Licença clique em Abrir painel do administrador. O painel fica em `/admin`.

A senha inicial é gravada protegida no banco na primeira inicialização. Alterar ADMIN_PASSWORD depois não altera a senha existente. Não exclua o disco para redefinir senha: isso apaga as contas e as licenças. Guarde a senha com segurança.

## 4. Conferir uma venda

1. Cliente cria conta, abre a aba Licença e paga o Pix de R$ 29,90.
2. Informa nome do pagador e ID da transação ou data/horário em Já paguei.
3. Você entra no painel e confere o recebimento na conta PJ Nubank.
4. Localiza o cliente por e-mail e clica em Liberar.
5. Registra o ID real da transação do banco e confirma o recebimento. Não use a mesma referência para compradores diferentes.
6. O cliente toca em Atualizar situação; a aplicação também consulta a licença periodicamente.

A chave informada é 58016990-04bb-43a6-802d-df180d221285. Favorecido informado: 60492660CLEBERSONDASILVAAMORIM. Não há conexão do sistema à conta Nubank.

## 5. Validação antes de divulgar

- Entrar no administrador com a senha definida na Render.
- Cadastrar uma conta de cliente de teste em outro navegador.
- Confirmar valor e titular mostrados no banco ao ler o QR Code, antes de pagar.
- Testar solicitação, aprovação e bloqueio. Não anunciar liberação instantânea.
- Criar obra/cliente/colaborador e pagamento, gerar recibo assinado e consultar PDF anexado.
- Exportar e restaurar um backup de teste.
- Reiniciar o serviço e verificar que contas e licenças continuam presentes no disco.
- Definir rotinas de cópia do banco de contas/licenças e validar a restauração do disco.
- Revisar as condições comerciais, o suporte e a informação de privacidade da página inicial.

Depois da hospedagem validada, envie apenas a URL pública do serviço para gerar/configurar o APK. Não envie ADMIN_PASSWORD. O APK antigo de teste não usa automaticamente este novo servidor.

## Desenvolvimento

Node 24. `npm test` executa testes HTTP de cadastro, sessão, isolamento administrativo, origem das requisições, solicitação, aprovação, referência duplicada e bloqueio. Para executar localmente, defina ADMIN_PASSWORD, DATA_DIRECTORY e PORT; use NODE_ENV=test apenas no computador de desenvolvimento. Para reconstruir a interface: `npm install --include=dev` e `npm run build:web`.

Documentação da hospedagem: https://render.com/docs/web-services e https://render.com/docs/disks .
