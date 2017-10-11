#include <WiFi.h>

// WiFi network name and password:
const char * networkName = "CBCI-6B65-2.4";
const char * networkPswd = "chores6054apple";

char * MakerIFTTT_Event = "Josh_ESP32_Button";
char * MakerIFTTT_Key = "dSzusF7Dp8WaSF1MkQKYWqNTl6erY_lL5wV2l-faLPr"; // Webhooks settings key
char * hostDomain = "maker.ifttt.com";
const int hostPort = 80;

const int BUTTON_PIN = 15;
const int LED_PIN = 5;

void setup()
{
  // Initilize hardware:
  Serial.begin(115200);
  pinMode(BUTTON_PIN, INPUT_2PULLUP);
  pinMode(LED_PIN, OUTPUT);

  // Connect to the WiFi network (see function below loop)
  connectToWiFi(networkName, networkPswd);

  digitalWrite(LED_PIN, LOW); // LED off
  Serial.println("Press button 0 to send a text ");

  sendSms();

}

void loop()
{
  if (digitalRead(BUTTON_PIN) == LOW)

  { // Check if button has been pressed
    while (digitalRead(BUTTON_PIN) == LOW)
      ; // Wait for button to be released
    Serial.println(" button pressed ");
    digitalWrite(LED_PIN, HIGH); // Turn on LED

    // send text message
    sendSms();

    digitalWrite(LED_PIN, LOW); // Turn off LED
  }
}

void connectToWiFi(const char * ssid, const char * pwd)
{
  delay(1000);
  int ledState = 0;

  printLine();
  Serial.println("Connecting to WiFi network: " + String(ssid));

  WiFi.begin(ssid, pwd);

  while (WiFi.status() != WL_CONNECTED)
  {
    // Blink LED while we’re connecting:
    digitalWrite(LED_PIN, ledState);
    ledState = (ledState + 1) % 2; // Flip ledState
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("WiFi connected!");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
}

void printLine()
{
  Serial.println();
  for (int i = 0; i < 30; i++)
    Serial.print("-");
  Serial.println();
}



void sendSms() {

  WiFiClient client;
  client.stop();

  if (client.connect(hostDomain, hostPort)) {

    String PostData = "{\"value1\" : \"Sparkfun ESP32 test!\"}";

    Serial.println("connected to server… Getting name…");
    // send the HTTP PUT request:
    String request = "POST /trigger/";
    request += MakerIFTTT_Event;
    request += "/with/key/";
    request += MakerIFTTT_Key;
    request += "HTTP/1.1";

    Serial.println(request);
    client.println(request);

    client.println("Host: " + String(hostDomain) );
    client.println("User-Agent: Energia/1.1");
    client.println("Connection: close");
    client.println("Content-Type: application/json");
    client.print("Content-Length: ");
    client.println(PostData.length());
    client.println();
    client.println(PostData);
    client.println();
  }
  else {
    // if you couldn’t make a connection:
    Serial.println("Connection failed");
    return;
  }

//  // Capture response from the server. (10 second timeout)
//  long timeOut = 10000;
//  long lastTime = millis();
//  Serial.println(lastTime);
//  //while ((millis() – lastTime) < timeOut) { // Wait for incoming response from server
//        while (client.available()) { // Characters incoming from the server
//          char c = client.read(); // Read characters
//          Serial.write(c);
//        }
  //}
  //Serial.println();
  //Serial.println("Request Complete!");

  return;

}
