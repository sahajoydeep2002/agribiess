
#include <SimpleDHT.h>
#include <Servo.h>
int pinDHT11 = 3;
SimpleDHT11 dht11(pinDHT11);

Servo myservo;
int sensorValue,ldrValue,moisture;
int a,b,z;
   byte temperature = 0;
  byte humidity = 0; 
void setup() {
  

  int err = SimpleDHTErrSuccess;
  if ((err = dht11.read(&temperature, &humidity, NULL)) != SimpleDHTErrSuccess) {
    Serial.print("Read DHT11 failed, err="); Serial.println(err);delay(1000);
    return;
  }
  Serial.begin(9600);
  pinMode(2,INPUT);
  pinMode(4,INPUT);
 pinMode(10,OUTPUT);
 pinMode(11,OUTPUT); 
 pinMode(12,OUTPUT);
 pinMode(8,OUTPUT);
 myservo.attach(9);
 digitalWrite(10,HIGH);
 digitalWrite(11,HIGH);
 digitalWrite(12,HIGH);
 digitalWrite(8,HIGH);
}

void loop() {
  String cmd = "";
  int s1,s2;
  s1=digitalRead(2);
  
s2=digitalRead(4);
if(s1==0 && a==1) 
{
  myservo.write(90);
  a=0;
}
else if (s2==0 && a==2)
{ myservo.write(90);
a=0;
}

if (Serial.available() > 0) {
    cmd = Serial.readString(); 
     
     
    if (cmd.equals("1"))
    {a=1;
      if(s1==1){
                  myservo.write(180);
                }

    }
    else if (cmd.equals("0"))
    {
     a=2;
 if(s2==1)
{
myservo.write(0);
}

    }
     else if (cmd.equals("3")){
      digitalWrite(10,LOW);
      
    }
    else if (cmd.equals("4")){
      digitalWrite(10,HIGH);
      
    }
    else if (cmd.equals("5")){
      digitalWrite(11,LOW);
      
    }
    else if (cmd.equals("6")){
      
      digitalWrite(11,HIGH);
    }
    else if (cmd.equals("7")){
        digitalWrite(12,LOW);
      
    }
    else if (cmd.equals("8")){
      
      digitalWrite(12,HIGH);
    }
    else if (cmd.equals("9")){
     digitalWrite(8,LOW);
      
    }
    else if (cmd.equals("10")){

      digitalWrite(8,HIGH);
    }
    else if (cmd.equals("11")){

      z=1;
    }
    else if (cmd.equals("12")){

      z=0;
    }
  }
sensorValue = analogRead(A1);
sensorValue=((double)sensorValue/1024)*100;
ldrValue=analogRead(A0);
ldrValue=((double)ldrValue/1024)*100;
moisture=100-sensorValue;
Serial.print("Moisture=");
  Serial.print(moisture);
  Serial.print("% , ");
  Serial.print("Temperature=");
  Serial.print((int)temperature);
  Serial.print("C , ");
  Serial.print("Humidity=");
  Serial.print((int)humidity);
  Serial.print("% , ");
  Serial.print("Sunlight=");
  Serial.print(ldrValue);
  Serial.print("%");
  Serial.println();
  delay(250);


if(z==1)
{

 //pumpon

if(moisture<40)
{
  digitalWrite(10,LOW);
}
else if (moisture>45)
{
  digitalWrite(10,HIGH);
}
 

//Lights on

if(ldrValue<30)
{
  digitalWrite(8,LOW);
}
else if(ldrValue>30)
{
  digitalWrite(8,HIGH);
}


//HEATER ON

if((int)temperature<26)
{
  digitalWrite(12,LOW);
}
else if((int)temperature>26)
{
  digitalWrite(12,HIGH);
}

//Fan on

if((int)temperature>=32)
{
  digitalWrite(11,LOW);
}
else if((int)temperature<29)
{
  digitalWrite(11,HIGH);
}



//high temp roof on

if(s2==1 and ldrValue<=85 and (int)temperature>28 and moisture<45)
{  
 
  myservo.write(0);
  b=4;
  
}

else if(s2==0 && b==4)
{ myservo.write(90);
  b=0;
}

//high sunlight roof off

if(s1==1 and ldrValue>85)
{  
 
  myservo.write(180);
  b=2;
  
}

else if(s1==0 && b==2)
{ myservo.write(90);
  b=0;
}

//Low temperature roof off

if(s1==1 and (int)temperature<28)
{  
 
  myservo.write(180);
  b=3;
  
}

else if(s1==0 && b==3)
{ myservo.write(90);
  b=0;
}

//Rain roof off
if(s1==1 and moisture>62 and (digitalRead(10))==1)
{  
 
  myservo.write(180);
  b=5;
  
}

else if(s1==0 && b==5)
{ myservo.write(90);
  b=0;
}
}

}
