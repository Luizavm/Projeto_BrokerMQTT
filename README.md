<h1>Comunicação ESP32 via MQTT com Raspberry Pi (Broker):</h1>
<p><i>Este projeto tem como foco o desenvolvimento de um sistema de comunicação entre dispositivos embarcados, utilizando o protocolo MQTT, muito usado em aplicações de Internet das Coisas (IoT). A ideia principal é que diferentes placas ESP32 consigam se comunicar entre si através de um servidor central MQTT, que neste caso é um Raspberry Pi com o Mosquitto instalado.

A comunicação é feita com base em tópicos MQTT, permitindo que uma ESP publique mensagens e outra ESP, conectada ao mesmo broker, as receba — por exemplo, para acender ou apagar um LED remotamente.</i></p>

<hr>

<h2>Recursos utilizados:</h2>
<ul>
  <li>2x Placas ESP32;</li>
  <li>1x Raspberry Pi;</li>
  <li>2x LEDs;</li>
  <li>2x Resistores de 220Ω;</li>
  <li>1x Protoboard;</li>
  <li>Jumpers (fios de conexão);</li>
  <li>1x Roteador ou ponto de acesso Wi-Fi;</li>
  <li>Notebook;</li>
  <li>Cabos micro-USB para conectar as ESP32;</li>
  <li>Monitor;</li>
  <li>Teclado e mouse.</li>
</ul>

<hr>

<h2>Esquema de ligações:</h2>
<table>
  <thead>
    <tr>
      <th>Pino ESP32</th>
      <th>Conexão</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GPIO 4</td>
      <td>Anodo (perna longa) do LED (via resistor)</td>
    </tr>
    <tr>
      <td>GND</td>
      <td>Catodo (perna curta) do LED</td>
    </tr>
  </tbody>
</table>

<h3>Explicação detalhada:</h3>
<ul>
  <li>O pino <strong>GPIO 4</strong> da ESP32 é configurado para acionar o LED.</li>
  <li>Um <strong>resistor</strong> está em série com o LED para limitar a corrente e evitar queimar o LED.</li>
  <li>O <strong>GND</strong> da ESP32 é conectado ao <strong>GND do LED</strong> (catodo).</li


<hr>

<h1>Passo a Passo: Instalação da Imagem da Raspberry Pi no Cartão SD</h1>

<h2>1️-Baixar o Raspberry Pi Imager:</h2>
<ul>
  <li>Acesse o site oficial: <a href="https://www.raspberrypi.com/software/" target="_blank">https://www.raspberrypi.com/software/</a></li>
  <li>Baixe o <strong>Raspberry Pi Imager</strong> para seu sistema operacional (Windows, macOS ou Linux).</li>
  <li>Instale o programa normalmente.</li>
</ul>

<h2>2️-Preparar o Cartão SD:</h2>
<ul>
  <li>Pegue um cartão microSD (recomendado: <strong>classe 10</strong> e <strong>8GB ou maior</strong>).</li>
  <li>Insira o cartão no computador (se precisar, use um adaptador USB).</li>
</ul>

<h2>3️-Abrir o Raspberry Pi Imager:</h2>
<ul>
  <li>Abra o programa instalado.</li>
  <li>Clique em <strong>"Choose OS"</strong> e selecione o sistema que deseja instalar (ex: "Raspberry Pi OS (32-bit)").</li>
  <li>Clique em <strong>"Choose Storage"</strong> e selecione o seu cartão SD.</li>
</ul>

<h2>4-Gravar a Imagem:</h2>
<ul>
  <li>Clique em <strong>"Write"</strong>.</li>
  <li>Confirme para apagar o conteúdo do cartão.</li>
  <li>Aguarde o processo de gravação (pode levar alguns minutos).</li>
</ul>

<h2>5-Finalizar:</h2>
<ul>
  <li>Quando o processo terminar, o programa vai avisar.</li>
  <li>Remova o cartão SD com segurança do computador.</li>
  <li>Insira o cartão SD na sua Raspberry Pi.</li>
  <li>Conecte a fonte de alimentação para ligar a Raspberry!</li>
</ul>

<h1>Instalação do Broker MQTT Mosquitto</h1>

<h2>1️-Atualizar o Sistema</h2>
<ul>
  <li>Abra o terminal da Raspberry Pi.</li>
  <li>Execute os comandos abaixo para atualizar os pacotes do sistema:
    <pre><code>sudo apt-get update
sudo apt-get upgrade</code></pre>
  </li>
</ul>

<h2>2️-Instalar o Mosquitto e o Cliente MQTT</h2>
<ul>
  <li>Instale o broker Mosquitto:
    <pre><code>sudo apt-get install mosquitto -y</code></pre>
  </li>
  <li>Instale os clientes para teste de publicação e subscrição:
    <pre><code>sudo apt-get install mosquitto-clients -y</code></pre>
  </li>
</ul>

<h2>3️-Configurar o Mosquitto</h2>
<ul>
  <li>Abra o arquivo de configuração do Mosquitto:
    <pre><code>sudo nano /etc/mosquitto/mosquitto.conf</code></pre>
  </li>
  <li>Dentro do arquivo, apague a seguinte linha:
    <pre><code>include_dir /etc/mosquitto/conf.d</code></pre>
  </li>
  <li>Adicione estas linhas no lugar:
    <pre><code>allow_anonymous false
password_file /etc/mosquitto/pwfile
listener 1883</code></pre>
  </li>
  <li>Salve o arquivo pressionando <strong>Ctrl + O</strong> e saia com <strong>Ctrl + X</strong>.</li>
</ul>

<h2>4️-Criar Usuário e Senha</h2>
<ul>
  <li>Substitua <code>{seu_nome_de_usuario}</code> pelo nome de usuário que deseja criar:
    <pre><code>sudo mosquitto_passwd -c /etc/mosquitto/pwfile {seu_nome_de_usuario}</code></pre>
  </li>
</ul>

<h2>5️-Reiniciar a Raspberry Pi</h2>
<ul>
  <li>Para aplicar todas as configurações, reinicie o sistema:
    <pre><code>sudo reboot</code></pre>
  </li>
</ul>

<h1>Instalação da Arduino IDE no Notebook</h1>

<h2>1️-Acesse o Site Oficial:</h2>
<ul>
  <li>Abra o navegador e vá até o site oficial da Arduino:</li>
  <li><a href="https://www.arduino.cc/en/software" target="_blank">https://www.arduino.cc/en/software</a></li>
</ul>

<h2>2️-Baixe a IDE:</h2>
<ul>
  <li>Escolha a versão compatível com seu sistema operacional:</li>
  <ul>
    <li>Windows</li>
    <li>macOS</li>
    <li>Linux</li>
  </ul>
  <li>Clique em “Download” e selecione “Just Download” para baixar gratuitamente.</li>
</ul>

<h2>3️-Instale a Arduino IDE:</h2>
<ul>
  <li>Abra o arquivo baixado e siga o assistente de instalação:</li>
  <li>Marque a opção <code>Install USB driver</code> se solicitado.</li>
  <li>Finalize a instalação e abra a IDE Arduino.</li>
</ul>

<h2>4️-Adicionar Suporte à ESP32:</h2>
<ul>
  <li>Abra a Arduino IDE e vá até:
    <pre><code>Arquivo &gt; Preferências</code></pre>
  </li>
  <li>No campo <strong>“URLs adicionais para Gerenciadores de Placas”</strong>, adicione o seguinte link:
    <pre><code>https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json</code></pre>
  </li>
  <li>Clique em <strong>“OK”</strong> para salvar.</li>
</ul>

<h2>5️-Instalar a Placa ESP32:</h2>
<ul>
  <li>Agora vá até:
    <pre><code>Ferramentas &gt; Placa &gt; Gerenciador de Placas</code></pre>
  </li>
  <li>Procure por <strong>ESP32</strong> e clique em <strong>Instalar</strong>.</li>
</ul>
<p>Este projeto é de uso educacional no <strong>SENAI</strong> e pode ser adaptado livremente para fins acadêmicos e de aprendizado.</p>

<h1>Instalação de Bibliotecas na Arduino IDE</h1>

<h2>1️-Abrir o Gerenciador de Bibliotecas:</h2>
<ul>
  <li>Na Arduino IDE, vá até o menu:
    <pre><code>Sketch &gt; Incluir Biblioteca &gt; Gerenciar Bibliotecas...</code></pre>
  </li>
</ul>

<h2>2️-Bibliotecas Necessárias:</h2>
<p>Pesquise e instale as seguintes bibliotecas:</p>
<ol>
  <li>
    <strong>WiFi</strong> (já vem instalada com o suporte da ESP32, mas certifique-se de que está presente)
  </li>
  <li>
    <strong>PubSubClient</strong>
    <ul>
      <li>Pesquise por <code>PubSubClient</code></li>
      <li>Autor: Nick O'Leary</li>
      <li>Clique em <strong>Instalar</strong></li>
    </ul>
  </li>
</ol>

<h2>3️-Confirmar Instalação:</h2>
<ul>
  <li>Após instalar, vá novamente em:
    <pre><code>Sketch &gt; Incluir Biblioteca</code></pre>
  </li>
  <li>Verifique se as bibliotecas <code>WiFi</code> e <code>PubSubClient</code> aparecem na lista.</li>
</ul>


