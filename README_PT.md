
# flutter_screenutil
[![pub package](https://img.shields.io/pub/v/flutter_screenutil.svg)](https://pub.dartlang.org/packages/flutter_screenutil)

**Um plugin Flutter para adaptação de tamanho de tela e fontes. Deixe sua UI exibir um layout aceitável em diferentes tamanhos de tela!**

*Nota*: Este plugin ainda está em desenvolvimento e algumas APIs podem ainda não estar disponíveis.

[中文文档](https://github.com/OpenFlutter/flutter_screenutil/blob/master/README_CN.md)
[README of English](https://github.com/OpenFlutter/flutter_ScreenUtil/blob/master/README.md)

github: https://github.com/OpenFlutter/flutter_screenutil

[Histórico de atualizações](/CHANGELOG.md)

## Como usar:

### Adicionando a dependência：
Por favor, verifique a última versão antes da instalação.
```
dependencies:
  flutter:
    sdk: flutter
  # add flutter_ScreenUtil
  flutter_screenutil: ^0.5.3
```

### Adicione o seguinte import em seu código Dart:
```
import 'package:flutter_screenutil/flutter_screenutil.dart';
```

### Propriedades
   
|Propriedade|Tipo|Valor Padrão|Descrição|
|:---|:---|:---|:---| 
|width|double|1080px|A largura do dispositivo no protótipo de design, em px|
|height|double|1920px|A altura do dispositivo no protótipo de design, em px|
|allowFontScaling|bool|false|Defina caso o tamanho da fonte seja dimensionado de acordo com a opção "tamanho de fonte" na acessibilidade do sistema|

### Inicialize e defina o tamanho de ajuste e tamanho da fonte para dimensionar de acordo com a opção "tamanho de fonte" na acessibilidade do sistema
Por favor, defina a largura e altura do protótipo de design antes de usar (em pixels).
Certifique-se de definir as dimensões na paginal inicial do MaterialApp (ou seja, no arquivo de entrada, defina apenas uma vez) para garantir que o tamanho de ajuste seja o mesmo antes de cada uso:

```dart

//Preencha o tamanho da tela do dispositivo no protótipo de design

//Valor padrão: width : 1080px , height:1920px , allowFontScaling:false
ScreenUtil.instance = ScreenUtil.getInstance()..init(context);

//Se o design é baseado no iPhone6 ​​(iPhone6 ​​750*1334)
ScreenUtil.instance = ScreenUtil(width: 750, height: 1334)..init(context);

//Se você quer definir que o tamanho da fonte seja ajustado de acordo com a opção "tamanho da fonte" na acessibilidade do sistema
ScreenUtil.instance = ScreenUtil(width: 750, height: 1334, allowFontScaling: true)..init(context);

```

### Uso：

#### Adaptar o tamanho da tela：

Informe o tamanho em pixels do protótipo de design:

Adaptado à largura da tela: `ScreenUtil.getInstance().setWidth(540)`,

Adaptado à altura da tela: `ScreenUtil.getInstance().setHeight(200)`,

Você também pode utilizar `ScreenUtil()` ao invés de `ScreenUtil.getInstance()`,
por exemplo:`ScreenUtil().setHeight(200)`

**Nota** 

Altura também é adaptada de acordo com o setWidth para garantir que não tenha deformação (quando quiser um quadrado)

O método setHeight é a principal forma de adaptar a altura, se quiser controlar a altura e a realidade de uma tela na UiUsed quando a mesma for exibida.

```dart
//Exemplo:
//Retângulo
Container(
           width: ScreenUtil.getInstance().setWidth(375),
           height: ScreenUtil.getInstance().setHeight(200),
           ...
            ),
            
////Se quiser exibir um quadrado:
Container(
           width: ScreenUtil.getInstance().setWidth(300),
           height: ScreenUtil.getInstance().setWidth(300),
            ),
            
```

#### Adaptar fonte:
``` dart
//Tamanho da fonte informado, em pixels, a fonte não irá dimensionar respeitando a opção "Tamanho de Fonte" nas configurações de acessibilidade
//(allowFontScaling quando iniciar o ScreenUtil)
ScreenUtil.getInstance().setSp(28)    
     
//Tamanho da fonte informado，em pixels，a fonte irá dimensionar respeitando a opção "Tamanho de Fonte" nas configurações de acessibilidade
//(Se em algum lugar não seguir a configuração global da propriedade allowFontScaling)
ScreenUtil(allowFontScaling: true).setSp(28)  

//Exemplo:

Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: <Widget>[
                Text(
                    'Minha fonte tem 24px no protótipo de design e não irá mudar com o sistema.',
                    style: TextStyle(
                      color: Colors.black,
                      fontSize: ScreenUtil.getInstance().setSp(24),
                    )),
                Text(
                    'Minha fonte tem 24px no protótipo de design e poderá mudar com o sistema.',
                    style: TextStyle(
                      color: Colors.black,
                      fontSize: ScreenUtil(allowFontScaling: true).setSp(24),
                    )),
              ],
            )


```

#### Outras APIs relacionadas：
```dart
    ScreenUtil.pixelRatio       //Densidade de pixels do dispositivo
    ScreenUtil.screenWidth      //Largura da tela do dispositivo
    ScreenUtil.screenHeight     //Altura da tela do dispositivo
    ScreenUtil.bottomBarHeight  //Distância segura do rodapé, adequada para botões em tela cheia
    ScreenUtil.statusBarHeight  //Altura da status bar em pixels, Notch será maior
    ScreenUtil.textScaleFactory //Fator de escala da fonte do sistema

    ScreenUtil.getInstance().scaleWidth //Razão entre a largura atual e a largura do protótipo de design em pixels
    ScreenUtil.getInstance().scaleHeight //Razão entre a altura atual e a altura do protótipo de design em pixels

```

```dart
//import
import 'package:flutter_screenutil/flutter_screenutil.dart';

...

  @override
  Widget build(BuildContext context) {
    ///Define o tamanho de ajuste (preenche o tamanho da tela do dispositivo no design). Se o design é baseado no tamanho do iPhone6 (iPhone6 ​​750*1334)
    ScreenUtil.instance = ScreenUtil(width: 750, height: 1334)..init(context);
    
    print('Largura do dispositivo:${ScreenUtil.screenWidth}'); //Largura do dispositivo
    print('Altura do dispositivo:${ScreenUtil.screenHeight}'); //Altura do dispositivo
    print(
        'Densidade de pixels do dispositivo:${ScreenUtil.pixelRatio}'); //Densidade de pixels do dispositivo
    print(
        'Distância segura do rodapé:${ScreenUtil.bottomBarHeight}'); //Distância segura do rodapé, adequada para botões em tela cheia
    print(
        'Altura da status bar:${ScreenUtil.statusBarHeight}px'); //Altura da status bar em pixels, Notch será maior
    print(
        'Razão entre a largura atual e a largura do protótipo de design em pixels:${ScreenUtil.getInstance().scaleWidth}'); 
    print(
        'Razão entre a altura atual e a altura do protótipo de design em pixels:${ScreenUtil.getInstance().scaleHeight}'); 
    print(
        'Razão da fonte e largura para o tamanho do design:${ScreenUtil.getInstance().scaleWidth * ScreenUtil.pixelRatio}');
    print(
        'Razão da fonte e altura para o tamanho do design:${ScreenUtil.getInstance().scaleHeight * ScreenUtil.pixelRatio}');
        
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Center(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Row(
              children: <Widget>[
                Container(
                  padding: EdgeInsets.all(ScreenUtil.getInstance().setWidth(10)),
                  width: ScreenUtil.getInstance().setWidth(375),
                  height: ScreenUtil.getInstance().setHeight(200),
                  color: Colors.red,
                  child: Text(
                    'Minha largura:${ScreenUtil.getInstance().setWidth(375)}dp',
                    style: TextStyle(
                        color: Colors.white,
                        fontSize: ScreenUtil.getInstance().setSp(12)),
                  ),
                ),
                Container(
                  padding: EdgeInsets.all(ScreenUtil.getInstance().setWidth(10)),
                  width: ScreenUtil.getInstance().setWidth(375),
                  height: ScreenUtil.getInstance().setHeight(200),
                  color: Colors.blue,
                  child: Text('Minha largura:${ScreenUtil.getInstance().setWidth(375)}dp',
                      style: TextStyle(
                          color: Colors.white,
                          fontSize: ScreenUtil.getInstance().setSp(12))),
                ),
              ],
            ),
            Text('Largura do dispositivo:${ScreenUtil.screenWidth}px'),
            Text('Altura do dispositivo:${ScreenUtil.screenHeight}px'),
            Text('Densidade de pixels do dispositivo:${ScreenUtil.pixelRatio}'),
            Text('Distância segura do rodapé:${ScreenUtil.bottomBarHeight}px'),
            Text('Altura da status bar:${ScreenUtil.statusBarHeight}px'),
            Text(
              'Razão entre a largura atual e a largura do protótipo de design em pixels:${ScreenUtil.getInstance().scaleWidth}',
              textAlign: TextAlign.center,
            ),
            Text(
              'Razão entre a altura atual e a altura do protótipo de design em pixels:${ScreenUtil.getInstance().scaleHeight}',
              textAlign: TextAlign.center,
            ),
            Text(
              'Razão da fonte e largura para o tamanho do design:${ScreenUtil.getInstance().scaleWidth * ScreenUtil.pixelRatio}',
              textAlign: TextAlign.center,
            ),
            Text(
              'Razão da fonte e altura para o tamanho do design:${ScreenUtil.getInstance().scaleHeight * ScreenUtil.pixelRatio}',
              textAlign: TextAlign.center,
            ),
            SizedBox(
              height: ScreenUtil.getInstance().setHeight(100),
            ),
            Text('Fator de escala da fonte do sistema:${ScreenUtil.textScaleFactory}'),
            Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: <Widget>[
                Text(
                    'Minha fonte tem 24px no protótipo de design e não irá mudar com o sistema.',
                    style: TextStyle(
                      color: Colors.black,
                      fontSize: ScreenUtil.getInstance().setSp(24),
                    )),
                Text(
                    'Minha fonte tem 24px no protótipo de design e poderá mudar com o sistema.',
                    style: TextStyle(
                      color: Colors.black,
                      fontSize: ScreenUtil(allowFontScaling: true).setSp(24),
                    )),
              ],
            )
          ],
        ),
      ),
    );
  }
```

### Exemplo:

[Demonstração](/example/lib/main_zh.dart)
 
Efeito:

![效果](demo_en.png)
