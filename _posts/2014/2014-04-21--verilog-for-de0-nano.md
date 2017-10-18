---
layout: post
title: 'First Verilog Code For DE0-Nano'
date: 2014-04-21 11:05
comments: true
categories: [Verilog, DE0-Nano, 初學, LED]
---
下半學期的專題研究課程終於擺脫了「手工藝」也就是焊接的訓練了，我們三個人開始朝著各自所選的目標去努力。這階段分成微控器msp430撰寫、電路版設計、Verilog撰寫三個方向，而我是朝著精通Verilog的目標去專研。
一開始教授就給我了一塊開發版─DE0-Nano
![IMG_2991.JPG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/vPGJQs3dRlG1UlnfkyVr_IMG_2991.JPG)
<!--more-->
小小的一塊版子，上面有一些按鈕、LED、與Gsensor，你得自己控制接線與邏輯閘，讓他跑出妳想要的成果。
開發環境是Quartus 2，以下是我研究網路上的文章，做出來跟之前MSP430一樣行為的Project，也就是讓LED一個接一個的亮。
給初學者參考一下步驟。
開啟Quartus 2，建立新專案File>New Project Wizzard，打上Project Name,再按next到page 3 of 5
![1.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/JxEHEYHJRqaiN0bLdOhK_1.PNG)
選擇正確的Device Family，並在Available devices裡選出開發版上元件的型號再按下Finish
![1.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/rN4L5T8SGJIC8bahKlgl_1.PNG)
左邊會出現之前打的Poject Name(test1)，但這時點test1會出現錯誤，因為們還沒新增.v檔，按下File>New
![1.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/qWTxDLHCR8GsY4Zr4GJn_1.PNG)
選擇Verilog HDL File
![擷取.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/EVTpXH3qQl6A3jinNHpe_%E6%93%B7%E5%8F%96.PNG)
貼上以下程式碼
```ruby test1.v
`timescale 1ns / 1ps
module test1(input clk,input reset,output [7:0] led );
       
        wire clkw;
        reg [7:0] led_output;
        reg [31:0] divider;
//除頻電路
        always@(posedge clk or negedge reset )
        begin
                if ( !reset )
                        divider <= 32'h0000_0000 ;
                else
                        divider <= divider + 1;
        end
        assign clkw = divider[19];  // 5M/2^20=0.3125 sec 
//LED左移電路
         always@( posedge clkw or negedge reset )
         begin
                if ( !reset )
                        led_output <= 8'h01 ;
                else
                ledout <= { led[6:0], led_output[7]};           
         end
         assign led = led_output;     
endmodule
```
初學者很常出現的錯誤是更改到module name，在一個Project裡至少要有一個module name跟Project name是一樣的，不然會出現Error。另外Verilog跟msp430不同的是除了程式碼的撰寫外，還得處理每個input,output所對應的原件，按照datasheet一個一個自己去assign。
按下Processing>Start Compilation確定都沒有Error後，再點選Assignments>Pin Planner，出現如下圖
![擷取.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/EDFi5me0SrixGSkeJjFQ_%E6%93%B7%E5%8F%96.PNG)
底下會出現妳之前Code裡定義的input,output，這時妳就要按照datasheet的pin去assign，可以點右邊的圖，比如PIN_A3，妳就點圖上坐標為A3的點，並在Node Name裡找到對應的變數，或是直接在底下找到變數所對應的Location打上PIN_A3也是可以
![擷取.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/siLFO8p0S2a2wLRf9ZaP_%E6%93%B7%E5%8F%96.PNG)
把全部要用到的變數都Assign完，就可以實際去跑跑看。再讓他Complie一次確認接線後，點選Tools>Programmer
![擷取.PNG](http://user-image.logdown.io/user/6700/blog/6666/post/207062/d2lDmVtRvCQRJAP68Zjz_%E6%93%B7%E5%8F%96.PNG)
確認開發版已連到電腦，按下start，就可以看到剛剛之前打的Code在開發版上完美呈現囉！


