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
