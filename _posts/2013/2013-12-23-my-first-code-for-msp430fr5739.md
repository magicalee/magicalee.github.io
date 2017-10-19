---
layout: post
title: 'my first code for msp430fr5739'
tags: ["Coding"]
date: 2013-12-23 22:37
comments: true
categories:
---
最近因為學校的專題課程，而入手了這片"msp430fr5739 experimenter board"。由於教授鼓勵我們從網路上自己學習，自己找資料，所以在花費幾乎一整天的時間下，我終於成功自己寫出能讓LED燈亮起來的程式，並且讓它再變成可以讓所有LED由1~8全部依序亮起來的code。以下就是我的code
<!--more-->
```
	/****************************************************************
	 * Date:2013.12.23
	 * Author:TDL
	 * Description:
	 * 		This is my first code on msp430fr5739 experimenter board.
	 * 		In this code I attempt to blink the LED from LED1 to LED8.
	 *****************************************************************/
	#include "msp430fr5739.h"
	
	void main(void) {
	 WDTCTL = WDTPW + WDTHOLD;  //stop watchdog
	 PJDIR = 0x00; 				//Initialize Port 3 as output by ensuring bit 0 is 0.
	 PJDIR = 0x00; 				//Initialize Port J as output by ensuring bit 0 is 0.
	 unsigned char LED[] = {BIT0,BIT1,BIT2,BIT3,BIT4,BIT5,BIT6,BIT7};
	 unsigned char i;
	 while(1){
		 for(i = 0 ; i<=8 ; i++){
			 if(i<4){
				 P3DIR = 0x00; 		//set port 3 output as low
				 PJDIR = 0xff; 		//set port J output as high
				 PJOUT = LED[i];	//PJ output to Blink LED from 1 to 4
			 }
			 if((i>=4)&& (i<=8)){
				 PJDIR = 0x00;
				 P3DIR = 0xff;
				 P3OUT = LED[i];	//P3 output to Blink LED from 5 to 8
			 }
			 _delay_cycles(100000); //delay 100000 cycles at 1MHz
		 }
	 }
	}
```
這是我參考之前單純讓一顆LED亮的程式碼後再自己加上讓所有LED都亮一遍的code。因為還不是很懂DIR的應用，所以好像寫了許多多餘的code。但whatever，看到LED按照自己的心意成功的亮起來，還是非常的有成就感。