#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#include <Adafruit_NeoPixel.h>
#define neopixel D2
#define led D4
IPAddress ip(172, 24, 1, 11);IPAddress gateway(172, 24, 1, 1);IPAddress subnet(255, 255, 255, 0);
WiFiClient client;
PubSubClient mqttclient(client);
const char* mqttServer="172.24.1.1";
const int mqttPort = 1883;
 
int serverack,packetsize;

const char* ssid = "OrangePi";
const char* password = "0963258741";

int k,trialwifi,trialmqtt;
unsigned long  millisecond=0, currentmillis=0, premillis=0,currentmicros=0,premicros=0,milli100ms,milli200ms,milli500ms,seconds,minutes,watchdogweb;
byte command=0,durum=0,Rout[64],Gout[64],Bout[64];
float brightness=0.5;
byte mqttsenddata[20];int mqttreceivedata[250];  String mqttreceivestr[250];
boolean tickms,ticks,tick500ms,tick500mms,tick200ms,tick100ms,tickm,tickh,sendserial,sendmqtt,cikisok,datareceived,flash,sendok,slide,startt,writeok,writeoutput,blinking,sweeper; 
boolean countup, countdown,allred,allgreen,allblue,allwhite,wificonnected,ledtoggle;  

int numPixels   = 64; // Popular NeoPixel ring size

Adafruit_NeoPixel pixels(numPixels, neopixel, NEO_GRB + NEO_KHZ800);


void MQTTreconnect() {  // Loop until we're reconnected
  mqttclient.setServer(mqttServer, mqttPort);  mqttclient.setCallback(mqttreceive);
  while (!mqttclient.connected() and trialmqtt<10) {   Serial.print("Attempting MQTT connection...");trialmqtt++;
                                                     if (mqttclient.connect("defneokul")) 
                                                          { Serial.println("connected");       mqttclient.publish("defnedurum", "HELLO WORLD");mqttclient.subscribe("defnecommand"); } 
                                                     else { Serial.print("failed, rc=");      Serial.print(mqttclient.state());           delay(200);    }  }}  
 

void WIFI_Connect(){   Serial.println("Booting Sketch...");  WiFi.mode(WIFI_STA);  WiFi.persistent(false); WiFi.config(ip, gateway, subnet);
WiFi.begin(ssid, password);    // Wait for connection
while (WiFi.status() != WL_CONNECTED and trialwifi<10) {delay(500);Serial.print(".");trialwifi++;yield();}Serial.println(""); Serial.print("Connected to: "); Serial.println(ssid); Serial.print("IP address: "); 
Serial.println(WiFi.localIP());Serial.println("");  wificonnected=true;}

void mqttsend(){ mqttsenddata[0]=durum; mqttsenddata[1]=byte(trunc(brightness*100));mqttsenddata[2]=2;mqttsenddata[3]=3;mqttsenddata[4]=4;  
    mqttsenddata[5]=5; mqttsenddata[6]=6; mqttsenddata[7]= 0;mqttsenddata[8]=0;mqttsenddata[9]=0;mqttsenddata[10]=0;
   
    mqttclient.publish("defnedurum", mqttsenddata,10);
    Serial.print("Sending Message:");  for (int i = 0; i < 10; i++) {    Serial.print(mqttsenddata[i]);Serial.print(", ");}
    Serial.println();  Serial.println("-----------------------");
    }
    
void mqttreceive(char* topic, byte* payload, unsigned int length) {int j=-1;  for (int i=0;i<200; i++) {mqttreceivestr[i]=' ';} 
   Serial.print("Receiving Message:");  for (int i = 0; i < length; i++) {    Serial.print(char(payload[i]));
  if ((char(payload[i])!='[') and (char(payload[i])!=',') and (char(payload[i])!=']')) {mqttreceivestr[j]=mqttreceivestr[j]+String(char(payload[i]));} else j=j+1;}  Serial.println();  Serial.println("-----------------------");
  for (int i=0;i<200; i++) {mqttreceivedata[i]=lowByte(mqttreceivestr[i].toInt());}yield();
  
  command=(command xor mqttreceivedata[0]);
  if (bitRead(mqttreceivedata[0],2) or bitRead(mqttreceivedata[0],3)) k=0;
  if (bitRead(mqttreceivedata[0],4)) command=command&0x1F;
  if (bitRead(mqttreceivedata[0],5)) command=command&0x2F;
  if (bitRead(mqttreceivedata[0],6)) command=command&0x4F;
  if (bitRead(mqttreceivedata[0],7)) command=command&0x8F;
  if((mqttreceivedata[193]>0) and (mqttreceivedata[193]<100)) {brightness=float(mqttreceivedata[193])/100;} else {brightness=1;};
   writeok=false;watchdogweb=0; datareceived=true;sendmqtt=false; ledtoggle=!ledtoggle; digitalWrite(led,ledtoggle);}
  

void pixelshow(){ pixels.clear();if (startt){ if (!blinking) {for (int i=0; i<64; i++) {pixels.setPixelColor(i, pixels.Color(Rout[i],Gout[i],Bout[i])); }}
                                             else {if (tick500mms) { for (int i=0; i<64; i++) {pixels.setPixelColor(i, pixels.Color(Rout[i],Gout[i],Bout[i]));}} else pixels.clear();     }}
                                 if (sweeper){ pixels.clear();  pixels.setPixelColor(k, pixels.Color(Rout[k],Gout[k],Bout[k]));}
                                 if (slide){pixels.clear(); for (int i=0; i<8; i++) { pixels.setPixelColor(i*8+k, pixels.Color(Rout[i*8+k],Gout[i*8+k],Bout[i*8+k]));} }
                                pixels.show(); }

void setup() {pinMode(neopixel,OUTPUT);pinMode(led,OUTPUT);
Serial.begin(115200); //Start serial communication at 115200 baud
Serial.print("Starting");
 pixels.begin();
 WIFI_Connect();
 MQTTreconnect();
}
void loop() { currentmillis=millis();
 
 if ((currentmillis-premillis)>=10) {millisecond+=10;milli200ms+=10;milli500ms+=10; tickms=!tickms; premillis=currentmillis;watchdogweb+=10;} 
 if (milli100ms>=100){tick100ms=true;milli100ms=0; }
 if (milli200ms>=200){tick200ms=true;milli200ms=0; }
 if (milli500ms>=500){tick500ms=true;tick500mms=!tick500mms;milli500ms=0; }
 if (millisecond>=1000){ millisecond=0; seconds+=1; ticks=!ticks;sendserial=false; cikisok=false;sendmqtt=false; writeok=false; }
 if (seconds>=60) {seconds=0; minutes+=1; tickm=true;      }
 if (minutes>=60) {minutes=0; tickh=!tickh;} 
  mqttclient.loop();

   
 if (!writeok){ writeok=true; startt=bitRead(command,0);blinking=bitRead(command,1);slide=bitRead(command,2);sweeper=bitRead(command,3);allred=bitRead(command,4);
                allgreen=bitRead(command,5);allblue=bitRead(command,6);allwhite=bitRead(command,7); durum=command; }
                   
                                                                             
 if(!sendmqtt ){ sendmqtt=true; mqttsend();datareceived=false;}                                 
 
 if(tick500ms ){tick500ms=false;  if(sweeper) {if (k<64 and countup) {k++;} 
                                                  if (k==64) countup=false; if (k==0) countup=true;
                                                  if (k>0 and !countup) { k--; }  }
                                  if(slide) {if (k<8 and countup) {k++;} 
                                                  if (k==8) countup=false; if (k==0) countup=true;
                                                  if (k>0 and !countup) { k--; }  }
                                                  Serial.print("k: "); Serial.println(k);}
                                 

 if (tickm){tickm=false; Serial.println("Checking WiFi"); if (WiFi.status() != WL_CONNECTED){ wificonnected=false; trialwifi=0; trialmqtt=0;WIFI_Connect();} yield();
                                                          if (!mqttclient.connected()and wificonnected) {    MQTTreconnect();  }}
 
//  if((watchdogweb>60000) or (command1&0x80)){Serial.print(command1);Serial.print("  Resetleniyor:"); ESP.restart();}

 if (tick200ms){tick200ms=false; 
                if (startt and !allred and !allgreen and !allblue and !allwhite){for (int i=0;i<64; i++) {Rout[i]=byte(trunc(mqttreceivedata[i*3+1]*brightness));Gout[i]=byte(trunc(mqttreceivedata[i*3+2]*brightness));Bout[i]=byte(trunc(mqttreceivedata[i*3+3]*brightness));}} 
                if (startt and allred and !allgreen and !allblue and !allwhite) {for (int i=0;i<64; i++) {Rout[i]=byte(trunc(128*brightness));Gout[i]=0;Bout[i]=0;}}   
                if (startt and !allred and allgreen and !allblue and !allwhite) {for (int i=0;i<64; i++) {Rout[i]=0;Gout[i]=byte(trunc(128*brightness));Bout[i]=0;}}
                if (startt and !allred and !allgreen and allblue and !allwhite) {for (int i=0;i<64; i++) {Rout[i]=0;Gout[i]=0;Bout[i]=byte(trunc(128*brightness));}}
                if (startt and !allred and !allgreen and !allblue and allwhite) {for (int i=0;i<64; i++) {Rout[i]=byte(trunc(128*brightness));Gout[i]=byte(trunc(128*brightness));Bout[i]=byte(trunc(128*brightness));}}
                if (!startt) {for (int i=0;i<64; i++) {Rout[i]=0;Gout[i]=0;Bout[i]=0;}}
 pixelshow();}
 
  yield();
 }
 
 
