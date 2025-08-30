# Clean Code em Dart/Flutter
# Por que usar Clean Code?

Clean Code significa escrever código claro, legível e fácil de manter. Aplicar esses princípios traz diversos benefícios:

- Facilita a colaboração entre desenvolvedores.
- Reduz o tempo para encontrar e corrigir bugs.
- Torna o onboarding de novos membros mais rápido.
- Diminui o custo de manutenção do projeto.
- Permite evoluir o sistema com menos retrabalho.

### Impacto na produtividade

Quando o código é limpo, o tempo gasto para implementar novas funcionalidades, corrigir erros e dar manutenção diminui drasticamente.

No início, código sujo parece mais rápido de implementar, mas com o tempo, o esforço para manter e evoluir cresce exponencialmente. Já o Clean Code mantém o ritmo de produtividade alto e constante.

**Resumo:**
Investir em Clean Code é investir em produtividade, qualidade e longevidade do projeto.

Documentação adaptada do livro Clean Code (Uncle Bob) para o contexto de Dart/Flutter.

## 1. Nomes Significativos
Use nomes claros e descritivos para variáveis, funções, classes e arquivos.
Evite abreviações e nomes genéricos.

**Exemplo ruim:**
```dart
var dist;
void clcDist() {}
class Usr {}
```

**Exemplo bom:**
```dart
var distancia;
void calcularDistancia() {}
class Usuario {}
```

## 2. Funções Pequenas
Funções devem fazer uma única coisa.
Prefira funções curtas e com poucos parâmetros.

**Exemplo ruim:**
```dart
void processarDados() {
  buscarDados();
  analisarDados();
  salvarDados();
}
```

**Exemplo bom:**
```dart
void buscarDados() {}
void analisarDados() {}
void salvarDados() {}
```

## 3. Evite Comentários Desnecessários
Código claro dispensa comentários.
Use comentários apenas para regras de negócio complexas.

**Exemplo ruim:**
```dart
// Atualiza o saldo
saldo = saldo + valor;
```

**Exemplo bom:**
```dart
saldo = atualizarSaldo(saldo, transacao);
```

## 4. Formatação e Organização
Mantenha o código bem indentado e organizado.
Separe widgets, controllers e services em arquivos próprios.

**Exemplo ruim:**
```dart
class MeuApp extends StatelessWidget {
Widget build(BuildContext context) {
return Scaffold(body:Center(child:Text('Olá')));}}
```

**Exemplo bom:**
```dart
class MeuApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('Olá'),
      ),
    );
  }
}
```

## 5. Objetos e Estruturas de Dados
- Use classes para encapsular dados e comportamentos.
- Prefira encapsulamento e imutabilidade quando possível.
- Exemplo:
```dart
class User {
  final String name;
  User(this.name);
}
```

## 6. Tratamento de Erros
- Use try/catch para tratar exceções.
- Não ignore erros silenciosamente.
- Exemplo:
```dart
try {
  await api.getData();
} catch (e) {
  print('Erro ao buscar dados: $e');
}
```

## 7. Testes
- Escreva testes automatizados para garantir qualidade.
- Use `test` para unitários e `flutter_test` para widgets.
- Exemplo:
```dart
test('deve somar corretamente', () {
  expect(somar(2, 2), 4);
});
```

## 8. Evite Duplicação
Reutilize código com funções e widgets.
DRY: Don’t Repeat Yourself.

**Exemplo ruim:**
```dart
ElevatedButton(onPressed: () {}, child: Text('Salvar'));
ElevatedButton(onPressed: () {}, child: Text('Cancelar'));
ElevatedButton(onPressed: () {}, child: Text('Excluir'));
```

**Exemplo bom:**
```dart
Widget customButton(String text) {
  return ElevatedButton(onPressed: () {}, child: Text(text));
}

customButton('Salvar');
customButton('Cancelar');
customButton('Excluir');
```

## 9. Classes e Métodos
Classes devem ser coesas e métodos bem definidos.
Veja exemplos práticos de cada princípio SOLID em Dart:

### S - Single Responsibility Principle (Responsabilidade Única)
Cada classe deve ter uma única responsabilidade.
```dart
// Ruim: mistura lógica de autenticação e dados
class Usuario {
  void login(String usuario, String senha) {}
  void salvarDados() {}
}

// Bom: cada classe tem uma responsabilidade
class AuthService {
  void login(String usuario, String senha) {}
}

class UsuarioRepository {
  void salvarDados(Usuario usuario) {}
}
```

### O - Open/Closed Principle (Aberto/Fechado)
Classes devem ser abertas para extensão, mas fechadas para modificação.
```dart
// Ruim: precisa modificar para adicionar novo tipo de pagamento
class Pagamento {
  void pagar(String tipo, double valor) {
    if (tipo == 'pix') {
      // paga com pix
    } else if (tipo == 'boleto') {
      // paga com boleto
    }
  }
}

// Bom: basta criar uma nova classe
abstract class Pagamento {
  void pagar(double valor);
}

class PagamentoPix implements Pagamento {
  @override
  void pagar(double valor) {
    // paga com pix
  }
}

class PagamentoBoleto implements Pagamento {
  @override
  void pagar(double valor) {
    // paga com boleto
  }
}
```

### L - Liskov Substitution Principle (Substituição de Liskov)
Subtipos devem poder substituir seus tipos base sem alterar o funcionamento.
```dart
// Ruim: subclasse altera comportamento esperado
class Forma {
  double area() => 0;
}
class Quadrado extends Forma {
  double lado;
  Quadrado(this.lado);
  @override
  double area() => lado * lado;
}
class Circulo extends Forma {
  double raio;
  Circulo(this.raio);
  @override
  double area() => 3.14 * raio * raio;
}
// Ambas podem ser usadas como Forma sem quebrar o código
```

### I - Interface Segregation Principle (Segregação de Interface)
Prefira interfaces pequenas e específicas.
```dart
// Ruim: interface genérica demais
abstract class Animal {
  void voar();
  void nadar();
}

// Bom: interfaces específicas
abstract class Voador {
  void voar();
}
abstract class Nadador {
  void nadar();
}
class Pato implements Voador, Nadador {
  @override
  void voar() {}
  @override
  void nadar() {}
}
```

### D - Dependency Inversion Principle (Inversão de Dependência)
Dependa de abstrações, não de implementações concretas.
```dart
// Ruim: depende diretamente de uma classe concreta
class Pedido {
  final PagamentoPix pagamento;
  Pedido(this.pagamento);
}

// Bom: depende de uma abstração
class Pedido {
  final Pagamento pagamento;
  Pedido(this.pagamento);
}
```

## 10. Código Limpo em Flutter
- Organize o projeto em camadas: UI, lógica, dados.
- Separe responsabilidades.
- Exemplo:
```dart
// lib/pages/login_page.dart
// lib/services/auth_service.dart
```

## 11. Refatoração
- Refatore sempre que possível para melhorar o código.
- Use ferramentas do Dart/Flutter para refatorar.

## 12. Conclusão
Clean Code facilita manutenção, colaboração e evolução do projeto.
Pratique sempre e revise o código regularmente.

---

## Tópicos Extras do Livro Clean Code

### Código Limpo vs. Código Sujo
**Código sujo:**
```dart
void a() {
  int x = 1;
  int y = 2;
  int z = x + y;
  print(z);
}
```
**Código limpo:**
```dart
void somarEDisplay(int primeiro, int segundo) {
  final resultado = primeiro + segundo;
  print(resultado);
}
```

### Eliminação de Código Morto
Remova funções, variáveis e comentários que não são mais usados.
**Resumo:**
Remova tudo que não é mais usado: funções, variáveis e comentários antigos. Isso deixa o código mais limpo e fácil de manter.

### Evitar Dependências Desnecessárias
Prefira código desacoplado e fácil de testar.
```dart
// Ruim
class Pedido {
  final PagamentoPix pagamento;
  Pedido(this.pagamento);
}

// Bom
class Pedido {
  final Pagamento pagamento;
  Pedido(this.pagamento);
}
```

### Refatoração Contínua
Melhore o código sempre que possível, sem alterar o comportamento.
```dart
// Bom
void processarTudo() {
  var dados = buscarDados();
  var resultado = analisarDados(dados);
  salvarDados(resultado);
}

List<int> buscarDados() {
  // Busca dados de uma fonte
  return [12, 5, 8];
}

List<int> analisarDados(List<int> dados) {
  // Aplica regra de negócio nos dados
  return dados.map((item) => item > 10 ? item * 2 : item + 5).toList();
}

void salvarDados(List<int> resultado) {
  // Salva ou exibe o resultado
  print(resultado);
}
  }
}

// Depois
void processar() {
  var dados = buscarDados();
  var resultado = analisarDados(dados);
  salvarResultado(resultado);
}

List<int> analisarDados(List<int> dados) {
  return dados.map((item) => item > 10 ? item * 2 : item + 5).toList();
}

void salvarResultado(List<int> resultado) {
  print(resultado);
}
```

### Funções Longas e Complexas
Divida funções grandes em partes menores e mais legíveis.
```dart
// Ruim
void processarTudo() {
  var dados = buscarDados();
  for (var i = 0; i < dados.length; i++) {
    if (dados[i] > 10) {
      dados[i] = dados[i] * 2;
    } else {
      dados[i] = dados[i] + 5;
    }
    print('Processando item: ${dados[i]}');
    if (dados[i] % 2 == 0) {
      salvarResultado(dados[i]);
    }
  }
  print('Processamento finalizado');
}

// Bom
List<int> buscarDados() {
  return [12, 5, 8];
}

List<int> analisarDados(List<int> dados) {
  return dados.map((item) => item > 10 ? item * 2 : item + 5).toList();
}

void salvarDados(List<int> resultado) {
  print(resultado);
}
```

### Testes Automatizados
Escreva testes para garantir que o código funciona e pode ser alterado com segurança.
```dart
test('deve calcular corretamente', () {
  expect(calcularNovo(), 42);
});
```

### Cuidado com Comentários
Prefira código autoexplicativo. Use comentários apenas para regras de negócio ou explicações realmente necessárias.
```dart
// Bom
// Calcula o saldo conforme regra bancária
saldo = atualizarSaldo(saldo, transacao);
```

### Nomes de Funções e Variáveis
Devem revelar intenção e facilitar entendimento.
```dart
// Ruim
void calMdAln() {}
// Bom
void calcularMediaAluno() {}
```

---

## Bônus: Ternário, If/Else e Early Return

### Uso excessivo de ternário
**Ruim:**
```dart
String resultado = condicao ? (outraCondicao ? 'A' : 'B') : (terceiraCondicao ? 'C' : 'D');
```

**Bom:**
```dart
String resultado;
if (condicao) {
  if (outraCondicao) {
    resultado = 'A';
  } else {
    resultado = 'B';
  }
} else if (terceiraCondicao) {
  resultado = 'C';
} else {
  resultado = 'D';
}
```

### Uso excessivo de if/else (sem early return)
**Ruim:**
```dart
String validarUsuario(String nome) {
  if (nome.isEmpty) {
    return 'Nome vazio';
  } else {
    if (nome.length < 3) {
      return 'Nome muito curto';
    } else {
      return 'Ok';
    }
  }
}
```

**Bom (early return):**
```dart
String validarUsuario(String nome) {
  if (nome.isEmpty) return 'Nome vazio';
  if (nome.length < 3) return 'Nome muito curto';
  return 'Ok';
}
```

---

**Referência:** Clean Code, Robert C. Martin (Uncle Bob)
