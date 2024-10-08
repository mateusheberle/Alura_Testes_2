## Teste de unidade

    arquivo:    test/unid_test.dart
    conteúdo:   teste 'Clients'

```dart
import 'package:client_control/models/client.dart';
import 'package:client_control/models/client_type.dart';
import 'package:client_control/models/clients.dart';
import 'package:client_control/models/types.dart';
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

void main() {

  group('Clients Test', () {
    
    final mateus = Client(
        name: 'Mateus',
        email: 'mateus@gmail.com',
        type: ClientType(name: 'Gold', icon: Icons.star));

    test('Clients model should add new client', () {
      var clients = Clients(clients: []);
      clients.add(mateus);
      clients.add(mateus);
      expect(clients.clients, [mateus, mateus]);
    });

    test('Clients model should remove old client', () {
      var clients = Clients(clients: [mateus, mateus, mateus]);
      clients.remove(0);
      clients.remove(1);
      expect(clients.clients, [mateus]);
    });
    
  });
```
---
    arquivo:    test/unid_test.dart
    conteúdo:   teste 'Types'

```dart
  group('Type Test', () {
    
    final gold = ClientType(name: 'Gold', icon: Icons.star);

    test('Type model should add new type', () {
      var types = Types(types: []);
      types.add(gold);
      types.add(gold);
      expect(types.types, [gold, gold]);
    });

    test('Types model should remove old type', () {
      var types = Types(types: [gold, gold, gold]);
      types.remove(0);
      types.remove(1);
      expect(types.types, [gold]);
    });
    
  });
}
```