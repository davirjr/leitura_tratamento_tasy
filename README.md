# Limpeza e Análise de Dados Exportados do Sistema Tasy

### **Descrição do Projeto**
Este projeto aborda a limpeza e preparação de dados extraídos do sistema de gestão hospitalar Tasy. O objetivo é transformar dados brutos e inconsistentes em um formato utilizável para análise exploratória dos dados, como o monitoramento de tempos de entrega de medicamentos urgentes. 

Os arquivos exportados do Tasy frequentemente apresentam desafios, como formatações irregulares e valores nulos. Aqui, mostramos como superar essas dificuldades utilizando Python e bibliotecas populares de ciência de dados.

---

### **Contexto**
Os dados analisados neste projeto foram extraídos do sistema Tasy e envolvem operações de farmácia hospitalar. Os principais objetivos desse projeto são:
- **Automatizar a análise** de tempos de entrega para agilizar a tomada de decisão.
- **Aumentar a precisão** nas análises eliminando inconsistências nos dados.
- **Promover eficiência operacional** utilizando métodos avançados de limpeza e manipulação de dados.

---

### **Recursos Utilizados**
- **Linguagem**: Python 3.9
- **Bibliotecas Principais**:
  - `pandas` - Manipulação e limpeza de dados.
  - `numpy` - Operações matemáticas e cálculos.
  - `matplotlib` / `seaborn` - Visualização de dados (opcional).
  - `datetime` - Manipulação de datas e horas.

---

### **Estrutura do Projeto**
- **`data/`**: Contém arquivos de dados simulados (não inclui dados confidenciais reais).
- **`scripts/`**: Scripts Python para limpeza e análise. Adicionalmente deixei algumas análise que só foram possíveis com o processo desse projeto. 
- **`notebooks/`**: Notebooks para exploração e desenvolvimento.

---

### **Como Funciona o Processo**
#### **1. Ajuste dos Cabeçalhos**
Os arquivos do Tasy frequentemente não apresentam cabeçalhos adequados para uma leitura em python. Esses são definidos manualmente com base nos cabeçalhos originais dos relatórios utilizados (O exemplo abaixo está o exemplo das colunas para um relatório personalizado que tem como objetivo explorar os tempos de entrega da farmácia):
```python
columns = [
    'Nr atendimento', 'Setor paciente', 'Setor prescricao', 
    'Ds local estoque', 'Cd material', 'Ds material', 
    'Nr sequencia', 'Dt prescricao', 'Dt entrega setor', 
    'Dt recebimento setor', 'Dt geracao lote', 'Ds classificacao', 
    'Dt ger lote', 'Hr ger lote', 'Dt entr setor', 'Hr entr setor', 
    'Usuario entrega'
]
```

#### **2. Conversão de Datas e Horas**
As datas e horas são convertidas para o formato apropriado utilizando a biblioteca `pandas`:
```python
dados['Dt prescricao'] = pd.to_datetime(dados['Dt prescricao'], format='%d/%m/%Y %H:%M:%S', errors='coerce')
```

#### **3. Remoção de Valores Faltantes**
Linhas com dados críticos ausentes (Alguns processos acabam sendo registrados no mesmo formato, e isto gera os dados 'faltantes' - na verdade esses dados são referêntes a lotes que não considerados nem 'agora' nem 'normais'), como classificações ou números de sequência, são removidas:
```python
dados_limpos = dados.dropna(subset=['Ds classificacao'])
```

#### **4. Criação de Novas Métricas**
São criadas métricas derivadas, como o tempo de entrega em minutos (Neste caso é uma simples subtração entre a hora de entrega menos a hora de geração - o próprio tasy poderia encaminhar o dado, porém, no relatório apresentado ele está faltante):
```python
dados_limpos['Tempo de entrega'] = (
    dados_limpos['Dt entrega setor'] - dados_limpos['Dt prescricao']
).dt.total_seconds() / 60
```

---

### **Como Reproduzir**
1. **Clone o Repositório**
   ```bash
   git clone https://github.com/seu-usuario/nome-do-repositorio.git
   cd nome-do-repositorio
   ```

2. **Instale as Dependências**
   ```bash
   pip install -r requirements.txt
   ```

3. **Execute o Script**
   Rode o script de limpeza diretamente:
   ```bash
   python scripts/limpeza_tasy.py
   ```

4. **Explore o Notebook**
   Abra o arquivo Jupyter Notebook para análise exploratória:
   ```bash
   jupyter notebook notebooks/limpeza_tasy.ipynb
   ```

---

### **Exemplo de Resultados**
- **Tempo Médio de Entrega:** 22 minutos.
- **Percentual de Atrasos (> 30 min):** 15%.
- **Outliers Identificados:** 8 registros (Q3 + 4 * IQR).

---

### **Contribuição**
Contribuições são bem-vindas! Para colaborar:
1. Faça um fork do projeto.
2. Crie uma branch para sua feature:
   ```bash
   git checkout -b minha-nova-feature
   ```
3. Envie um pull request.

---

### **Contato**
- Nome: Davi
- Email: davirjr@gmail.com
- LinkedIn: [https://linkedin.com/in/seu-perfil](https://www.linkedin.com/in/davi-rodrigues-junior-b1b822b4/)
