<h2>esp8266 stack dump 보는 법</h2>
<p>이유를 알수 없이 esp8266 Arduino 프로그램이 다운되거나 리부팅이 될때, 원인분석을 하기 위해서는 esp8266의 stack 정보를 분석하여야 한다. 이를 위해 다음과 같은 절차를 수행하면 된다</p> 
<ol> 
    <li>setup()에 Serial.begins()를 넣어, stack dump가 일어 날때, serial port로 프린트하게 만든다.</li>
    <li>보드를 기동시키고, PIO의 status bar에 있는 Terminal Panle을 열어 다음과 같은 명령을 수행한다.</li>
    <pre>
    pio device monitor -f esp8266_exception_decoder
    </pre>
    <li>보드를 멈추거나 pio device mointor 프로세스를 멈추고, 출력된 내용을 분석한다.</li>
</ol>
<h2>Example</h2>
<p>이 분석과정을 연습하게 하기 위해, 이 repo는 2개의 crash example을 제공한다.</p>
<ul>
    <li>src/main.cpp</li>
    <li>wifi/main.cpp</li>
</ul>
<h3>src/main.cpp</h3>
<ol>
    <li>src/main.cpp example 사용은, 이 repo를 받아 빌드해서 run 하면 dump를 내면서 계속 리부팅을 할것이다.</li>
    <li>PIO의 status bar에 있는 Terminal Panle을 열어 다음과 같은 명령을 수행한다.</li>
        <pre>
        pio device monitor -f esp8266_exception_decoder
        </pre>
    <li>보드를 멈추거나 pio device mointor 프로세스를 멈추고, 출력된 내용을 살펴본다.</li>
    
![Untitled](https://user-images.githubusercontent.com/13171662/133095791-ff041467-8ab5-4b25-9789-5d9b50b3a16f.jpg)
    <li>필요한 경우, 위 사진처럼, 마우스 포인터를 올리고 Cmd + Click 혹은 Ctrl + Click 하여 해당 source 코드로 이동하여 분석한다.</li>
</ol>

<h3>wifi/main.cpp</h3>
<ol>
    <li>wifi/main.cpp 을 src/main.cpp로 교체하고, 빌드해서 run 한다</li>
    <li>같은 무선 네트워크에 붙어있는 노트북이나 스마트폰의 브라우저로 http://esp8266.local/ 를 연다</li>
    <li>dump르 내보내며 보드가 리부트 할것이다</li>
    <li>PIO의 status bar에 있는 Terminal Panle을 열어 다음과 같은 명령을 수행한다.</li>
        <pre>
        pio device monitor -f esp8266_exception_decoder
        </pre>
    <li>보드를 멈추거나 pio device mointor 프로세스를 멈추고, 출력된 내용을 살펴본다.</li>
    <li>더 분석하고자 하는 파일에 마우스 포인터를 올리고 Cmd + Click 혹은 Ctrl + Click 하여 해당 source 코드로 이동하여 분석한다.</li>
</ol>
<hr/>

  [![PlatformIO/VSCode에서 esp8266의 Exception / Stack dump 보는 법](https://user-images.githubusercontent.com/13171662/135889891-ef5160a4-3bfc-430f-8a15-8774ee0d9835.png)](https://youtu.be/5bzSeuSBMBA "PlatformIO/VSCode에서 esp8266의 Exception / Stack dump 보는 법")


### 주의사항
#### platformio.ini에 [env:xxxxx] 는 하나만 넣는다. 

가령 [env:env:release]와 [env:debug] 이렇게 두개의 section을 만들면, PIO는 이 env:xxxxx 각각에 대해 platformio.ini에 있는 순서대로 빌드하고 업로드를 하는데

1. 최종의 env:xxxxx의 바이너리가 보드에서 돌게 되며, 
2. 이 monitor filter인 filter_exception_decoder.py는 platormio.ini에서 처음 나오는 env:xxxxx를 prog_path에 가지고 있어 addr2line 수행에 필요한 정보가 없는 바이너리를 포인팅한다. 그래서 원하는 소스코드명과 라인을 알수 없다.
