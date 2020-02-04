---
layout: post
title: Tutorial 1  - Flutter GIS - Como Adicionar Marcadores em um Mapa
subtitle: 
tags: [flutter, GIS, Tutorial]
---

Neste post mostrarei como desenvolver um aplicativo de SIG utilizando o Flutter. O aplicativo permite adicionar pontos em um mapa e obter suas coodernadas.

 
![](/img/post_flutter_markers/marker_demo.gif)



O Flutter é um SDK desenvolvido pela Google que permite o desenvolvimento de aplicativos para multiplataformas, isto é, com o mesmo
código é possível desenvolver aplicativos para android, IOS, web e desktop. Este framework ainda não é muito utilizado no mercado, porém,
a comunidade de desenvolvedores e bibliotecas diponíveis estão aumentando rapidamente, além disso, startups como a NuBank já utilizam
o Flutter para desenvolver seus aplicativos. O lançamento do Fuchsia, novo sistema operacional da Google, deve aumentar ainda mais o uso deste SDK,
já que os aplicativos serão desenvolvidos com base no Flutter. Portanto, a tendência é que o Flutter seja cada vez mais utilizado no mercado.

Para desenvolver este aplicativo é necessário que você já tenha o Flutter instalado em sua máquina e que saiba conceitos básicos da Liguagem Dart, utilizada pelo Flutter, e de sistema de informações geográficas (SIG). O Android Studio é a IDE utilizada neste tutorial.




### Workflow

- [0. Criar um novo projeto Flutter](#0-criar-um-novo-projeto-flutter) 

- [1. Adicionar as bibliotecas](#1-adicionar-as-bibliotecas) 

- [2. Importar as bibliotecas](#2-importar-as-bibliotecas) 

- [3. Desenvolver a UI](#3-desenvolver-a-UI) 

- [4. Adicionar o mapa](#4-adicionar-o-mapa) 

- [5. Adicionar os marcadores no mapa](#5-adicionar-os-marcadores-no-mapa) 

- [6. Mostrar a SnackBar com as coordenadas](#6-mostrar-snackbar-com-as-coordenadas) 

- [*Código Completo*](#código-completo)

&nbsp;

#### 0. Criar um novo projeto flutter

Abra o Android Studio e crie um novo projeto do Flutter, escolha o nome e o local em qual o projeto será salvo.

![](/img/post_flutter_markers/new_project.png)


#### 1. Adicionar as bibliotecas

Utilizaremos as bibliotecas flutter_map e latlong:

- [Flutter_map](<https://pub.dev/packages/flutter_map>)

- [latlong](<https://pub.dev/packages/latlong>)


Vá ao arquivo *pubspec.yaml* e adicione as seguintes bibliotecas como dependências:

```flutter
flutter_map: ^0.8.2
latlong: ^0.6.1
```

![](/img/post_flutter_markers/pubspec_yaml.png)


#### 2. Importar as bibliotecas

No arquivo *main.dart* iremos importar as bibliotecas que serão utilizadas no aplicativo, assim, na parte superior, adicione as seguintes linhas de código:


```flutter
// Importar bibliotecas 
import 'package:flutter_map/flutter_map.dart';
import 'package:latlong/latlong.dart';
```


![](/img/post_flutter_markers/importar_bibliotecas.png)


#### 3. Desenvolver a UI

Agora iremos desenvolver a interface do usuário, utilizaremos o [scaffold](<https://api.flutter.dev/flutter/material/Scaffold-class.html>) como o widget base deste aplicativo.

Apague tudo o que está escrito abaixo das bibliotecas no arquivo *main.dart* e reescreva seguindo o código abaixo:


```flutter
import 'package:flutter/material.dart';

// Importar bibliotecas
import 'package:flutter_map/flutter_map.dart';
import 'package:latlong/latlong.dart';

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Flutter - Mapa"),
        ),
      ),
    );
  }
}

```

A classe MyApp é do tipo statefulWidget o que permite aos widgets dispostos na tela serem atualizados. O widget scaffold foi adicionado no argumento **home** do widget MaterialApp. A parte final do código adiciona a barra superior do aplicativo, conhecida como AppBar, e o título do aplicativo.

```flutter
// Habilita o uso do material design no aplicativo.
MaterialApp(
      // Scaffold widget.
      home: Scaffold(
      // Barra superior do aplicativo.
        appBar: AppBar(
        // titulo do aplicativo.
          title: Text("Flutter - Mapa"),
        ),
      ),
    );

```
Ao rodar o aplicativo você obterá o seguinte resultado:



![](/img/post_flutter_markers/ui_inicial.png)


#### 4. Adicionar o Mapa

Agora iremos adicionar o mapa ao nosso aplicativo utilizando as bibliotecas importadas anteriormente. Para isso, no argumento **body**, do widget scaffold, adicione o widget FlutterMap(). Este widget apresenta **options** e **layers** como argumentos. O argumento **options** espera um widget do tipo MapOptions e o argumento layers espera uma lista do tipo LayerOptions. O código abaixo irá adicionar o mapa em nosso aplicativo.


```flutter
Scaffold(
            appBar: AppBar(
              // Titulo da pagina
              title: Text("Flutter - Mapa"),
            ),
            body: FlutterMap(
              options: MapOptions(

                // Coordenada central do mapa.
                  center: LatLng(-15.799862, -47.864195),
                  // Quantidade de zoom do mapa.
                  zoom: 17),
              layers: [
                // Url do mapa.
                TileLayerOptions(
                  urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
                ),
              ],
            ),
          )

```

O widget mapOptions() foi passado como o argumento **options** do widget FlutterMap(), para estabelecer a coordenada central e quantidade de zoom que aparecerá ao iniciar mapa:

```
MapOptions(
              // Coordenada central do mapa - Congresso Nacional.
              center: LatLng(-15.799862, -47.864195),
              // Quantidade de zoom do mapa.
              zoom: 17),

```

Já no argumento layers, passamos uma lista que contém o widget TileLayerOptions(). Este widget recebe um url, no argumento **urltemplate**, que carrega o mapa no aplicativo.

```
[
            // Url do mapa.
            TileLayerOptions(
              urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
            ),
          ],
```

Agora é possível ver um mapa no aplicativo que está centralizado no Congresso Nacional, em Brasília.


![](/img/post_flutter_markers/mapa_app.gif)

#### 5. Adicionar os marcadores no mapa

Agora iremos adicionar marcadores ao clicar no mapa. Para isso, iremos criar uma varíavel chamada **tappedPoints** a qual receberá uma lista de pontos do tipo LatLng. O LatLng é o tipo de valor que é passado pela biblioteca ao interagir com o mapa. O dado apresenta a seguinte forma: LatLng(latitude:-15.798386, longitude:-47.864817). Além desta variável, precisamos também de uma função a qual adiciona os pontos a esta lista. Assim, criaremos também a função \_handleTap. Esta função tem como parâmentro de entrada valores do tipo LatLng e adiciona os pontos a lista tappedPoints quando houver um clique na tela. A função *setState()* atualiza o aplicativo para mostrar os marcadores na tela.

Logo abaixo de \_MyAppState adicione esta função e a variável.

```
class _MyAppState extends State<MyApp> {
  // Lista de pontos adicionados ao clicar na tela <LatLng>
  List<LatLng> tappedPoints = [];

// funcao que atualiza o estado do mapa e salva a coordenada na lista tappedPoints.
   void _handleTap(LatLng latlng) {
    setState(() {
      print(latlng);
      tappedPoints.add(latlng);
    });
  }
```

Agora passaremos esta função no argumento **onTap** no widget MapOptions(), adicionado na etapa anterior. Veja que agora ao clicar no mapa, as coordenadas lat e long aparecem no console do Android Studio. 

```
MapOptions(

              // Coordenada central do mapa.
              center: LatLng(-15.799862, -47.864195),
              // Quantidade de zoom do mapa.
              zoom: 17,
              onTap: _handleTap
          ),

```


![](/img/post_flutter_markers/ontap.gif)


Para fazer os marcadores aparecerem ao clicar na tela, é preciso criar uma nova variável **markers** que definirá o tipo do marcador adicionado e suas dimensões. Esta variável deve ser adicionada dentro da função build.

```
Widget build(BuildContext context) {
 var markers = tappedPoints.map((latlng) {
      return Marker(
        // dimensao dos marcadores
        width: 80.0,
        height: 80.0,
        // coordenadas do marcadores.
        point: latlng,
        builder: (ctx) => Container(
          child: Icon(
            // Icone do marcador
            Icons.pin_drop,
            color: Colors.red,
          ),
        ),
      );
    }).toList();
   ```
   
O valor de latlng da lista **tappedPoints** será utilizado como dicionário (map) para os marcadores. A função de mapeamento retornará um widget do tipo Marker o qual permite difinir a largura (width), altura (height) e coordenada (point) como argumentos. Além disso, no argumento builder é definido o widget que aparecerá no mapa. Aqui será retornado um Widget Container que têm como filho um ícone (*pin_drop*) de cor vermelha.

Para adicionar os ícones ao mapa, adicione na lista de layers do widget FlutterMap(), o widget MarkerLayerOptions(), com o argumento **markers** apontando para a varíavel markers criada anteriormente.


```
layers: [
            // Url do mapa.
            TileLayerOptions(
              urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
            ),
            // Marcadores
            MarkerLayerOptions(markers: markers)
          ],

```

Agora ao clicar na tela aparecem pins de cor vermelha.

![](/img/post_flutter_markers/pins.gif)


#### 6. Mostrar SnackBar com as coordenadas

Para finalizar, iremos colocar um widget do tipo GestureDetector() nos ícones adicionados ao mapa para mostrar a coordenadas dos pins adicionados. Assim, envolva o widget Container() com o widget GestureDetector(). No argumento onTap, vamos adicionar um SnackBar com as coordenadas de cada ponto.

```
var markers = tappedPoints.map((latlng) {
      return Marker(
        width: 80.0,
        height: 80.0,
        point: latlng,
        builder: (ctx) => GestureDetector(
          onTap: () {
            // Mostrar uma SnackBar quando clicar em um marcador
            Scaffold.of(ctx).showSnackBar(SnackBar(
                content: Text("Latitude =" +
                    latlng.latitude.toString() +
                    " :: Longitude = " +
                    latlng.longitude.toString())));
          },
          child: Container(
            child: Icon(
              // Icone do marcador
              Icons.pin_drop,
              color: Colors.red,
            ),
          ),
        ),
      );
    }).toList();
```

Pronto, agora ao clicar nos pins do mapa suas coordenadas aparecem na parte inferior da tela.

![](/img/post_flutter_markers/app_final.gif)


#### Código Completo
```
import 'package:flutter/material.dart';

// Importar bibliotecas
import 'package:flutter_map/flutter_map.dart';
import 'package:latlong/latlong.dart';

void main() => runApp(MyApp());


class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  // Lista de pontos adicionados ao clicar na tela <LatLng>
  List<LatLng> tappedPoints = [];

// funcao que atualiza o estado do mapa e salva a coordenada na lista tappedPoints.
  void _handleTap(LatLng latlng) {
    setState(() {
      print(latlng);
      tappedPoints.add(latlng);
    });
  }

  @override
  Widget build(BuildContext context) {
    var markers = tappedPoints.map((latlng) {
      return Marker(
        // dimensao dos marcadores
        width: 80.0,
        height: 80.0,
        // coordenadas do marcadores.
        point: latlng,
        builder: (ctx) => GestureDetector(
          onTap: () {
            // Mostrar uma SnackBar quando clicar em um marcador
            Scaffold.of(ctx).showSnackBar(SnackBar(
                content: Text("Latitude =" +
                    latlng.latitude.toString() +
                    " :: Longitude = " +
                    latlng.longitude.toString())));
          },
          child: Container(
            child: Icon(
              // Icone do marcador
              Icons.pin_drop,
              color: Colors.red,
            ),
          ),
        ),
      );
    }).toList();

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Flutter - Mapa"),
        ),
            body: FlutterMap(
              options: MapOptions(

                // Coordenada central do mapa.
                  center: LatLng(-15.799862, -47.864195),
                  // Quantidade de zoom do mapa.
                  zoom: 17,
              onTap: _handleTap),
              layers: [
                // Url do mapa.
                TileLayerOptions(
                  urlTemplate: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
                ),
                MarkerLayerOptions(markers: markers),
              ],
            ),
      ),
    );
  }
}


```






