---
layout: post
title: Tutorial 1  - Flutter GIS - Como Adicionar Marcadores em um Mapa
subtitle: 
tags: [flutter, GIS, Tutorial]
---

Neste post mostrarei como desenvolver um aplicativo de SIG com o Flutter que permite adicionar pontos em um mapa e obter sua coodernada.

 
![](/img/post_flutter_markers/marker_demo.gif)



O Flutter é um SDK desenvolvido pela Google que permite o desenvolvimento de aplicativos para multiplataformas, isto é, com o mesmo
código é possível desenvolver aplicativos para android, IOS, web e desktop. Este framework ainda não é muito utilizada no mercado, porém,
a comunidade de desenvolvedores e bibliotecas diponíveis estão aumentando rapidamente, além disso, startups como a NuBank já utilizam
o Flutter para desenvolver seus aplicativos. O lançamento do Fuchsia, novo sistema operacional da Google, deve aumentar ainda mais o uso deste SDK,
já que os aplicativos serão desenvolvidos com base no Flutter. Portanto, a tendência é que o Flutter seja cada vez mais utilizado no mercado.

Para desenvolver este aplicativo é necessário que você já tenha o Flutter instalado em sua máquina e que saiba conceitos básicos da Liguagem Dart, utilizada pelo Flutter, e de sistema de informações geográficas (SIG).O Android Studio é a IDE utilizada neste tutorial. 


### Workflow

- [0. Criar um novo projeto Flutter](#0-criar-um-novo-projeto-flutter) 

- [1. Adicionar as bibliotecas](#1-adicionar-as-bibliotecas) 

- [2. Importar as bibliotecas](#2-importar-as-bibliotecas) 

- [3. Desenvolver a UI](#3-desenvolver-a-UI) 

- [3. Adicionar o mapa](#4-Adicionar-o-mapa) 

&nbsp;

#### 0. Criar um novo projeto flutter

Abra o Android Studio e crie um novo projeto do Flutter, escolha o nome e o local em qual o projeto será salvo.

![](/img/post_flutter_markers/new_project.png)


#### 1. Adicionar as bibliotecas

Utilizaremos as bibliotecas flutter_map e latlong:

- [Flutter_map](<https://pub.dev/packages/flutter_map>)

-[latlong](<https://pub.dev/packages/latlong>)


Vá ao arquivo *pubspec.yaml* e adicione o código abaixo:

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

A classe MyApp é do tipo statefulWidget o que permite aos widgets dispostos na tela serem atualizados futuramente. O widget scaffold foi adicionado no atributo **home** do widget MaterialApp. A parte final do código adiciona a barra superior do aplicativo, conhecida como AppBar, e o título do aplicativo.

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

Agora iremos adicionar o mapa ao nosso aplicativo utilizando as bibliotecas importadas anteriormente. Para isso, no atributo **body**, do widget scaffold, adicione o widget FlutterMap(). Este widget apresenta **options** e **layers** como atributos. O atributo **options** espera um widget do tipo MapOptions e o atributo layers espera uma lista do tipo LayerOptions. O código abaixo irá adicionar o mapa em nosso aplicativo.


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

O widget mapOptions() foi passado como atributo **options** do widget FlutterMap() para estabelecer a coordenada central e quantidade de zoom que aparecerá ao iniciar mapa:

```
MapOptions(
              // Coordenada central do mapa - Congresso Nacional.
              center: LatLng(-15.799862, -47.864195),
              // Quantidade de zoom do mapa.
              zoom: 17),

```

Já no atributo layers, passamos uma lista que contém o widget TileLayerOptions(). Este widget recebe um url, no atributo **urltemplate**, que carrega o mapa no aplicativo.

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




