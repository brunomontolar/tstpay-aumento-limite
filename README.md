# Análise Aumento de Limite
## Tabelas utilizadas
- 1.01.07 -- Ocorrências de Alteração de Limite
    - Histórico das alterações de limite.
    - Mostra apenas a última alteração de limite (um registro por cliente).
- 1.02.01 -- Clietes Cadastrados
    - Base de Clientes.
    - Data de Cadastro
    - Limite Atual
- 1.05.01 -- Pagamento de Faturas
    - Pagamentos realizados por fatura, 
    - Pode conter mais de um registro por fatura
    - Pode conter pagamentos maiores do que o valor da fatura
    - Exemplo: Pagamentos parcelados
- 1.06.01 -- Faturas
    - Faturas emitidas, somente existe um registro por fatura

## Regras para aumento do limite
- Cliente ter pelo menos 180 dias de cadastro
- Considerar apenas faturas vencidas a não mais de 180 dias
- Cliente não ter tido nenhum aumento de limite nos últimos 90 dias
- Cliente ter gasto médio de pelo menos 30% do limite
- Tolerância para pagamento da fatura (tem que atender a todas as condições):
    - Máximo de R$10,00 pago a menos
    - Máximo de 2% pago a menos
    - Máximo de 10 dias de atraso para pagamento
- Considera no máximo as 6 últimas faturas do cliente
- Cliente com status ATIVO

## Regras para sugestão de novo limite
- R$4.000,00 de teto do novo limite 
- Aumento escalonado com base no limite anterior:
    - Até R\$1.000,00 - 50% de aumento
    - De R\\$1.000,01 a R\\$2.000,00 - 40% de aumento
    - De R\\$2.000,01 a R\\$3.000,00 - 30% de aumento
    - De R\\$3.000,01 a R\\$4.000,00 - 20% de aumento

## Observações
- Clientes ficam com o status bloqueado caso atrasem o pagamento em mais de 3 dias úteis. Nestes casos, caso o cliente esteja com a fatura mais recente atrasada, ele será desconsiderado, independentemente da tolerância de 10 dias
- Para o cálculo do gasto médio, é considerado o valor pago das faturas em relação ao limite. Desta forma, se o cliente pagou a fatura a mais, foi considerada a situação como o cliente aumentando seu consumo e próprio limite.

## TODO
- Definir formato de automatização da rotina para processamento mensal
- Parametrizar variável ```data_bases``` para ser em função da data dos arquivos
- Definir formato do arquivo CSV.
    - Há diferença no formato do arquivo quando exportado manualmente ou quando exportado automaticamente via agendamento do Fiabilité
    - Em função do formato, é necessário fazer os seguintes ajustes:
        - Mudança do encoding na função ```read_csv``` de cada arquivo
        - Mudança no tratamento dos valores R$, conforme vírgula e ponto como separadores de milhares e decimais na função ```convert_currency_value```        