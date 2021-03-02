# TempandPulseMeter
## Ateş ve Nabız Ölçer 

<p align="center">
  <img width="500" height="350" src="https://github.com/celikslab/TempandPulseMeter/blob/main/images/Main.jpg">
</p>

Önerilen bu yaklaşımın amacı, otomatik bir sağlık izleme sistemidir. Sıcaklık ve kalp atış hızı ile hastanın vücudu sürekli olarak kontrol edilir. Bu sistemde kullanılan gerekli bileşenler bir ATmega328p mikrodenetleyici, sıcaklık sensörü, nabız sensörü, lazer ve OLED ekrandır. Arduino burada hastanın sürekli verilerini kontrol eder eğer vücut ısısı normal sıcaklık oranından daha yüksek ise sesli uyarı(bip sesi) çıkarmaya başlar.

## Malzemeler :

<li>Arduino Nano </li>
<li>Gy-906 Temassız Kızılötesi Sıcaklık Sensörü</li>
<li>Pulse Nabız Ölçer Sensörü</li>
<li>SD1306 128x64 Oled Ekran</li>
<li>5V Lazer Modülü</li>
<li>12x12 Push Buton</li>
<li>5V Buzzer</li>
<li>40Pin Dişi Header</li>
<li>40Pin Erkek Header</li>
<li>Delikli Plaket</li>

## Devre Şeması :

<p align="center">
  <img width="660" height="372" src="https://github.com/celikslab/TempandPulseMeter/blob/main/images/Schematic.png">
</p>

## Kodlar :

Kodların tamamına [buradan](https://github.com/celikslab/TempandPulseMeter/blob/main/pulseandtemp.ino) ulaşabilirsiniz.
