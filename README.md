# jogo_da_velha
import 'package:flutter/material.dart';

void main() {
  runApp(MeuApp());
}

class MeuApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Jogo da Velha Flutter',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MinhaPaginaInicial(),
    );
  }
}

class MinhaPaginaInicial extends StatefulWidget {
  @override
  _MinhaPaginaInicialState createState() => _MinhaPaginaInicialState();
}

class _MinhaPaginaInicialState extends State<MinhaPaginaInicial> {
  List<String> grid = List.generate(9, (index) => '');
  String jogadorAtual = 'X';
  bool jogoFinalizado = false;
  String resultado = '';

  void verificarVencedor() {
    List<List<int>> vitorias = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8],
      [0, 3, 6], [1, 4, 7], [2, 5, 8],
      [0, 4, 8], [2, 4, 6]
    ];

    for (var combo in vitorias) {
      if (grid[combo[0]] != '' &&
          grid[combo[0]] == grid[combo[1]] &&
          grid[combo[1]] == grid[combo[2]]) {
        jogoFinalizado = true;
        resultado = '${grid[combo[0]]} venceu!';
      }
    }

    if (!grid.contains('') && resultado == '') {
      jogoFinalizado = true;
      resultado = 'Empate!';
    }
  }

  void resetarJogo() {
    setState(() {
      grid = List.generate(9, (index) => '');
      jogadorAtual = 'X';
      jogoFinalizado = false;
      resultado = '';
    });
  }

  void jogar(int index) {
    if (!jogoFinalizado && grid[index] == '') {
      setState(() {
        grid[index] = jogadorAtual;
        jogadorAtual = jogadorAtual == 'X' ? 'O' : 'X';
        verificarVencedor();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Jogo da Velha'),
        actions: [
          IconButton(
            icon: Icon(Icons.refresh),
            onPressed: resetarJogo,
          ),
        ],
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Text(resultado, style: TextStyle(fontSize: 24)),
          GridView.builder(
            shrinkWrap: true,
            gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 3,
            ),
            itemCount: 9,
            itemBuilder: (context, index) {
              return GestureDetector(
                onTap: () => jogar(index),
                child: Container(
                  decoration: BoxDecoration(
                    border: Border.all(color: Colors.black),
                  ),
                  child: Center(
                    child: Text(
                      grid[index],
                      style: TextStyle(fontSize: 48),
                    ),
                  ),
                ),
              );
            },
          ),
        ],
      ),
    );
  }
}
