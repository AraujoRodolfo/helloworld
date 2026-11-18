# PROCEDIMENTOS INTERNOS DE LANÇAMENTO

###  1. EXPORTAR AS NOTAS DO PORTAL
  > No portal Ead, exporte apenas as colunas referentes às atividades, excluindo qualquer outra como: total, média..etc
###  2. ABRIR O ARQUIVO DO EXCEL:
 - DADOS -> FILTRO -> ORDEM ALFABETICA NO NOME DOS ALUNOS
 - EXCLUIR AS COLUNAS QUE NÃO SEJAM AS NOTAS 
 - RENOMEAR AS COLUNAS COMO P1, P2, P3... (OBSERVANDO O TÍTULO DA TAREFA)
 - REOGANIZAR A ORDEM DAS COLUNAS SE NECESSÁRIO: P1, P2, P3, P4...

   > CONSIDERE A ORDEM DAS COLUNAS DE NOTAS CRIADAS NO PORTAL, DEVERÁ SER A MESMA
 - FORMATAR AS NOTAS SEM CASAS DECIMAIS, SÓ NÚMEROS INTEIROS
 - SALVAR O ARQUIVO COMO CSV
###  3. NO PORTAL:
 - CRIAR AS COLUNAS DE NOTAS NO PORTAL SEGUINDO A ORDEM DAS COLUNAS DO EXCEL COMO SP1A1, SP1A2...SP2A1..ETC
 - CONFIGURAR A FÓRMULA DAS NOTAS PARA REMOVER A DIVISÃO (PESO): **ROUND (N1+N2+N3+N4+N5+N6+N7,0)**

    > AS NOTAS JÁ TEM PESO CONFIGURADO NO PORTAL EAD, TOTALIZANDO 100. NÃO FAREMOS PESO SOBRE PESO
###  4. ABRIR A PLANILHA PARA DIGITAÇÃO DE NOTAS
 - ABRA A TELA DA UNIDADE CURRICULAR, NA GUIA NOTAS PARA QUE SEJA EXIBIDO OS CAMPOS DE DIGITAÇÃO DE CADA NOTA
 - > NÃO SERÁ NECESSÁRIO INSPECIONAR A PÁGINA PARA MAPEAR MAIS, O CÓDIGO FOI ATUALIZADO PARA FAZER ISSO SOZINHO.
##### CÓDIGO ATUALIZADO (V1.2)
```js
function importarNotasCSV() {
  const fileInput = document.createElement('input');
  fileInput.type = 'file';
  fileInput.accept = '.csv';
  
  fileInput.onchange = event => {
    const file = event.target.files[0];
    if (!file) return;

    const reader = new FileReader();
      
    reader.onload = function(e) {
      const csvContent = e.target.result;
      // Remove caracteres ocultos de quebra de linha do Windows (\r) e ignora linhas vazias no final
      const rows = csvContent.replace(/\r/g, '').split('\n').filter(row => row.trim() !== '');
      
      // Seleciona todas as linhas da tabela, ignorando a primeira que é o cabeçalho (<th>)
      const tableRows = document.querySelectorAll('#MainContent_Notas_gdvNotas tr:not(:first-child)');
      
      // i = 1 para pular o cabeçalho do CSV
      for (let i = 1; i < rows.length; i++) {
        const columns = rows[i].split(';').map(col => col.trim()); 
        const studentIndex = i - 1; // A linha 1 do CSV bate com a linha 0 de dados da tabela
        
        // Se a linha do aluno existir no HTML
        if (tableRows[studentIndex]) {
            // Pega apenas os campos de input de texto desta linha específica (os campos de nota)
            const inputs = tableRows[studentIndex].querySelectorAll('input[type="text"]');
            
            columns.forEach(function(columnValue, colIndex) {              
                if (columnValue === "-") {
                    columnValue = "0";
                }              
                
                // Se existir um input na mesma posição da coluna do CSV, insere a nota
                if (inputs[colIndex]) {
                    inputs[colIndex].value = columnValue; 
                }
            });
        }
      }
      alert("Notas importadas com sucesso!");
    };    
    reader.readAsText(file); 
  };
  
  fileInput.click();
}

// Executa a ferramenta direto
importarNotasCSV(); 

```

### 5. A MÁGICA DE LANÇAMENTO RECORD:
- COPIAR O SCRIPT ACIMA E COLAR NO CONSOLE
  > SERÁ NECESSÁRIO PERMITIR ESSA AÇÃO DIGITANDO O QUE SERÁ SOLICITADO NO CONSOLE
  > COPIAR E COLAR NOVAMENTE O CÓDIGO E PRESSIONAR ENTER
- NA JANELA QUE SERÁ ABERTA, ESCOLHA O ARQUIVO CSV PREPARADO ANTERIORMENTE
- TODAS AS NOTAS SERÃO PREENCHIDAS SEQUENCIALMENTE E AUTOMATICAMENTE

### 6. ATENÇÃO
  > DEPOIS, É SÓ SALVAR AS NOTAS
    > **CASO A PLATAFORMA EXIBA ERRO, ESTES PODEM SER OS MOTIVOS:**
    > - NOTAS PREENCHIDAS COM VÍRGULA (NÃO SÃO NÚMEROS INTEIROS)

### 7. PRONTO 🎉
