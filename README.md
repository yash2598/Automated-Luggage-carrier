#define START_CMD_CHAR '>'
#define END_CMD_CHAR '\n'
#define DIV_CMD_CHAR ','

#define DEBUG 1 // Set to 0 if you don't want serial output of sensor data

String inText;
float value0, value1, value2;
int x=0;
int y=0;
int ledPin = 13;
void setup()
{pinMode(13,OUTPUT);   //left motors forward
pinMode(12,OUTPUT);   //left motors reverse
pinMode(11,OUTPUT);   //right motors forward
pinMode(10,OUTPUT);   //right motors reverse
pinMode(9,OUTPUT);   //Led

  Serial.begin(9600);
  Serial.println("\nSensoDuino 0.13 by TechBitar.com (2013).\n");
  Serial.println("Android Sensor Type No: ");
  Serial.println("1- ACCELEROMETER  (m/s^2 - X,Y,Z)");
  Serial.println("2- MAGNETIC_FIELD (uT - X,Y,Z)");
  Serial.println("3- ORIENTATION (Yaw, Pitch, Roll)");
  Serial.println("4- GYROSCOPE (rad/sec - X,Y,Z)");
  Serial.println("5- LIGHT (SI lux)");
  Serial.println("6- PRESSURE (hPa millibar)");
  Serial.println("7- DEVICE TEMPERATURE (C)");
  Serial.println("8- PROXIMITY (Centimeters or 1,0)");
  Serial.println("9- GRAVITY (m/s^2 - X,Y,Z)");
  Serial.println("10- LINEAR_ACCELERATION (m/s^2 - X,Y,Z)");
  Serial.println("11- ROTATION_VECTOR (Degrees - X,Y,Z)" );
  Serial.println("12- RELATIVE_HUMIDITY (%)");
  Serial.println("13- AMBIENT_TEMPERATURE (C)");
  Serial.println("14- MAGNETIC_FIELD_UNCALIBRATED (uT - X,Y,Z)");
  Serial.println("15- GAME_ROTATION_VECTOR (Degrees - X,Y,Z)");
  Serial.println("16- GYROSCOPE_UNCALIBRATED (rad/sec - X,Y,Z)");
  Serial.println("17- SIGNIFICANT_MOTION (1,0)");
  Serial.println("97 - AUDIO (Vol.)");
  Serial.println("98 - GPS1 (Lat., Long., Alt.)");
  Serial.println("99 - GPS2 (Bearing, Speed, Date/Time)");
  Serial.println("\n\nNOTE: IGNORE VALUES OF 99.99\n\n");
  Serial.flush();


}

void loop()
{
  Serial.flush();
  int inCommand = 0;
  int sensorType = 0;
  unsigned long logCount = 0L;

  char getChar = ' ';  //read serial

  // wait for incoming data
  if (Serial.available() < 1) return; // if serial empty, return to loop().

  // parse incoming command start flag
  getChar = Serial.read();
  if (getChar != START_CMD_CHAR) return; // if no command start flag, return to loop().

  // parse incoming pin# and value
  sensorType = Serial.parseInt(); // read sensor typr
  logCount = Serial.parseInt();  // read total logged sensor readings
  value0 = Serial.parseFloat();  // 1st sensor value
  value1 = Serial.parseFloat();  // 2nd sensor value if exists
  value2 = Serial.parseFloat();  // 3rd sensor value if exists


delay(252);
  x = Serial.parseFloat();  // 1st sensor value
  y = Serial.parseFloat();  // 2nd sensor value if exists

if(value1>10)
{
digitalWrite(12,LOW);
  digitalWrite(10,HIGH);
  digitalWrite(11,LOW);
  digitalWrite(13,HIGH);
  
   Serial.println("Forward");
  }

else if(value1<-20)
{
  
  digitalWrite(12,HIGH);
  digitalWrite(10,LOW);
  digitalWrite(11,HIGH);
  digitalWrite(13,LOW);
  
  
    Serial.println("REVErse");
}
else if(value2>15)
{
  Serial.println("left");
  digitalWrite(10,HIGH);
  digitalWrite(13,LOW);
  
    Serial.println("LEFT");
}
else if(value2<-20)
{
  Serial.println("right");
  digitalWrite(13,HIGH);
   digitalWrite(11,LOW);
    Serial.println("RIGHT");
}
else{
  Serial.println("stop");
  digitalWrite(13,LOW);
  digitalWrite(12,LOW);
  digitalWrite(11,LOW);
  digitalWrite(10,LOW);
  Serial.println("stop");
  
  }

if (DEBUG)
  {
    Serial.print("Sensor type: ");
    Serial.println(sensorType);
    Serial.print("Sensor log#: ");
    Serial.println(logCount);
    Serial.print("Val[0]: ");
    Serial.println(value0);
    Serial.print("Val[1]: ");
    Serial.println(value1);
    Serial.print("Val[2]: ");
    Serial.println(value2);
    Serial.println("-----------------------");
    delay(10);
  }
}
