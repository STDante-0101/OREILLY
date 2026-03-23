# 🎮 Capítulo 1 - MatchGame (Jogo da Memória)

## 📝 Descrição

Jogo da memória desenvolvido em WPF com 20 cartas (10 pares de animais em emojis). O jogador deve clicar nas cartas para encontrar os pares correspondentes, com um cronômetro registrando o tempo.

## 🎯 Objetivos de Aprendizado

Este projeto foi criado para ensinar os seguintes conceitos:

### 1. **WPF (Windows Presentation Foundation)**
- Criação de interfaces gráficas com XAML
- Separação entre UI (XAML) e lógica (C#)
- Data binding e eventos

### 2. **Grid Layout**
- Estruturação de layouts com `Grid.RowDefinitions` e `Grid.ColumnDefinitions`
- Posicionamento de elementos com `Grid.Row` e `Grid.Column`
- Uso de `Grid.ColumnSpan` para elementos que ocupam múltiplas colunas

### 3. **Event Handlers**
- Manipulação de eventos `MouseDown`
- Delegates e métodos de callback
- Controle de estado da aplicação

### 4. **Collections em C#**
- Uso de `List<T>` para armazenar dados
- Métodos `Add`, `RemoveAt`, `Count`
- Iteração com `foreach`

### 5. **DispatcherTimer**
- Criação de cronômetros em aplicações WPF
- Configuração de intervalos com `TimeSpan`
- Eventos `Tick` para atualização periódica

### 6. **Classe Random**
- Geração de números aleatórios
- Uso de `Random.Next()` para embaralhamento

## 🐛 Problemas Encontrados e Soluções

### **Bug 1: ArgumentOutOfRangeException**

**Problema:**
```
Exception: System.ArgumentOutOfRangeException
Message: Index was out of range. Must be non-negative and less than the size of the collection.
Line: string nextEmoji = animalEmoj[index];
```

**Causa Raiz:**
- Grid tinha **21 TextBlocks** (20 do jogo + 1 `timeTextBlock`)
- Lista `animalEmoj` tinha apenas **20 emojis**
- O loop `foreach` tentava atribuir emojis a todos os TextBlocks

**Solução Aplicada:**
```csharp
foreach (TextBlock textBlock in mainGrid.Children.OfType<TextBlock>())
{
    if (textBlock.Name == "timeTextBlock")
        continue;  // Ignora o TextBlock do timer
    
    int index = aleatorio.Next(animalEmoj.Count);
    string nextEmoji = animalEmoj[index];
    textBlock.Text = nextEmoji;
    animalEmoj.RemoveAt(index);
}
```

### **Bug 2: Timer Sobrepondo os Animais**

**Problema:**
O `timeTextBlock` estava configurado para `Grid.Row="5"`, mas o Grid tinha apenas 5 linhas (índices 0-4), causando sobreposição visual.

**Solução Aplicada:**
Adicionei uma 6ª linha ao Grid:
```xaml
<Grid.RowDefinitions>
    <RowDefinition/>  <!-- 0 -->
    <RowDefinition/>  <!-- 1 -->
    <RowDefinition/>  <!-- 2 -->
    <RowDefinition/>  <!-- 3 -->
    <RowDefinition/>  <!-- 4 -->
    <RowDefinition/>  <!-- 5 - NOVA! -->
</Grid.RowDefinitions>
```

## 🏗️ Estrutura do Projeto

```
Cap01-MatchGame/
├── MatchGame.sln              # Solution do Visual Studio
├── MatchGame/
│   ├── App.xaml               # Configuração da aplicação
│   ├── App.xaml.cs            # Code-behind da aplicação
│   ├── MainWindow.xaml        # Interface principal (XAML)
│   ├── MainWindow.xaml.cs     # Lógica principal (C#)
│   ├── AssemblyInfo.cs        # Metadados do assembly
│   └── MatchGame.csproj       # Arquivo do projeto
└── README.md                  # Este arquivo
```

## 🎨 Layout Visual

```
┌─────────┬─────────┬─────────┬─────────┐
│   🐙    │   🦖    │   🐩    │   🐐    │  Row 0
├─────────┼─────────┼─────────┼─────────┤
│   🦍    │   🦒    │   🐲    │   🦬    │  Row 1
├─────────┼─────────┼─────────┼─────────┤
│   🦈    │   🐖    │   🐙    │   🦖    │  Row 2
├─────────┼─────────┼─────────┼─────────┤
│   🐩    │   🐐    │   🦍    │   🦒    │  Row 3
├─────────┼─────────┼─────────┼─────────┤
│   🐲    │   🦬    │   🦈    │   🐖    │  Row 4
├─────────┴─────────┴─────────┴─────────┤
│         Elapsed time: 14.3s           │  Row 5
└─────────────────────────────────────────┘
```

## 🚀 Como Executar

### Pré-requisitos:
- Visual Studio 2022 ou superior
- .NET 8 SDK
- Windows 10 ou superior

### Passos:
1. Abra o arquivo `MatchGame.sln` no Visual Studio
2. Pressione `F5` para compilar e executar
3. Clique nas cartas para encontrar os pares

## 📊 Conceitos-Chave de C#

### **Collections**
```csharp
List<string> animalEmoj = new List<string>() { "🐙", "🐙", ... };
animalEmoj.RemoveAt(index);  // Remove item em um índice específico
```

### **LINQ**
```csharp
mainGrid.Children.OfType<TextBlock>()  // Filtra apenas TextBlocks
```

### **Events**
```csharp
private void TextBlock_MouseDown(object sender, MouseButtonEventArgs e)
{
    TextBlock textBlock = sender as TextBlock;
    // Lógica do evento
}
```

### **Timers**
```csharp
DispatcherTimer timer = new DispatcherTimer();
timer.Interval = TimeSpan.FromSeconds(0.1);
timer.Tick += Timer_Tick;
```

## 💡 Melhorias Futuras (Desafios)

- [ ] Implementar contador de tentativas
- [ ] Adicionar níveis de dificuldade
- [ ] Criar animações de flip nas cartas
- [ ] Adicionar sons de feedback
- [ ] Persistir recordes (high scores)
- [ ] Modo multiplayer local

## 📚 Recursos Adicionais

- [Documentação WPF - Microsoft](https://docs.microsoft.com/pt-br/dotnet/desktop/wpf/)
- [Grid Layout Guide](https://docs.microsoft.com/pt-br/dotnet/desktop/wpf/controls/grid)
- [Event Handling in WPF](https://docs.microsoft.com/pt-br/dotnet/desktop/wpf/events/)

---

**Capítulo do Livro:** 1  
**Tempo de Desenvolvimento:** ~2 horas  
**Dificuldade:** ⭐⭐☆☆☆ (Iniciante)  
**Status:** ✅ Concluído