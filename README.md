 //Exercicio-Checkpoint-Dart

import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: Home(),
    debugShowCheckedModeBanner: false,
  ));
}

class Home extends StatefulWidget {
  @override
  _HomeState createState() => _HomeState();
}

class _HomeState extends State<Home> {
  GlobalKey<FormState> _formKey = GlobalKey<FormState>();
  TextEditingController pesoController = TextEditingController();
  TextEditingController alturaController = TextEditingController();
  String _resultado = "Informe seus dados!";
  Color _corResultado = Colors.transparent;

  void _reset() {
    pesoController.text = "";
    alturaController.text = "";

    setState(() {
      _resultado = "Informe seus dados!";
      _corResultado = Colors.transparent;
      _formKey = GlobalKey<FormState>();
    });
  }

  void _calcularIMC() {
    setState(() {
      double peso = double.parse(pesoController.text.replaceAll(',', '.'));
      double altura =
          double.parse(alturaController.text.replaceAll(',', '.')) / 100;

      double imc = peso / (altura * altura);
      String resultado;
      Color cor;

      if (imc < 18.5) {
        resultado = "Abaixo do peso";
        cor = Colors.yellow;
      } else if (imc >= 18.5 && imc < 24.9) {
        resultado = "Peso normal";
        cor = Colors.green;
      } else if (imc >= 25 && imc < 29.9) {
        resultado = "Sobrepeso";
        cor = Colors.orange;
      } else if (imc >= 30 && imc < 34.9) {
        resultado = "Obesidade Grau 1";
        cor = Colors.purple[200]!;
      } else if (imc >= 35 && imc < 39.9) {
        resultado = "Obesidade Grau 2";
        cor = Colors.purple[500]!;
      } else {
        resultado = "Obesidade Grau 3";
        cor = Colors.purple[900]!;
      }

      _resultado = "IMC: ${imc.toStringAsFixed(1)}\n$resultado";
      _corResultado = cor;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Calculadora de IMC",
          style: TextStyle(color: Colors.white),
        ),
        centerTitle: true,
        backgroundColor: Colors.lightBlue[900],
        actions: <Widget>[
          IconButton(
              icon: Icon(Icons.refresh, color: Colors.white),
              onPressed: () {
                _reset();
              })
        ],
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.fromLTRB(10.0, 0, 10.0, 0),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: <Widget>[
              Icon(
                Icons.person,
                size: 150.0,
                color: Colors.lightBlue[900],
              ),
              TextFormField(
                controller: pesoController,
                textAlign: TextAlign.center,
                keyboardType: TextInputType.number,
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return "Informe o seu peso";
                  }
                  return null;
                },
                decoration: InputDecoration(
                    labelText: "Peso (kg)",
                    labelStyle: TextStyle(color: Colors.lightBlue[900])),
                style: TextStyle(color: Colors.lightBlue[900], fontSize: 26.0),
              ),
              TextFormField(
                controller: alturaController,
                textAlign: TextAlign.center,
                keyboardType: TextInputType.number,
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return "Informe a sua altura (cm)";
                  }
                  return null;
                },
                decoration: InputDecoration(
                    labelText: "Altura (cm)",
                    labelStyle: TextStyle(color: Colors.lightBlue[900])),
                style: TextStyle(color: Colors.lightBlue[900], fontSize: 26.0),
              ),
              Padding(
                  padding: EdgeInsets.only(top: 20.0, bottom: 20.0),
                  child: Container(
                      height: 50.0,
                      child: ElevatedButton(
                        child: Text("Calcular",
                            style:
                                TextStyle(color: Colors.white, fontSize: 26.0)),
                        style: ElevatedButton.styleFrom(
                            backgroundColor: Colors.lightBlue[900]),
                        onPressed: () {
                          if (_formKey.currentState!.validate()) {
                            _calcularIMC();
                          }
                        },
                      ))),
              Container(
                padding: EdgeInsets.all(16),
                color: _corResultado,
                child: Text(
                  _resultado,
                  textAlign: TextAlign.center,
                  style: TextStyle(color: Colors.white, fontSize: 26.0),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

