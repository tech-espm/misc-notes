# Notas de Aula sobre Introdução de VBA

## Comentários

Um comentário é um trecho de código que serve como explicação para quem ler o código em algum momento posterior.

```vb
' Tudo que estiver depois de uma aspa simples é desconsiderado!
' Ao final da linha o comentário para de valer, e para criar um
' comentário utilizando várias linhas, devemos iniciar cada linha
' com uma aspa simples.
```

## Formato básico do corpo de um procedimento

Um procedimento é uma forma de agrupar diversas linhas de código com um propósito comum, atribuindo um nome a elas, de tal modo que elas possam ser executadas a qualquer momento, bastanto utilziar seu nome.

```vb
Public Sub NomeDoProcedimento()

    ' O código vai aqui dentro!

End Sub
```

> Para executar um procedimento, a partir do editor, basta deixar o cursor dentro do procedimento, e clicar no botão Play (ou a tecla F5).
> 
> F9 des/marca uma linha como um ponto de interrupção, fazendo com que a execução pare naquela linha, para que se possa acompanhar a execução passo-a-passo.
>
> Depois da execução ter parado em um ponto de interrupção, pode-se utilizar F5 para prosseguir, ou F8 para efetivamente seguir executando o código passo-a-passo. Também é possível iniciar a execução de um procedimento diretamente no modo passo-a-passo, bastanto utilizar a tecla F8 direto de dentro do editor.

## Tipos de dados primitivos

```vb
Boolean  ' Valor booleano (False ou True)
Byte     ' Número inteiro de 8 bits (0 até 255)
Integer  ' Número inteiro de 16 bits (-32768 até 32767)
Long     ' Número inteiro de 32 bits (-2147483648 até 2147483647)
LongLong ' Número inteiro de 64 bits (-9223372036854775808 até 9223372036854775807)
Single   ' Número real de 32 bits (precisão de aprox. 5...6 casas decimais)
Double   ' Número real de 64 bits (precisão de aprox. 15 casas decimais)
String   ' Texto
Date     ' Data e hora
```

> Para mais informações sobre os tipos de dados primitivos, basta consultar a [documentação oficial](https://docs.microsoft.com/pt-br/office/vba/language/reference/user-interface-help/data-type-summary).

## Tipos de dados primitivos mais utilizados

```vb
Long   ' Número inteiro de 32 bits (-2147483648 até 2147483647)
Double ' Número real de 64 bits (precisão de aprox. 15 casas decimais)
String ' Texto
Date   ' Data e hora
```

## Declaração de variáveis

```vb
Dim nome As String
Dim preco As Double
Dim idade As Long, altura As Double
```

## Atribuição de valores às variáveis

```vb
nome = "Valor de texto"
preco = 4.5
idade = 30
altura = 1.8

' Cuidado!!!! Apesar de não fazer diferença, matematicamente falando,
' x = 9 e 9 = x são coisas diferentes em linguagem de programação!
'
' O operador = significa algo como "armazenar em", de modo que x = 9
' poderia ser traduzido como "armazene 9 em x".
'
' Logo, não é possível executar o comando 9 = x como uma atribuição,
' e, de fato, um erro ocorrerá se você tentar armazenar um valor
' dessa forma.
```

## Operadores aritméticos

```vb
+   ' Soma
-   ' Subtração
*   ' Multiplicação
/   ' Divisão
\   ' Divisão inteira
^   ' Potenciação
Mod ' Resto de divisão
```

Alguns exemplos de uso de operadores aritméticos:

```vb
Dim x As Long, y As Long, d1 As Double, d2 As Double

x = 2
y = (4 + x) * 5 ' y valerá 30

x = 999
y = x / 100 ' y valerá 10 (arredondamento)

x = 940
y = x / 100 ' y valerá 9 (arredondamento)

x = 999
y = x \ 100 ' y valerá 9 (divisão inteira)

x = 940
y = x \ 100 ' y valerá 9 (divisão inteira)

x = 14
y = x Mod 5 ' y valerá 4 (resto da divisão inteira de 14 por 5)

d1 = 4.5 ' Sempre utilizamos . como separador decimal no código
d2 = d1 * 2 ' d2 valerá 9

d1 = 999
d2 = d1 / 100 ' d2 valerá 9.99

d1 = 940
d2 = d1 / 100 ' d2 valerá 9.4

d1 = 999
d2 = d1 \ 100 ' d2 valerá 9 (divisão inteira)

d1 = 940
d2 = d1 \ 100 ' d2 valerá 9 (divisão inteira)
```

## Concatenação de texto

```vb
Dim s As String, x As Long, d As Double

x = 9
d = 4.5
s = "Tenho duas variáveis: uma com valor " & x & " e outro com valor " & d

' s valerá o texto abaixo:
' Tenho duas variáveis: uma com valor 9 e outra com valor 4,5
'
' Repare que, mesmo sendo obrigatório o uso de . como o separador decimal
' ao longo do código, o separador mostrado no texto é , e não .
'
' Isso ocorre porque estamos no Brasil, com o Windows e o Excel em Português.
' Assim, espera-se que os usuários utilizassem , e não .
```

## Entrada e saída básica de dados

```vb
Dim s As String

' Entrada básica
s = InputBox("Mensagem", "Título", "Valor Inicial")

' Saída básica
MsgBox "Mensagem", vbOKOnly, "Título"

' 1 - Repare como InputBox utiliza () mas MsgBox não utiliza. Essa é uma
' característica da linguagem Visual Basic. Para apenas executar um procedimento
' ou uma função, não utilizamos parênteses. Porém, se estivermos interessados
' em obter o valor de retorno, armazenando-o em uma variável, devemos utilizar!
'
' 2 - A palavra vbOKOnly é uma constante definida pelo VBA. Para saber quais são
' os outros valores que poderiam ser utilizados no lugar do vbOKOnly, basta teclar
' Ctrl + Espaço, que o editor exibirá as opções em uma lista.
'
' 3 - InputBox sempre retorna textos, mesmo se o usuário digitasse um valor
' somente com dígitos. Na sequência veremos como converter textos em números e
' vice-versa.
```

## Convertendo texto em números

```vb
Dim s As String, x As Long, d As Double
    
s = "4,6" ' s contém três caracteres: 4,6... Não é possível realizar operações aritméticas!
x = CLng(s) ' x valerá 5 (arredondamento)
d = CDbl(s) ' d valerá 4.6
```

> Para mais informações sobre conversões entre tipos de dados, basta consultar a [documentação oficial](https://docs.microsoft.com/pt-br/office/vba/language/concepts/getting-started/type-conversion-functions).

## Convertendo números em texto

```vb
Dim s As String, x As Long, d As Double
    
x = 9
s = CStr(x) ' s valerá "9"

d = 4.6
s = CStr(d) ' s valerá "4,6"
s = Format(d, "0.00") ' s valerá "4,60"
s = Format(d, "000.00") ' s valerá "004,60"
d = 45.6
s = Format(d, "000.00") ' s valerá "045,60"
d = 452.6
s = Format(d, "000.00") ' s valerá "452,60"
d = 4521.6
s = Format(d, "000.00") ' s valerá "4521,60"
d = 4.6
s = Format(d, "#,##0.00") ' s valerá "4,60"
d = 45.6
s = Format(d, "#,##0.00") ' s valerá "45,60"
d = 452.6
s = Format(d, "#,##0.00") ' s valerá "452,60"
d = 4521.6
s = Format(d, "#,##0.00") ' s valerá "4.521,60"

' Repare que o Format oferece mais opções do que CStr. Para especificar formatos com o
' Format, sempre se utiliza . como separador decimal e , como separador de milhar, 0
' como um dígito obrigatório e # como um dígito opcional.
```

> Para mais informações sobre os detalhes de Format, basta consultar a [documentação oficial](https://docs.microsoft.com/pt-br/office/vba/language/reference/user-interface-help/format-function-visual-basic-for-applications).

## Trabalhando com datas

```vb
Dim s As String, d As Date, x As Long

' A forma de escrever datas no código em Visual Basic é:
' #Mês/Dia/Ano Hora:Minuto:Segundo [AM ou PM]#
d = #5/14/1990 10:30:00 PM#
s = CStr(d) ' s valerá "14/05/1990 22:30:00", porque estamos com o Windows e o Excel em Português!
d = CDate(s) ' A operação oposta também é válida
s = Format(d, "dd/mmm/yyyy") ' s valerá "14/mai/1990"

x = Day(d) ' x valerá 14
x = Month(d) ' x valerá 5
x = Year(d) ' x valerá 1990
x = Hour(d) ' x valerá 22
x = Minute(d) ' x valerá 30
x = Second(d) ' x valerá 0
x = Weekday(d) ' x valerá 2, porque o dia era uma segunda-feira

d = Date ' d valerá a data atual (sem horário)
d = Time ' d valerá a hora atual (sem data)
d = Now ' d valerá a data e hora atuais

d = #5/14/1990 10:30:00 PM#
x = DateDiff("d", d, Now) ' x valerá a quantidade de dias entre d (inicial) e Now (final)
```

> Para mais informações sobre os detalhes de Format, basta consultar a [documentação oficial](https://docs.microsoft.com/pt-br/office/vba/language/reference/user-interface-help/format-function-visual-basic-for-applications).
>
> Para mais informações sobre os detalhes de DateDiff, basta consultar a [documentação oficial](https://docs.microsoft.com/pt-br/office/vba/language/reference/user-interface-help/datediff-function).

## Estruturas de tomada de decisão

As estruturas de tomada de decisão servem para fazer com que a execução do programa tome desvios em determinados momentos, alterando seu comportamento com base nos valores das variáveis.

Exemplo 1:

```vb
Dim x As Long, y As Long

x = ... ' Atribuindo um valor qualquer para x
y = ... ' Atribuindo um valor qualquer para y

If x > y Then

    ' O código aqui dentro só será executado se x for maior do que y!

End If
```

Exemplo 2:

```vb
Dim x As Long, y As Long

x = ... ' Atribuindo um valor qualquer para x
y = ... ' Atribuindo um valor qualquer para y

If x > y Then

    ' O código aqui dentro só será executado se x for maior do que y!

Else

    ' O código aqui dentro só será executado se o código acima não for executado!

End If
```

Exemplo 3:

```vb
Dim x As Long

x = ... ' Atribuindo um valor qualquer para x

If x > 8 Then

    ' O código aqui dentro só será executado se x for maior do que 8!

ElseIf x > 5 Then

    ' O código aqui dentro só será executado se o código acima não for executado, mas x for maior do que 5!

Else

    ' O código aqui dentro só será executado se nenhum dos códigos acima forem executados!

End If
```

## Operadores booleanos

```vb
=   ' Igual
<>  ' Diferente
>   ' Maior
<   ' Menor
>=  ' Maior ou igual
<=  ' Menor ou igual
And ' Combina dois resultados booleanos utilizando a lógica E
Or  ' Combina dois resultados booleanos utilizando a lógica OU
```

## Estruturas de repetição

As estruturas de repetição servem para fazer com que a execução do programa repita mais de uma vez, sem que seja necessário efetivamente copiar e colar um trecho de código várias vezes.

Exemplo 1:

```vb
Dim x As Long, y As Long

x = ... ' Atribuindo um valor qualquer para x
y = ... ' Atribuindo um valor qualquer para y

While x <= y

    ' O código aqui dentro será executado enquanto x for menor ou igual à y!

    x = x + 1 ' Incrementa o valor de x

Wend
```

Exemplo 2:

```vb
Dim x As Long, y As Long

y = ... ' Atribuindo um valor qualquer para y

For x = ... To y ' O valor inicial de x é definido aqui!

    ' O código aqui dentro será executado enquanto x for menor ou igual à y!
    ' Diferente do While, aqui, x é incrementado automaticamente de 1 em 1.

Next
```

Exemplo 3:

```vb
Dim x As Long, y As Long

y = ... ' Atribuindo um valor qualquer para y

For x = ... To y Step 4 ' O valor inicial de x é definido aqui!

    ' O código aqui dentro será executado enquanto x for menor ou igual à y!
    ' Diferente do While, aqui, x é incrementado automaticamente de 4 em 4.

Next
```

## Acessando valores das células da planilha atual

```vb
Dim x As Long
Dim y As Double
Dim z As String

x = ActiveSheet.Cells(1, 1).Value ' Armazena o conteúdo de A1 em x, como um número inteiro
y = ActiveSheet.Cells(2, 1).Value ' Armazena o conteúdo de A2 em y, como um número real
z = ActiveSheet.Cells(3, 1).Value ' Armazena o conteúdo de A3 em z, como texto

' Durante a leitura, ocorrerá um erro se o dado da célula não puder ser convertido para o tipo desejado.
' Isso não ocorre durante a escrita, pois as células aceitam qualquer tipo de valor!

ActiveSheet.Cells(1, 2).Value = x ' Armazena o valor da variável x em B1
ActiveSheet.Cells(2, 2).Value = y ' Armazena o valor da variável y em B2
ActiveSheet.Cells(3, 2).Value = z ' Armazena o valor da variável z em B3
```

> O objeto `ActiveSheet` pode ser omitido se o código estiver sendo executado na própria planilha e não em um módulo.
>
> Durante a leitura, se existir dúvida quanto ao tipo de dados presente nas células, é recomendável utilizar as funções de conversão mostradas anteriormente, como `CLng()` ou `CDbl()`.

## Acessando valores das células de outras planilhas

```vb
Dim x As Long
Dim y As Double
Dim z As String

x = Sheets("Gastos").Cells(1, 1).Value ' Armazena o conteúdo de A1, da planilha Gastos, em x, como um número inteiro
y = Sheets("Gastos").Cells(2, 1).Value ' Armazena o conteúdo de A2, da planilha Gastos, em y, como um número real
z = Sheets("Gastos").Cells(3, 1).Value ' Armazena o conteúdo de A3, da planilha Gastos, em z, como texto

' Durante a leitura, ocorrerá um erro se o dado da célula não puder ser convertido para o tipo desejado.
' Isso não ocorre durante a escrita, pois as células aceitam qualquer tipo de valor!

Sheets("Planilha 1").Cells(1, 2).Value = x ' Armazena o valor da variável x em B1, da planilha Planilha 1
Sheets("Planilha 1").Cells(2, 2).Value = y ' Armazena o valor da variável y em B2, da planilha Planilha 1
Sheets("Planilha 1").Cells(3, 2).Value = z ' Armazena o valor da variável z em B3, da planilha Planilha 1
```

O nome correto da planilha pode ser obtido na janela "Projeto", como mostrado abaixo:

![Janela Projeto do VBA](https://raw.githubusercontent.com/tech-espm/misc-notes/master/assets/images/projeto-vba.png)

Para evitar o uso constante da forma `Sheets("Nome")`, convém armazenar a planilha em uma variável, como no exemplo abaixo, que reescreve o exemplo anterior, utilizando variáveis do tipo `Worksheet`:

```vb
Dim x As Long
Dim y As Double
Dim z As String

Dim gastos As Worksheet
Set gastos = Sheets("Gastos")

Dim planilha1 As Worksheet
Set planilha1 = Sheets("Planilha 1")

x = gastos.Cells(1, 1).Value ' Armazena o conteúdo de A1, da planilha Gastos, em x, como um número inteiro
y = gastos.Cells(2, 1).Value ' Armazena o conteúdo de A2, da planilha Gastos, em y, como um número real
z = gastos.Cells(3, 1).Value ' Armazena o conteúdo de A3, da planilha Gastos, em z, como texto

' Durante a leitura, ocorrerá um erro se o dado da célula não puder ser convertido para o tipo desejado.
' Isso não ocorre durante a escrita, pois as células aceitam qualquer tipo de valor!

planilha1.Cells(1, 2).Value = x ' Armazena o valor da variável x em B1, da planilha Planilha 1
planilha1.Cells(2, 2).Value = y ' Armazena o valor da variável y em B2, da planilha Planilha 1
planilha1.Cells(3, 2).Value = z ' Armazena o valor da variável z em B3, da planilha Planilha 1
```
