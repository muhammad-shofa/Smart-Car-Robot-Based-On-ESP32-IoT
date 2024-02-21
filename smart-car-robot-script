#include <WiFi.h>
#include <AsyncTCP.h>
#include <ESPAsyncWebServer.h>

const char* ssid = ""; // your wifi ssid
const char* password = ""; // your wifi password

int ENA_pin = 25;
int ENB_pin = 14;
int IN1 = 26;
int IN2 = 27;
int IN3 = 32;
int IN4 = 33;

const int frequency = 500;
const int pwm_channel = 0;
const int resolution = 8;

int currentSpeed = 0;

AsyncWebServer server(80);

const char index_html[] PROGMEM = R"rawliteral(
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Remot Control Motor DC</title>
    <style>
      html {
        font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
          Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
          sans-serif;
        text-align: center;
      }
      h2 {
        font-size: 23px;
      }
      p {
        font-size: 16px;
      }
      body {
        width: 100%;
        height: 100vh;
      }

      input,
      label {
        width: 80px;
        padding: 7px 12px;
        margin: 5px;
        border-radius: 5px;
        outline: none;
        border: 2px solid #f2f2f2;
        color: #ffffff;
        background-color: #000000;
      }

      input:hover,
      label:hover {
        cursor: pointer;
        background-color: #65b741;
      }

      input:active {
        background-color: #65b741;
      }

      input[type="radio"] {
        display: none;
      }

      input[type="radio"]:checked ~ label {
        background-color: #65b741;
      }

      .container-speed-btn {
        display: flex;
        justify-content: center;
      }

      .center-direction-btn {
        display: flex;
        justify-content: center;
      }
    </style>
  </head>
  <body>
    <h2>Speed</h2>
    <div class="container-speed-btn">
      <p>
        <input
          type="radio"
          onclick="speed('low')"
          value="Low"
          id="low-btn"
          name="speed"
        />
        <label for="low-btn">Low</label>
      </p>
      <p>
        <input
          type="radio"
          onclick="speed('mid')"
          value="Mid"
          id="mid-btn"
          name="speed"
        />
        <label for="mid-btn">Mid</label>
      </p>
      <p>
        <input
          type="radio"
          onclick="speed('high')"
          value="High"
          id="high-btn"
          name="speed"
        />
        <label for="high-btn">High</label>
      </p>
    </div>
    <h2>Direction</h2>
    <p>
      <input type="button" onclick="move('forward')" value="Forward" />
    </p>
    <div class="center-direction-btn">
      <p>
        <input type="button" onclick="move('left')" value="Left" />
      </p>
      <p>
        <input type="button" onclick="move('stop')" value="Stop" />
      </p>
      <p>
        <input type="button" onclick="move('right')" value="Right" />
      </p>
    </div>
    <p>
      <input type="button" onclick="move('backward')" value="Backward" />
    </p>
    <script>
      // select speed func
      function speed(level) {
        var xhr = new XMLHttpRequest();
        xhr.open("GET", "/speed?level=" + level, true);
        xhr.send();
      }

      // direction
      function move(direction) {
        var xhr = new XMLHttpRequest();
        xhr.open("GET", "/move?direction=" + direction, true);
        xhr.send();
      }

      // stop
      function stop() {
        var xhr = new XMLHttpRequest();
        xhr.open("GET", "/move?direction=stop", true);
        xhr.send();
      }
    </script>
  </body>
</html>
)rawliteral";

void setSpeed(int speedLevel) {
  currentSpeed = speedLevel;
  ledcWrite(pwm_channel, map(currentSpeed, 0, 100, 0, 255));
}

void setup(){
  Serial.begin(115200);
  delay(1000);
  pinMode(ENA_pin, OUTPUT);
  pinMode(ENB_pin, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  ledcSetup(pwm_channel, frequency, resolution);
  ledcAttachPin(ENA_pin, pwm_channel);
  ledcAttachPin(ENB_pin, pwm_channel);
  ledcWrite(pwm_channel, 0);
  
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);

  connectWiFi();
  
    server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send_P(200, "text/html", index_html);
  });
  
  // speed setting
server.on("/speed", HTTP_GET, [] (AsyncWebServerRequest *request) {
  String level;
  if (request->hasParam("level")) {
    level = request->getParam("level")->value();
    if (level == "low") {
      setSpeed(215); // Sesuaikan nilai kecepatan rendah
    } else if (level == "mid") {
      setSpeed(235); // Sesuaikan nilai kecepatan sedang
    } else if (level == "high") {
      setSpeed(255); // Sesuaikan nilai kecepatan tinggi
    }
  }
  request->send(200, "text/plain", "OK");
});



  // direction setting
  server.on("/move", HTTP_GET, [] (AsyncWebServerRequest *request) {
    String direction;
    if (request->hasParam("direction")) {
      direction = request->getParam("direction")->value();
      if (direction == "forward") {
        digitalWrite(IN1, HIGH);
        digitalWrite(IN2, LOW);
        digitalWrite(IN3, HIGH);
        digitalWrite(IN4, LOW);
      } else if (direction == "backward") {
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, HIGH);
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, HIGH);
      } else if (direction == "left") {

        digitalWrite(IN1, HIGH);
        digitalWrite(IN2, LOW);
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, HIGH);
      } else if (direction == "right") {
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, HIGH);
        digitalWrite(IN3, HIGH);
        digitalWrite(IN4, LOW);
      } else if (direction == "stop") {
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, LOW);
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, LOW);
      }
    }
    request->send(200, "text/plain", "OK");
  });
  
  server.begin();
}

void loop() {
  
}

void connectWiFi() {
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting...");
  }

  Serial.println(WiFi.localIP());
}
