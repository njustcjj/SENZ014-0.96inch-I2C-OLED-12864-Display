# SENZ014 0.96寸 I2C OLED 12864 显示屏

###### 翻译

> `英文` 请参考 [`这里`](https://github.com/njustcjj/SENZ014-0.96inch-I2C-OLED-12864-Display/blob/master/README.md)

> `中文` 请参考 [`这里`](https://github.com/njustcjj/SENZ014-0.96inch-I2C-OLED-12864-Display/blob/master/README_CN.md)

![](https://github.com/njustcjj/SENZ014-0.96inch-I2C-OLED-12864-Display/blob/master/pic/SENZ014_front.jpg "SENZ014_Front")
![](https://github.com/njustcjj/SENZ014-0.96inch-I2C-OLED-12864-Display/blob/master/pic/SENZ014_back.jpg "SENZ014_Back")
 

### 产品介绍

> SENZ014 OLED显示屏是一款无需背景光源，自发光式的显示模块。模块采用蓝色背景，显示尺寸控制在0.96英寸，采用OLED专用驱动芯片SSD1306控制。该模块支持通过I2C接口与控制器通信，支持高传输速率，能够实现60Hz的刷新频率。
> 
> OLED又称作有机发光二极管，相比传统的LCD，OLED具备更快的响应速度和更轻薄的体积优势，屏幕厚度可以小于1毫米，仅为LCD屏幕的1/3左右。并且功耗更低，抗震性好，可以广泛应用于移动设备的显示应用上。


### 产品参数

- 工作电压：+3 ~ 5V
- 显示颜色：蓝色
- 像素个数：128列 x 64行
- 接口方式：I2C
- 刷屏速率：60fps
- FOV：> 160°
- 全屏点亮功耗：80mA
- 工作温度：-30 ~ +70℃
- 显示面积：21.744 x 10.864mm
- 模块尺寸：27.3 x 27.8 x 3.7mm
- 安装孔尺寸：Φ 20mm


### 使用教程

#### 引脚定义

|Sensor pin|Ardunio Pin|Function Description|
|-|:-:|-|
|VCC|+3~5V|Power|
|GND|GND||
|D0|Digital pin|Clock|
|D1|Digital pin|Data, MOSI|
|RES|Digital pin|Reset|
|DC|Digital pin|Data/Command, A0|
|CS|Digital pin|Chip selection|


![](https://github.com/njustcjj/SENZ014-0.96inch-I2C-OLED-12864-Display/blob/master/pic/SENZ014_pin.jpg "引脚定义") 


#### 连线图

![](https://github.com/njustcjj/SENZ014-0.96inch-I2C-OLED-12864-Display/blob/master/pic/SENZ014_connect.png "连线图") 
See sample code:
>VCC = 3.3V, GND =GND, CS = 10, DC = 9, RES = 3,  D0 = 13 and D1 = 11

#### 支持的库文件
1. 通用U8glib库文件，点击[`下载`](https://bintray.com/olikraus/u8glib/download_file?file_path=u8glib_arduino_v1.18.1.zip)
2. Arduino 101适用的U8g2 Library库文件，点击[`查阅`]（https://github.com/olikraus/u8g2/wiki/u8g2reference）


### 示例代码


	//本程序实现了一个字符串向左移动，向右移动，向下称动，向上移动，在同一个位置显示一个ASCII字符
	//将各个函数改写使之通用化
	#include "U8glib.h"

	U8GLIB_SSD1306_128X64 u8g(10, 9, 3);              
	/* 
	HW SPI Com: CS = 10, A0 = 9, RST = 3 (Hardware Pins are  SCK = 13 and MOSI = 11)
	*/

	char str1[]="HW SPI Com: CS = 10, A0 = 9 ";
	char str2[]="HW SPI Com: CS = 10, A0 = 9 (Hardware Pins are  SCK = 13 and MOSI = 11)";
 
 
	void moveStrToRight(char *str,int x,int y)
	//这个函数未实现字符串从左向右移动，而是出现了字符串叠加的现象。
	//原来是字符串太长了
	{
 
	  for (int i=0;i<=12;i++)
	  { 
	    u8g.firstPage();  
	    do {
 
	      u8g.drawStr(i*10+x,y,(char *)str);
 
		    } 
	    while( u8g.nextPage() );    
	    delay(100);
	  }  
 
	}
 
	void moveStr2ToRight(char *str1,char *str2,int x1,int y1,int x2,int y2)
	//这个函数未实现字符串从左向右移动，而是出现了字符串叠加的现象。
	//原来是字符串太长了
	{
 
	  for (int i=0;i<=12;i++)
	  { 
	    u8g.firstPage();  
	    do {
 
	      u8g.drawStr(i*10+x1,y1,(char *)str1);
	      u8g.drawStr(i*10+x2,y2,(char *)str2);
		    } 
	    while( u8g.nextPage() );    
	    delay(100);
	  }  
 
	}
 
 
	void moveStrToLeft(char *str,int x, int y)
	//这个函数未实现字符串从右向左移动，而是出现了字符串叠加的现象。
	//原来是字符串太长了
	{

 
	  for (int i=0;i<=12;i++)
	  { 
	    u8g.firstPage();  
	    do {
	      u8g.drawStr(x-i*10,y,(char *)str1);
 
		    } 
	    while( u8g.nextPage() );    
	    delay(100);
	  }  
 
	}
	void moveStrToDown(char *str,int x ,int y)// 这个过程是字符串竖直排,从上向下移动位置
 
	{
 
	  //注这个字符串共有20个字符，20*8=160（字体像素以6*8计算）
	  //显示的位置是从Y轴-160开始，然后每次向下增进10个像素值，从上向下完全的完整显示字符串
	  for (int i=0;i<=10;i++)
	  { 
	    u8g.firstPage();  
	    do {
	      u8g.drawStr90(x,i*16-y,(char *)str);

		    } 
	    while( u8g.nextPage() );    
	    delay(100);
	  }  
 
	}
 
	void moveStrToUp(char *str,int x,int y)// 这个过程是字符串竖直排,从下向上移动位置
 
	{
 
 
	  //str1[]  注这个字符串共有20个字符，20*8=160（字体像素以6*8计算）
	  //显示的位置是从Y轴-160开始，然后每次向上增进10个像素值，从下向上完全的完整显示字符串
	  //为了能显示最后一个字符，Y轴的值至少为-20*8=-160
	  for (int i=0;i<=17;i++)
	  { 
	    u8g.firstPage();  
	    do {
	      u8g.drawStr90(x,y-i*10,(char *)str1);
 
		    } 
	    while( u8g.nextPage() );    
	    delay(100);
	  }  
 
	}
	void u8g_prepare(void) {
	  u8g.setFont(u8g_font_6x10);
	  u8g.setFontRefHeightExtendedText();
	  u8g.setDefaultForegroundColor();
	  u8g.setFontPosTop();
	}
	uint8_t draw_state = 0;
 
	void mydraw(void) {
	  u8g_prepare();
	  switch(draw_state >> 3) {
	  case 0: 
	    moveStrToLeft(str1,120,10); //x=20(个字符数）*6（字符宽）,y=10差不多在第一行
	    break;
	  case 1:   
	    moveStrToRight(str1,0,10) ; //x=0,y=10 ,y=10差不多在第一行
	    break;
	  case 2: 
	    moveStrToUp(str1,10,60); //x=10,y=60 x在超过一个字符位置，Y在差不多第六行
	    break;
	  case 3: 
	    moveStrToDown(str1,10,120); //x=10,由于str1字符长度20个，字符串长度是=20*8=160，y就等于120
	    break;
	  case 4: 
	    displayAchar(); //在依次显示不同的ASCII字符
	    break; 
	  case 5: 
	    u8g_ascii_1(30,50); //在指定的坐标，显示不同的ASCII字符
	    break; 
 
	  }
	}
 
 
	void u8g_ascii_1(int i,int j) //显示ascii在屏幕上
	{
	  char s[2] = " ";
	  uint8_t x, y;
 
	  for( y = 0; y < 6; y++ ) {
	    for( x = 0; x < 16; x++ ) 
	    {
	      s[0] = y*16 + x + 32;
	      do {
	        u8g.drawStr( 0, 0, "ASCII page 1");
	        u8g.drawStr(i, j, s);   
	        //如果需要依次在不同的位置显示，如下行
	        //  u8g.drawStr(x*7+i, y*10+j, s);        
	        delay(50);
 
		      } 
	      while( u8g.nextPage() );

	    }
	  }
	}
 
 
	void setup()
	{
 
 
	  if ( u8g.getMode() == U8G_MODE_R3G3B2 ) 
	    u8g.setColorIndex(255);     // white
	  else if ( u8g.getMode() == U8G_MODE_GRAY2BIT )
	    u8g.setColorIndex(3);         // max intensity
	  else if ( u8g.getMode() == U8G_MODE_BW )
	    u8g.setColorIndex(1);         // pixel on
 
	  // u8g.setFont(u8g_font_unifont);
	  Serial.begin(9600);
 
	  u8g_prepare();//初始化字体，屏幕参数

	}
 
	void loop()
	{
 
 
	  //moveStr2ToRight(str1,str2,0,0,0,10);
	  mydraw();
	  draw_state++;
	  if ( draw_state >= 7*8 )
	    draw_state = 0;
 
	  // rebuild the picture after some delay
	  delay(100);
 
	}
 
 
 
	void displayAchar()//这个过程,依次在不同的坐标，显示不同ASCII的字符
	{
 
	  char s[2] = " ";
	  uint8_t x, y;
 
	  for( y = 0; y < 6; y++ ) {
	    for( x = 0; x < 16; x++ ) 
	    {
	      s[0] = y*16 + x + 32;
	      u8g.firstPage();  
	      do {
	        u8g.drawStr( 0, 0, "ASCII page 1");
	        //  u8g.drawStr(31, 40, s);   
	        //如果需要依次在不同的位置显示，如下行
	        u8g.drawStr(x*7, y*10, s);        
	        delay(50);
 
	      } 
	      while( u8g.nextPage() );

	    }
	  }
 
	}


### 购买[*SENZ014 0.96寸 I2C OLED 12864 显示屏*](https://www.ebay.com/).