# Painel de planos de ação ANEEL — Razão de Obras Analítico

Painel de acompanhamento dos planos de ação das distribuidoras do Grupo Energisa junto à ANEEL.
Arquivo único, sem servidor e sem dependências externas: o SheetJS está embutido no `index.html`
e todo o processamento acontece no navegador.

Duas abas:

- **Gerencial** — indicadores consolidados, ranking das distribuidoras por criticidade, vencimentos
  por semestre e a lista das entregas com prazo vencido.
- **Operacional** — detalhe filtrável e pesquisável, por distribuidora, plano e entrega.

Os filtros de Empresa, Ano e Situação valem para as duas abas.

## Publicar no GitHub Pages

1. Crie o repositório e envie o conteúdo desta pasta.
2. Em **Settings → Pages**, selecione a branch (`main`) e a pasta raiz (`/`).
3. O painel fica disponível em `https://<usuario>.github.io/<repositorio>/`.

O arquivo `.nojekyll` está incluído para o GitHub servir o conteúdo sem processar via Jekyll.

## Como o painel obtém os dados

Ao abrir, ele procura um `dados.json` na mesma pasta do `index.html`.

- **Encontrou** → monta os indicadores direto, sem nenhuma ação do usuário.
- **Não encontrou** → mostra a tela de upload, onde qualquer pessoa carrega a planilha localmente.

Para apontar para outro arquivo, use `?dados=outro-arquivo.json` na URL.

## Atualizar os dados publicados

1. Abra o painel e carregue a planilha de acompanhamento (.xlsx).
2. Clique em **baixar dados.json**.
3. Faça commit do `dados.json` gerado na raiz do repositório.

O `dados.json` guarda apenas os campos que o painel usa. O atraso não fica congelado no arquivo:
é recalculado contra o semestre corrente toda vez que o painel abre.

## Formato esperado da planilha

O painel identifica as abas pelos cabeçalhos, não pelo nome nem pela posição, e ignora linhas
em branco acima do cabeçalho.

**Aba de planos** — `Sigla` (ou `Empresa`), `Distribuidora`, `Nº Processo ANEEL`,
`Nº Nota Técnica`, `Ano`, `Descrição do Plano`, `Total de Etapas`, `Etapas Concluídas`.

**Aba de etapas** — `Empresa`, `Etapa / Entrega`, `Entrega`, `Data Fim Entrega`,
`Data Fim Projeto`, `Status`, `Observações`.

As duas abas são casadas por sigla + descrição do plano; se as descrições divergirem, o painel
avisa quantos planos ficaram sem etapas vinculadas. Enviando só a aba de etapas, ele deriva os
planos contando as entregas concluídas — mas sem a aba de planos os totais passam a ser os da
própria contagem.

Datas em semestre (`1º Semestre 2027`) são convertidas em período comparável. Uma entrega conta
como em atraso quando o prazo já passou e o status não é "Concluído".

## Aviso sobre os dados

**Um repositório público expõe o `dados.json` para qualquer pessoa.** Se o conteúdo do
acompanhamento for interno, use um repositório privado — o GitHub Pages funciona em repositório
privado nos planos pagos — ou não faça commit do `dados.json` e deixe cada usuário carregar a
planilha pela tela de upload. As planilhas de origem já estão bloqueadas no `.gitignore`.
