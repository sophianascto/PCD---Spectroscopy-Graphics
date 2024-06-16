# PCD---Spectroscopy-Graphics
Descrição breve do projeto.

## FWHM
Primeiro selecione o arquivo de dados escolhido e defina as bandas para achar o pico definido. Essa parte do código é customizada.

```python
df = pd.read_csv("seus_dados.txt")
df = df[(df["Wavelength (nm)"] >= 300) & (df["Wavelength (nm)"] <= 500)]
```
A parte a seguir é inalterável, ela define as linhas de mínino e máximo dos pontos do pico escolhido e calcula a largura à meia altura.

```python
max_val = df2["Absorvância"].max()
min_val = df2["Absorvância"].min()

meia_altura = (max_val - min_val)/2
meia_altura_abs = df.iloc[(df["Absorvância"] - meia_altura).abs().argsort()[:2]]
fwhm = (meia_altura_abs.iloc[0] - meia_altura_abs.iloc[1])["Wavelength (nm)"]
print(fwhm)
```
FWHM (full width half maximum/largura à meia onda) é uma métrica utilizada para a caracterização e análise de dados experimentais em várias técnicas químicas. Em espectroscopia de UV-Vis é usada para caracterizar a largura dos picos de absorção, que pode fornecer informações sobre a pureza da amostra e sobre a interação entre as moléculas.

## Picos de absorção traçados por uma tangente
Primeiro utilizamos três funções: uma para ler o arquivo .txt, outra para normalizar a absorção que pode ter diferentes escalas e no fim para tirarmos os pontos máximos que irão ser comparados. 

```python
# Ler os dados do arquivo .txt
def ler_dados(arquivo_txt):
    # Ler o arquivo .txt em um DataFrame, supondo que as colunas são separadas por espaços
    df = pd.read_csv(arquivo_txt, sep='\s+', header=None)
    return df

# Normalizar a absorção
def normalizar_coluna(df, coluna_index, valor_max):
    df.iloc[:, coluna_index] = (df.iloc[:, coluna_index] / df.iloc[:, coluna_index].max()) * valor_max
    return df

# Encontrar os pontos máximos
def encontrar_ponto_maximo(df):
    indice_maximo = df.iloc[:, 1].idxmax()
    x_max = df.iloc[indice_maximo, 0]
    y_max = df.iloc[indice_maximo, 1]
    return x_max, y_max
```
A parte a seguir é alterável, ela puxa os arquivos do seu diretório. Você pode usar quantos arquivos quiser.

```python
# Caminhos para os arquivos .txt
arquivo1 = 'seu_arquivo1.txt'  
arquivo2 = 'seu_arquivo2.txt'   

# Ler os dados dos arquivos
df1 = ler_dados(arquivo1)
df2 = ler_dados(arquivo2)

# Normalizar a segunda coluna do primeiro DataFrame. Aqui usamos um exemplo para que o maior valor seja 1.4
df1 = normalizar_coluna(df1, 1, 1.4)
df2 - normalizar_coluna(df3, 1, 1.4)
```
Agora basta criar o gráfico e ligar os pontos:

```python
# Criar o gráfico
plt.figure(figsize=(10, 6))

# Plotar o primeiro arquivo normalizado
plt.plot(df1.iloc[:, 0], df1.iloc[:, 1], label='Extinção Ideal para 63 nm', c = '#800000')
x_max1, y_max1 = encontrar_ponto_maximo(df1)
plt.scatter(x_max1, y_max1, color='red', label=f'Ponto Máximo Tabela 1: ({x_max1} nm)') #define o ponto máximo que será comparado

# Plotar o segundo arquivo normalizado
plt.plot(df2.iloc[:, 0], df2.iloc[:, 1], label='Extinção da Síntese II', c = 'orange')
x_max2, y_max2 = encontrar_ponto_maximo(df2)
plt.scatter(x_max2, y_max2, color='red', label=f'Ponto Máximo da Síntese II: ({x_max2} nm)') #define o ponto máximo que será comparado
```
Por fim use plt.plot nos pontos encontrados para conectar os pontos.
```python
plt.plot([x_max1, x_max2], [y_max1, y_max2], linestyle='--', color='black')
plt.plot([x_max2, x_max3], [y_max2, y_max3], linestyle='--', color='black')
```
A comparação dos picos de máxima absorção é uma prática central na química analítica e em várias outras disciplinas químicas, sendo crucial para a identificação de compostos, quantificação, monitoramento de reações e estudo de interações moleculares. Por exemplo, os espectros obtidos pela espectroscopia UV-Vis apresentam picos característicos para diferentes compostos. Comparar esses picos de absorção máxima permite identificar quais compostos estão presentes na amostra. Cada composto absorve luz em comprimentos de onda específicos, resultando em picos de absorção distintos. Utilizamos desse conhecimento muitas vezes durante as aulas de laboratório do primeiro semeste.


