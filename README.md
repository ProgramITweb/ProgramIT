<!DOCTYPE html>
<html>
<head>
     This is ProgramIT
</head>
<body>
    <h1>ESP8266 webservercode!</h1><br>
    <p>Hi this is the image of code for ESP8266 webserver!</p><br><br>
    <a href="http://www.youtube.com/@ProgramIT_code">ProgramIT YT Channel</a><br><br>
    <img src="Screenshot (11).jpg" alt="Arduino IDE image" width="500" height="500">
    <img src="Screenshot (12).jpg" alt="Arduino IDE image2" width="500" height="500"><br>
    <img src="Screenshot (13).jpg" alt="Arduino IDE image3" width="500" height="500">
    <img src="Screenshot (14).jpg" alt="Arduino IDE image4" width="500" height="500"><br>
     <br><h2> To install ESP8266 , Open your Arduino IDE.<br>Go to File > Preferences.<br>In the “Additional Boards Manager URLs” field,<br> enter this URL: http://arduino.esp8266.com/stable/package_esp8266com_index.json.
     <br>
     Click OK.<br>
     Now, open the Boards Manager by going to Tools > Board > Boards Manager….<br>
     Search for ESP8266 and click the Install button for the “ESP8266 by ESP8266 Community.”<br>
     Wait a few seconds for the installation to complete<br>
     <br>
     </h2>
     <h3>This is th the code :</h3><br><br>

                     
        #include <ESP8266WiFi.h>
        #include <WiFiClient.h>
        #include <ESP8266WebServer.h>
                        
        #ifndef STASSID
        #define STASSID "your ssid"
        #define STAPSK "your password"
        #endif
                    
        const char* ssid = STASSID;
        const char* password = STAPSK;
                   
        ESP8266WebServer server(your port common is 80);
                    
        // Check if header is present and correct
        bool is_authenticated() {
        Serial.println("Enter is_authenticated");
        if (server.hasHeader("Cookie")) {
           Serial.print("Found cookie: ");
           String cookie = server.header("Cookie");
           Serial.println(cookie);
        if (cookie.indexOf("ESPSESSIONID=1") != -1) {
           Serial.println("Authentication Successful");
        return true;
      }
    }
      Serial.println("Authentication Failed");
      return false;
    }
                  
    // login page, also called for disconnect
    void handleLogin() {
    String msg;
    if (server.hasHeader("Cookie")) {
       Serial.print("Found cookie: ");
       String cookie = server.header("Cookie");
       Serial.println(cookie);
    }
    if (server.hasArg("DISCONNECT")) {
       Serial.println("Disconnection");
       server.sendHeader("Location", "/login");
       server.sendHeader("Cache-Control", "no-cache");
       server.sendHeader("Set-Cookie", "ESPSESSIONID=0");
       server.send(301);
     return;
    }
    if (server.hasArg("USERNAME") && server.hasArg("PASSWORD")) {
      if (server.arg("USERNAME") == "Good" && server.arg("PASSWORD") == "101202") {
         server.sendHeader("Location", "/");
         server.sendHeader("Cache-Control", "no-cache");
         server.sendHeader("Set-Cookie", "ESPSESSIONID=1");
         server.send(301);
         Serial.println("Log in Successful");
         return;
      }
     msg = "Wrong username/password! try again.";
     Serial.println("Log in Failed");
    }
     String content = "<html><body><form action='/login' method='POST'>To log in, please use : admin/admin<br>";
     content += "User:<input type='text' name='USERNAME' placeholder='user name'><br>";
     content += "Password:<input type='password' name='PASSWORD' placeholder='password'><br>";
     content += "<input type='submit' name='SUBMIT' value='Submit'></form>" + msg + "<br>";
     content += "<h9> </h9>"
     content += "You also can go <a href='/inline'>here</a></body></html>";
     server.send(200, "text/html", content);
    }
             
    // root page can be accessed only if authentication is ok
    void handleRoot() {
       Serial.println("Enter handleRoot");
    String header;
    if (!is_authenticated()) {
       server.sendHeader("Location", "/login");
       server.sendHeader("Cache-Control", "no-cache");
       server.send(301);
       return;
    }
    String content = "<html><body><H2>hello, you successfully connected to esp8266!</H2><br><p1>Hi I am Hari and welcome to my 
    own website</p1><br><br><br><p2>Now visit my YT Channel the link is =</p2><br><br><br><br><br>";
    if (server.hasHeader("User-Agent")) { content += "the user agent used is : " + server.header("User-Agent") + "<br><br>"; }
    content += "You can access this page until you <a href=\"/login?DISCONNECT=YES\">disconnect</a><br><br><p1>Hi i will host 
    verios python programs in this webServer</p1></body></html>";
    server.send(200, "text/html", content);
    }
        
    // no need authentication
    void handleNotFound() {
      String message = "File Not Found\n\n";
      message += "URI: ";
      message += server.uri();
      message += "\nMethod: ";
      message += (server.method() == HTTP_GET) ? "GET" : "POST";
      message += "\nArguments: ";
      message += server.args();
      message += "\n";
      for (uint8_t i = 0; i < server.args(); i++) { message += " " + server.argName(i) + ": " + server.arg(i) + "\n"; }
      server.send(404, "text/plain", message);
    }
            
    void setup(void) {
         
       Serial.begin(2000000);
       WiFi.mode(WIFI_STA);
       WiFi.begin(ssid, password);
       Serial.println("");
                    
       // Wait for connection
        while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
     }
    Serial.println("");
    Serial.print("Connected to ");
    Serial.println(ssid);
    Serial.print("IP address: ");
    Serial.println(WiFi.localIP());
         
          
    server.on("/", handleRoot);
    server.on("/login", handleLogin);
    server.on("/inline", []() {
    server.send(200, "text/plain", "this 101202 without need of authentication");
    });
          
    server.onNotFound(handleNotFound);
    // ask server to track these headers
    server.collectHeaders("User-Agent", "Cookie");
    server.begin();
    Serial.println("HTTP server started");
    }  

    void loop(void) {
    server.handleClient();
      
    }

<br><br>
Edit it:<br>

                         ESP8266WebServer server(your port common is 80);

 <br>
 replase the   [ your port common is 80 ] to your port example<br>
 [ "8080" , "80" , "443" or any 4 digit number ]<br>
 <br>
 <br>
 
 like:


                          ESP8266WebServer server(80);



<br><br>
and open the serial monitor if the board is connected it will display<br>
something like this:<br><br>
 <img src="Screenshot (15).jpg" alt="Serial monitor" width="500" height="242"><br><br><br>

 
</body>
 </html>
