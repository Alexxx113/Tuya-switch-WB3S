# Tuya switch WB3S WB2S WB2L

<p>
Configure tuya switch OpenBK7231T
</p>
<img src="https://github.com/Alexxx113/Tuya-switch-WB3S-WB2S-WB2L/blob/main/IMG_20220716_015555.jpg" alt="альтернативный текст" />
<p></p>
<b>Flashing for BK7231T</b>

<p>remove smd components</p>
<img src="https://github.com/Alexxx113/Tuya-switch-WB3S-WB2S-WB2L/blob/main/firmware%20remowe.jpg" alt="альтернативный текст" />
<p></p>
<p>Go to pages firmware instruction</p>


<a name="идентификатор">OpenBK7231T_App]https://github.com/openshwprojects/OpenBK7231T_App</a>

<p></p>
<p>connect uart 3.3v !!!!</p>
<img src="https://github.com/Alexxx113/Tuya-switch-WB3S-WB2S-WB2L/blob/main/firmware uart contacts.jpg" alt="альтернативный текст" />

<p></p>
<b>configure module:</b>
<p></p>
<p>P1 wifi_led_n 0</p>
<p>P9 rel 1</p>
<p>P10 btn 1</p>

<p>configure MQTT</p>

<b>home assistant config:</b>
<p>"name" = name switch</p>

<div class="snippet-clipboard-content notranslate position-relative overflow-auto" data-snippet-clipboard-copy-content="">
  <pre class="notranslate">
  <code>
light:
  - platform: mqtt
    unique_id: "name"
    name: "name"
    state_topic: ""name"/1/get"
    command_topic: ""name"/1/set"
    qos: 1
    payload_on: 1
    payload_off: 0
    retain: true
    availability_topic: ""name"/connected"
  </code>
  </pre>
</div>




<b>if you want add temp sensor:</b>
<p>P23 adc 2</p>
<p>NTC thermistor a B value of 3950 100k</p>
<p>resistor 20k</p>

<b>home assistant config:</b>
<p>"name" = name switch</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto" data-snippet-clipboard-copy-content="">
  <pre class="notranslate">
  <code>  
sensor:
  - platform: mqtt
    state_topic: ""name"/2/get"
    name: "mqtt temp sens"
    qos: 1
    device_class: power
    availability_topic: "koridor/connected"
  </code>
  </pre>
</div
  <p>"name" = name switch</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto" data-snippet-clipboard-copy-content="">
  <pre class="notranslate">
  <code>
template:
  sensor:
    - name: Temperatura NTC
      unique_id: temperature
      device_class: temperature
      state: >
        {% set Vo = states('sensor.mqtt_temp_sens')|float %}
        {% set c1 = 1.009249522e-03 %}
        {% set c2 = 2.378405444e-04 %}
        {% set c3 = 2.019202697e-07 %}
        {% set R1 = 100000 %}
        {% set R2 = R1 * 1023 / ( Vo - 1 ) %}
        {% set logR2 = log(R2,10) %}
        {% set T = ( 1 / ( c1 + c2 * logR2 + c3 * logR2**3 ) - 273.15 )  / 5 -11.3 %}
        {{ T |round(1) }}
      unit_of_measurement: '°C'
  </code>
  </pre>
</div>
