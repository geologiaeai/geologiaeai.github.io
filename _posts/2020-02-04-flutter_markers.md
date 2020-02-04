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

- [0. Adicionar as bibliotecas](#0-adicionar-as-bibliotecas) 

&nbsp;

#### 0. Criar um novo projeto flutter

Abra o Android Studio e crie um novo projeto do Flutter, escolha o nome e o local em qual o projeto será salvo.

![](/img/post_flutter_markers/new_project.png)


#### 1. Adicionar as bibliotecas

Utilizaremos as bibliotecas flutter_map e latlong

- [Flutter_map](<https://pub.dev/packages/flutter_map>)

-[latlong](<https://pub.dev/packages/latlong>)


Vá ao arquivo *pubspec.yaml* e adicione o código abaixo:

```flutter
flutter_map: ^0.8.2
latlong: ^0.6.1
```






