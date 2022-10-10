# 颜值PK安卓客户端


![](1010day1.gif "1010day1.gif")



yaml中导入   image_picker: ^0.8.6

    

```


import 'dart:io';

import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp();

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // This is the theme of your application.
        //
        // Try running your application with "flutter run". You'll see the
        // application has a blue toolbar. Then, without quitting the app, try
        // changing the primarySwatch below to Colors.green and then invoke
        // "hot reload" (press "r" in the console where you ran "flutter run",
        // or simply save your changes to "hot reload" in a Flutter IDE).
        // Notice that the counter didn't reset back to zero; the application
        // is not restarted.
        primarySwatch: Colors.deepPurple,
      ),
      home: const MyHomePage("颜值PK"),
    );
  }
}
class Adapt {
  static MediaQueryData _mediaQueryData = MediaQueryData();
  static double screenWidth = 0;
  static double screenHeight = 0;
  static double rpx = 0;
  static double px = 0;
  static double vh = 0;

  static void initialize(BuildContext context, {double standardWidth = 750}) {
    _mediaQueryData = MediaQuery.of(context);
    screenWidth = _mediaQueryData.size.width;
    screenHeight = _mediaQueryData.size.height;
    rpx = screenWidth / standardWidth;
    px = screenWidth / standardWidth * 2;
    vh = screenHeight/100;
  }

  // 按照像素来设置
  static double setPx(double size) {
    return Adapt.rpx * size * 2;
  }

  // 按照rxp来设置
  static double setRpx(double size) {
    return Adapt.rpx * size;
  }
  static double setVh(double size) {
    return Adapt.vh * size;
  }
}


class MyHomePage extends StatefulWidget {
  const MyHomePage(this.title);

  // This widget is the home page of your application. It is stateful, meaning
  // that it has a State object (defined below) that contains fields that affect
  // how it looks.

  // This class is the configuration for the state. It holds the values (in this
  // case the title) provided by the parent (in this case the App widget) and
  // used by the build method of the State. Fields in a Widget subclass are
  // always marked "final".

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  File _image;
  File _image1;
  File _image2;
  File _image3;
  int sign=0;
  ImagePicker imagePicker;

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    imagePicker=ImagePicker();
  }

  Future<void> ChooseImageFromGallery() async {

    PickedFile pickedFile=await imagePicker.getImage(source: ImageSource.gallery);

      setState(() {
        _image=File(pickedFile.path);
      });
  }
  Future<void> ChooseImageFromCamera() async {
    // PickedFile pickedFile;
    // PickedFile pickedFiler;
    PickedFile pickedFile=await imagePicker.getImage(source: ImageSource.camera);
    if(sign==0){
      // pickedFile=await imagePicker.getImage(source: ImageSource.camera,preferredCameraDevice:CameraDevice.front);
      setState(() {
        sign=1;
        _image=File(pickedFile.path);
      });
    }else if(sign==1){
      // pickedFile=await imagePicker.getImage(source: ImageSource.camera,preferredCameraDevice:CameraDevice.rear);
      setState(() {
        sign=2;
        _image1=File(pickedFile.path);
      });
    }else if(sign==2){
      // pickedFile=await imagePicker.getImage(source: ImageSource.camera,preferredCameraDevice:CameraDevice.rear);
      setState(() {
        sign = 3;
        _image2 = File(pickedFile.path);
      });
    }else if(sign==3){
      // pickedFile=await imagePicker.getImage(source: ImageSource.camera,preferredCameraDevice:CameraDevice.rear);
      setState(() {
        sign = 0;
        _image3 = File(pickedFile.path);
      });
    }

  }


  @override
  Widget build(BuildContext context) {
    Adapt.initialize(context);
    // This method is rerun every time setState is called, for instance as done
    // by the _incrementCounter method above.
    //
    // The Flutter framework has been optimized to make rerunning build methods
    // fast, so that you can just rebuild anything that needs updating rather
    // than having to individually change instances of widgets.
    return Scaffold(
      appBar: AppBar(
        // Here we take the value from the MyHomePage object that was created by
        // the App.build method, and use it to set our appbar title.
        title: Text(widget.title),
      ),
      body: Center(
        // Center is a layout widget. It takes a single child and positions it
        // in the middle of the parent.
        child: Column(
          // Column is also a layout widget. It takes a list of children and
          // arranges them vertically. By default, it sizes itself to fit its
          // children horizontally, and tries to be as tall as its parent.
          //
          // Invoke "debug painting" (press "p" in the console, choose the
          // "Toggle Debug Paint" action from the Flutter Inspector in Android
          // Studio, or the "Toggle Debug Paint" command in Visual Studio Code)
          // to see the wireframe for each widget.
          //
          // Column has various properties to control how it sizes itself and
          // how it positions its children. Here we use mainAxisAlignment to
          // center the children vertically; the main axis here is the vertical
          // axis because Columns are vertical (the cross axis would be
          // horizontal).
          mainAxisAlignment: MainAxisAlignment.end,
          children: <Widget>[
            Row(

              children: [
                Container(
                  width: Adapt.setRpx(375),
                  height: Adapt.setVh(40),
                  child:_image!=null?Image.file(_image):Icon(Icons.image,size: 100,),
                ),

                Container(
                  width: Adapt.setRpx(375),
                  height: Adapt.setVh(40),
                  child:_image1!=null?Image.file(_image1):Icon(Icons.image,size: 100,),
                ),
              ],
            ),
            Row(
              children: [
                Container(
                  width: Adapt.setRpx(375),
                  height: Adapt.setVh(40),
                  child:_image2!=null?Image.file(_image2):Icon(Icons.image,size: 100,),
                ),

                Container(
                  width: Adapt.setRpx(375),
                  height: Adapt.setVh(40),
                  child:_image3!=null?Image.file(_image3):Icon(Icons.image,size: 100,),
                ),
              ],
            ),
            MaterialButton(onPressed: (){
              ChooseImageFromCamera();
            },child: Text("拍照"),color: Colors.deepPurple,textColor: Colors.white,
            minWidth: double.infinity)
          ],
        ),
      ),

    );
  }
}

```