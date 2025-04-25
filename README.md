<h1>🔁 Comunicação ESP32 via MQTT com Raspberry Pi (Broker)</h1>

<p>Este projeto conecta duas <strong>ESP32</strong> utilizando <strong>MQTT</strong> via uma <strong>Raspberry Pi como broker</strong>. Quando um botão é pressionado em uma ESP, o LED da outra acende. Isso permite comunicação bidirecional entre os grupos A e B.</p>

<hr>

<h2>📡 Tecnologias Utilizadas</h2>
<ul>
  <li><strong>ESP32</strong> (Placa de desenvolvimento)</li>
  <li><strong>Raspberry Pi</strong> (Broker MQTT com Mosquitto)</li>
  <li><strong>Protocolo MQTT</strong> (Leve, ideal para IoT)</li>
  <li><strong>Linguagem C++ (Arduino)</strong></li>
</ul>

<hr>

<h2>🧰 Componentes Necessários</h2>
<ul>
  <li>2x ESP32</li>
  <li>1x Raspberry Pi com Wi-Fi</li>
  <li>2x LEDs</li>
  <li>2x Resistores (220Ω)</li>
  <li>2x Botões</li>
  <li>Jumpers e Protoboard</li>
</ul>

<hr>

<h2>🔌 Esquema de Ligação</h2>
<p><strong>Para cada ESP32:</strong></p>
<ul>
  <li>LED no GPIO 18 com resistor (ligado ao GND)</li>
  <li>Botão no GPIO 4 com resistor pull-up (ou INPUT_PULLUP no código)</li>
</ul>

<hr>

<h2>💻 Código da ESP32 (Grupo A)</h2>

<p>Este código conecta a ESP32 ao Wi-Fi e ao broker MQTT, escuta um tópico e envia uma mensagem ao outro grupo ao apertar o botão.</p>

<p><a href="https://github.com/seu-usuario/seu-repo/blob/main/esp_grupoA.ino">Clique aqui para ver o código</a></p>

<hr>

<h2>🍓 Configuração do Broker (Raspberry Pi)</h2>

<ol>
  <li>Instale o Mosquitto:
    <pre><code>sudo apt update
sudo apt install mosquitto mosquitto-clients</code></pre>
  </li>
  <li>Ative o serviço:
    <pre><code>sudo systemctl enable mosquitto
sudo systemctl start mosquitto</code></pre>
  </li>
  <li>Configure para permitir conexões:
    <pre><code>sudo nano /etc/mosquitto/mosquitto.conf</code></pre>
    <p>Adicione:</p>
    <pre><code>listener 1883
allow_anonymous true</code></pre>
  </li>
  <li>Reinicie o Mosquitto:
    <pre><code>sudo systemctl restart mosquitto</code></pre>
  </li>
</ol>

<hr>

<h2>🧪 Testes com Mosquitto</h2>

<p><strong>Escutar tópico:</strong></p>
<pre><code>mosquitto_sub -t grupoA/led</code></pre>

<p><strong>Publicar mensagem:</strong></p>
<pre><code>mosquitto_pub -t grupoA/led -m "ligar"</code></pre>

<hr>

<h2>🧠 Funcionamento</h2>
<p>Cada ESP fica escutando um tópico próprio e publicando em um tópico do outro grupo.</p>

<table border="1" cellpadding="8">
  <tr>
    <th>Grupo</th>
    <th>Tópico que ESCUTA</th>
    <th>Tópico que ENVIA</th>
  </tr>
  <tr>
    <td>Grupo A</td>
    <td>grupoA/led</td>
    <td>grupoB/led</td>
  </tr>
  <tr>
    <td>Grupo B</td>
    <td>grupoB/led</td>
    <td>grupoA/led</td>
  </tr>
</table>

<hr>

<h2>📦 Estrutura do Projeto</h2>

<pre>
📁 projeto-mqtt-esp32
├── 📂 grupoA
│   └── esp_grupoA.ino
├── 📂 grupoB
│   └── esp_grupoB.ino
├── 📁 raspberry
│   └── mosquitto-config.txt
└── README.md
</pre>

<hr>

<h2>📃 Licença</h2>
<p>Este projeto é de uso educacional no <strong>SENAI</strong> e pode ser adaptado livremente para fins acadêmicos e de aprendizado.</p>
