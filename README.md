//Smart Cleaning Bot (Autoamtic Vacuum Cleaner)

int L_IR=0;
int C_IR=0;
int R_IR=0;

// inches_L = 0;
int cm_L = 0;
int cm_C = 0;
int cm_R = 0;

long readUltrasonicDistance(int triggerPin, int echoPin)
  {
  pinMode(triggerPin, OUTPUT);  // Clear the trigger
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  // Sets the trigger pin to HIGH state for 10 microseconds
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  // Reads the echo pin, and returns the sound wave travel time in microseconds
  return pulseIn(echoPin, HIGH);
}

void setup()
{
  pinMode(10,OUTPUT);   //Left Motor A   //  out 4  //in4
  pinMode(11,OUTPUT);    //Left Motor B   // out 3  // in3
  pinMode(12,OUTPUT);    //Right Motor A  //  out 2 // in2   
  pinMode(13,OUTPUT);    //Right Motor B  // out1 // in1
 
  pinMode(7,OUTPUT);   // Left IR
  pinMode(8,OUTPUT);   // Center IR
  pinMode(9,OUTPUT);   // Right IR
  
  pinMode(3,OUTPUT); // Enable A
  pinMode(5,OUTPUT);  //Enable B
  analogWrite(3, 150);
  analogWrite(5, 150);

  pinMode(2,OUTPUT);  //Left Ultra Sonic Trigger
  pinMode(A0,OUTPUT);  //Center Ultra Sonic Trigger
  pinMode(A1,OUTPUT);  //Right Ultra Sonic Trigger
  
  pinMode(1,INPUT);  //Left Ultra Sonic Echo
  pinMode(4,INPUT);  //Center Ultra Sonic Echo
  pinMode(6,INPUT);  //Right Ultra Sonic Echo


  
  
  //Serial.begin(9600);
}

void loop()
{

  
cm_L = 0.01723 * readUltrasonicDistance(2, 1);
 cm_C = 0.01723 * readUltrasonicDistance(A0, 4);
 cm_R = 0.01723 * readUltrasonicDistance(A1, 6);
  
  
  L_IR= digitalRead(7);
  C_IR= digitalRead(8);
  R_IR= digitalRead(9);
  
  if ((L_IR==1)&&(C_IR==1)&&(R_IR==1))
      {
        forward();
      }
      
  if ((L_IR==1)&&(C_IR==0)&&(R_IR==1))
      {
         stop();
         backward();
         right();
      }
      
 if ((L_IR==0)&&(C_IR==1)&&(R_IR==1))
     
     {
        stop();
        delay(1000);
         forward();
        }
  
    
 if ((L_IR==1)&&(C_IR==1)&&(R_IR==0))
     
     {
        stop();
        delay(1000);
        forward();
     }
      
      
      
 if((cm_L<=10)&& (cm_C>10)&& (cm_R>10))
    {
    stop();
    right();
    }
    
    if((cm_L>10)&& (cm_C>10)&& (cm_R<=10))
    {
    stop();
    left();
    }
   
    if((cm_L>10)&& (cm_C>10)&& (cm_R>10))
    {
    forward();
    }
       
    if((cm_L>10)&& (cm_C<=10)&& (cm_R>10))
    {
      stop();
      backward();
      stop();
      right();
    }
         
       
  
}

void forward()
{
digitalWrite(10, HIGH);
digitalWrite(11, LOW);
digitalWrite(12, HIGH);
digitalWrite(13, LOW);
}


void backward()
{
digitalWrite(10, LOW);
digitalWrite(11, HIGH);
digitalWrite(12, LOW);
digitalWrite(13, HIGH);
}

void left()
{
digitalWrite(10, LOW);
digitalWrite(11, HIGH);
digitalWrite(12, HIGH);
digitalWrite(13, LOW);
}


void right()
{
digitalWrite(10, HIGH);
digitalWrite(11, LOW);
digitalWrite(12, LOW);
digitalWrite(13, HIGH);
}


void stop()
{

digitalWrite(10, LOW);
digitalWrite(11, LOW);
digitalWrite(12, LOW);
digitalWrite(13, LOW);

}
