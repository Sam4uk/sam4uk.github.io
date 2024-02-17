---
title: "PassWord"
#title: Markdown синтаксис
date: 2024-02-17
description: Handle passwords easily
draft: false
hideToc: false
enableToc: false
enableTocContent: false
# author: Choi
authoravatar:
authorEmoji: 🤖
tags:
categories:
- Arduino
series:
- Arduino
# image: http://www.artvex.com/content/Clip_Art/Electronics_and_Technology/Remote_Controls/0010884.gif
---

How this library appeared on my blog. I am working on a project created to work in the `Arduino` framework. During the development process, I transferred the compilation of the entire project to the `github` service. Where all the compilation of the project takes place (I need the project to be able to restore the board to its previous state without having access to the source file). One of the developers of the project included in the project a library that is not in the official register of the library manager, so I took it upon myself to publish the library. The license allows you to modify it.

I did not change the code, just added a few additional comments in the style of `doxygen` and made it possible to download it directly from the `Arduino` framework

PassWord
========

HelloPassword
-------------

```cpp
/**
 *
 * @file HelloPassword.ino
 * @version 1.0
 * @author Alexander Brevig
 * @authors Alexander Brevig, Sam4uk
 *
 * @details A demonstration of the simple API of the Password library
 *
 */

#include <Password.h>

Password  //
    password = Password("1234");
const char  //
    _true_[] = "true",
    _false_[] = "false";

void setup() {
  Serial.begin(9600);

  password.append('1');    ///< add 1 to the guessed password
  password.append('2');    ///< add 2 to the guessed password
  password << '3' << '4';  ///< add 3 and 4 to the guessed password
  Serial.println(password.evaluate()
                     ? _true_
                     : _false_);  ///< should print true, since 1234 == 1234

  password.reset();  ///< reset the guessed password to NULL
  Serial.println(password.evaluate()
                     ? _true_
                     : _false_);  ///< should print false, since 1234 != NULL

  password.set("qwerty");  ///< set target password to qwerty
  Serial.println(password.is("qwerty")
                     ? _true_
                     : _false_);  ///< should print true, since qwerty == qwerty
  Serial.println(
          password == "qwirty"
          ? _true_
          : _false_);  ///< should print false, since qwerty != qwirty
}

void loop() { /*nothing to loop*/
}
```
[HelloPassword.ino](https://raw.githubusercontent.com/Sam4uk/Password/master/examples/HelloPassword/HelloPassword.ino)


PasswordKeypad
--------------

```cpp
/**
 * @file PasswordKeypad.ino
 * 
 * @brief Simple Password Entry Using Matrix Keypad 
 *  
 * @authors Nathan Sobieck (athan@Sobisource.com)
 * 
 * @note
 *  2012-05-04 - Nathan Sobieck : Simple Password Entry Using Matrix Keypad
 */

//* is to validate password   
//# is to reset password attempt

/////////////////////////////////////////////////////////////////

#include <Password.h> 
#include <Keypad.h>

Password password = Password( "1234" );

const byte ROWS = 4; ///< rows
const byte COLS = 4; ///<  columns
// Define the Keymap
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'} ///< * is to validate password
                    ///< # is to reset password attempt  
};

byte rowPins[ROWS] = 
    {9, 8, 7, 6};  ///< Connect keypad ROW0, ROW1, ROW2 and ROW3 to these Arduino pins.
byte colPins[COLS] = 
    {5, 4, 3, 2};  ///< Connect keypad COL0, COL1 and COL2 to these Arduino pins.

// Create the Keypad
Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

void setup(){

  Serial.begin(9600);
  keypad.addEventListener(keypadEvent); ///< add an event listener for this keypad
}

void loop(){
  keypad.getKey();
}

///take care of some special events
void keypadEvent(KeypadEvent eKey){
  switch (keypad.getState()){
    case PRESSED:
	Serial.print("Pressed: ");
	Serial.println(eKey);
	switch (eKey){
	  case '*': checkPassword(); break;
	  case '#': password.reset(); break;
	  default: password.append(eKey);
     }
  }
}

void checkPassword(){
  if (password.evaluate()){
    Serial.println("Success");
    //Add code to run if it works
  }else{
    Serial.println("Wrong");
    //add code to run if it did not work
  }
}
```
[PasswordKeypad](https://raw.githubusercontent.com/Sam4uk/Password/master/examples/PasswordKeypad/PasswordKeypad.ino)

SerialMonitor
-------------
```cpp
/**
 *
 * @file SerialMonitor.ino
 * @version 1.1
 * @author Alexander Brevig, Sam4uk
 *
 * @details A simple password application that uses the serial monitor as input
 * source.
 *
 */

#include <Password.h>

Password  //
    password = Password("1234");

byte  //
    currentLength = 0;

void setup() {
  Serial.begin(9600);
  Serial.println("Try to guess the password!");
  Serial.println("Reset with ! evaluate with ?");
  Serial.print("Enter password: ");
}

void loop() {
  if (Serial.available()) {
    char input = Serial.read();
    switch (input) {
      case '!':  ///< reset password
        password.reset();
        currentLength = 0;
        Serial.println("\tPassword is reset!");
        break;
      case '?':  ///< evaluate password
        if (password.evaluate()) {
          Serial.println("\tYou guessed the correct password!");
        } else {
          Serial.println("\tYou did not guess the correct password!");
        }
        break;
      default:  ///< append any keypress that is not a '!' nor a '?' to the
                ///< currently guessed password.
        password << input;
        currentLength++;

        /// Print some feedback.
        Serial.print("Enter password: ");
        for (byte i = 0; i < currentLength; ++i) {
          Serial.print('*');
        }
        Serial.println();
    }
  }
}
```
[SerialMonitor](https://raw.githubusercontent.com/Sam4uk/Password/master/examples/SerialMonitor/SerialMonitor.ino)