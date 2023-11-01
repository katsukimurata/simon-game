# simon-game


// C++ code
//

  int botao1= 13;
  int botao2 = 12;
  int botao3 = 11;
  int botao4 = 10;
  int botaoreset = 2;
  int led1 = 7;
  int led2 = 6;
  int led3 = 5;
  int led4 = 4;
  int buzzer = 9;
  int leds[4][4] = {
        {7, 6, 5, 4},
        {4, 5, 6, 7}, 
        {7, 4, 5, 6}, 
        {5, 7, 4, 6} 
    };
int player[4]= {0,0,0,0};
int contbotao = 0;
int fase = 0;
void setup()
{
  	Serial.begin(9600);
	pinMode(botao1, INPUT_PULLUP);
    pinMode(botao2, INPUT_PULLUP);
    pinMode(botao3, INPUT_PULLUP);
    pinMode(botao4, INPUT_PULLUP);
    pinMode(botaoreset, INPUT_PULLUP);
  
	pinMode(led1,OUTPUT);
 	pinMode(led2,OUTPUT);
 	pinMode(led3,OUTPUT);
 	pinMode(led4,OUTPUT);

  pinMode(buzzer,OUTPUT);
 	
  reset();
}

void reset(){
  
  
  for(int i =0; i < 4;i++){
  digitalWrite(leds[fase][i],HIGH); //HIGH is set to about 5V PIN8
  delay(300);               //Set the delay time, 1000 = 1S
  digitalWrite(leds[fase][i],LOW);  //LOW is set to about 5V PIN8
  delay(300);
  };
}

void inputuser(){
  if (digitalRead(botao1) == LOW){
  	player[contbotao]=7;
    digitalWrite(led1,HIGH);
    contbotao = contbotao + 1;
    delay(200);
    digitalWrite(led1,LOW);
    delay(200);
  };
  if (digitalRead(botao2) == LOW){
  	player[contbotao]=6;
    digitalWrite(led2,HIGH);
    contbotao = contbotao + 1;
    delay(200);
    digitalWrite(led2,LOW);
    delay(200);
  };
  if (digitalRead(botao3) == LOW){
  	player[contbotao]=5;
    digitalWrite(led3,HIGH);
    contbotao = contbotao + 1;
    delay(200);
    digitalWrite(led3,LOW);
    delay(200);
  };
  if (digitalRead(botao4) == LOW){
  	player[contbotao]=4;
    digitalWrite(led4,HIGH);
    contbotao = contbotao + 1;
    delay(200);
    digitalWrite(led4,LOW);
    delay(200);
  };
  if (digitalRead(botaoreset) == LOW){
    fase = 0;
    acabou();
    reset();
    delay(200);
  };
}
void acabou(){
	contbotao = 0;
  for(int i = 0; i<4;i++){
  	player[i]=0;
  }
};

void musicavitoria(){
  int tempo = 400;
  tone(buzzer,440,tempo); 
  delay(tempo);
  tone(buzzer,294,tempo); 
  delay(tempo);
  tone(buzzer,349,tempo/2); 
  delay(tempo/2);
  tone(buzzer,392,tempo/2); 
  delay(tempo/2);
  tone(buzzer,440,tempo); 
  delay(tempo);
  tone(buzzer,294,tempo); 
  delay(tempo);
  tone(buzzer,349,tempo/2); 
  delay(tempo/2);
  tone(buzzer,392,tempo/2); 
  delay(tempo/2);
  tone(buzzer,330,tempo); 
};

void musicaderrota(){
  tone(buzzer,349,300);
};

void loop()
{
  inputuser();
  if(player[0] != 0 && player[1] != 0 && player[2] != 0 && player[3] != 0){
    if(player[0] == leds[fase][0] && player[1] == leds[fase][1] && player[2] == leds[fase][2] && player[3] == leds[fase][3]){
     Serial.println("foi bem");
      fase = fase+1;
        if(fase < 4){
       	acabou();
      	reset();
        } else if(fase == 4){
          acabou();
          musicavitoria();
          Serial.println("Acabou o jogo!");
        };

    } else{ 
    	Serial.println("mamou");
      fase = 0;
      acabou();
      musicaderrota();
      reset();
    };
  };
}

