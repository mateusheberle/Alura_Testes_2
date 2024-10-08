## Teste de integração

1 - Baixar dependência no `pubspec.yaml`
    
    arquivo:    `pubspec.yaml`

```yaml
dev_dependencies:
  integration_test:
    sdk: flutter
```

2 - Criar nova pasta fora da /lib
    
    arquivo:    `integration_test/app_test.dart`

```dart
import 'package:client_control/models/clients.dart';
import 'package:client_control/models/types.dart';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'package:client_control/main.dart' as app;
import 'package:provider/provider.dart';

void main() {
  // garantir que o aplicativo está rodando antes do teste começar
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  testWidgets('Integration Test', (tester) async {
    final providerKey = GlobalKey();

    app.main(list: [], providerKey: providerKey);
    await tester.pumpAndSettle(); // esperar os micro-processos acabarem

    // 1 - Testando tela inicial
    expect(find.text('Clientes'), findsOneWidget);
    expect(find.byIcon(Icons.menu), findsOneWidget);
    expect(find.byType(FloatingActionButton), findsOneWidget);

    // 2 - Testando o drawer
    await tester.tap(find.byIcon(Icons.menu));
    await tester.pumpAndSettle();
    expect(find.text('Menu'), findsOneWidget);
    expect(find.text('Gerenciar clientes'), findsOneWidget);
    expect(find.text('Tipos de clientes'), findsOneWidget);
    expect(find.text('Sair'), findsOneWidget);

    // 3 - Testar tela de navegação e a Tela de Tipos
    await tester.tap(find.text('Tipos de clientes'));
    await tester.pumpAndSettle();
    expect(find.text('Tipos de cliente'), findsOneWidget);
    expect(find.byType(FloatingActionButton), findsOneWidget);
    expect(find.byIcon(Icons.menu), findsOneWidget);
    expect(find.text('Platinum'), findsOneWidget);
    expect(find.text('Golden'), findsOneWidget);
    expect(find.text('Titanium'), findsOneWidget);
    expect(find.text('Diamond'), findsOneWidget);

    // 4 - Testar a criação de um Tipo de Cliente
    await tester.tap(find.byType(FloatingActionButton));
    await tester.pumpAndSettle();
    expect(find.byType(AlertDialog), findsOneWidget);
    await tester.enterText(find.byType(TextFormField), 'Ferro');
    await tester.tap(find.text('Selecionar icone'));
    await tester.pumpAndSettle();
    await tester.tap(find.byIcon(Icons.card_giftcard));
    await tester.pumpAndSettle();
    await tester.tap(find.text('Salvar'));
    await tester.pumpAndSettle();
    expect(find.text('Ferro'), findsOneWidget);
    expect(find.byIcon(Icons.card_giftcard), findsOneWidget);

    expect(
        Provider.of<Types>(providerKey.currentContext!, listen: false)
            .types
            .last
            .name,
        'Ferro');
    expect(
        Provider.of<Types>(providerKey.currentContext!, listen: false)
            .types
            .last
            .icon,
        Icons.card_giftcard);

    // 5 - Testando novo cliente
    await tester.tap(find.byIcon(Icons.menu));
    await tester.pumpAndSettle();
    await tester.tap(find.text('Gerenciar clientes'));
    await tester.pumpAndSettle();
    await tester.tap(find.byType(FloatingActionButton));
    await tester.pumpAndSettle();
    await tester.enterText(find.byKey(const Key('NameKey1')), 'DandaraBot');
    await tester.enterText(
        find.byKey(const Key('EmailKey1')), 'dandara@gmail.com');

    await tester.tap(find.byIcon(Icons.arrow_downward));
    await tester.pumpAndSettle();
    await tester.tap(find.text('Ferro').last); // clicar no último 'Ferro'
    await tester.pumpAndSettle();
    await tester.tap(find.text('Salvar'));
    await tester.pumpAndSettle();

    // 6 - Verificando se o cliente apareceu devidamente
    expect(find.text('DandaraBot (Ferro)'), findsOneWidget);
    expect(find.byIcon(Icons.card_giftcard), findsOneWidget);
    expect(
        Provider.of<Clients>(providerKey.currentContext!, listen: false)
            .clients
            .last
            .name,
        'DandaraBot');
    expect(
        Provider.of<Clients>(providerKey.currentContext!, listen: false)
            .clients
            .last
            .email,
        'dandara@gmail.com');
  });

  // testWidgets('Integration Test', (tester) async {
  //   app.main();
  //   await tester.pumpAndSettle(); // esperar os micro-processos acabarem
  //   expect(find.text('Menu'), findsNothing);

  //   await tester.tap(find.byIcon(Icons.menu));
  //   await tester.pumpAndSettle();
  //   expect(find.text('Menu'), findsOneWidget);
  // });
}

```