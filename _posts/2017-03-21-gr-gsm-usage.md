---
layout: post
title: RTL-SDR GR-GSM USAGE
cover: grgsmusage.png
date:   2017-03-21 00:46:00
categories: posts
---


### GR-GSM ile GSM sinyali dinleme ve analiz etme.

Öncelikle GSM sinyallerini dinleme ve analiz etmek için belli başlı toollara kullanacağımızı daha önceki yazılarımızda söylmiştik, şimdi bu toollardan biri olan GR-GSM toolunu ele alacağız.

  * [https://github.com/ptrkrysik/gr-gsm/tree/packaging](https://github.com/ptrkrysik/gr-gsm/tree/packaging) adresinden packaging branch'ı olması şartı ile paketimizi .zip uzantısıyla indiriyoruz.<br/>
 Paketi indirdikten sonra sırasıyla uygulayacağımız adımlar :
  * Kurulum yapılacak olan dizini oluşturun
  <pre><code>mdkir gr-gsm
  cd gr-gsm
  mv ~/Downloads/gr-gsm-packaging.zip ~/gr-gsm
  </code></pre>
  * [https://github.com/ptrkrysik/gr-gsm/wiki/Manual-compilation-and-installation](https://github.com/ptrkrysik/gr-gsm/wiki/Manual-compilation-and-installation) projenin "Wiki"'sinden installation için gerekli adımları uyguluyoruz.
  * Eğer Kali, Ubuntu gibi debian temelli bir linux kullanıcısıysanız yapmanız gerekenler şunlar :
  * Wiki'den alınmış ve kurulması gereken programları kuruyoruz :<br/>
  <pre><code>sudo apt-get install gnuradio gnuradio-dev rtl-sdr librtlsdr-dev cmake osmo-sdr libosmosdr-dev libosmocore libosmocore-dev libboost-all-dev libcppunit-dev swig doxygen liblog4cpp5-dev python-scipy
  </code></pre>
  * Sonraki aşamada normalde git clone diyerek git'ten dosyaları çekmemiz gerekiyor fakat Wikinin bize verdiği bu link master brach olduğundan ve bizim packaging branch üzerinde çalıştığımızdan dolayı o aşamayı es geçip kaldı ki bu dosyanın packaging branch'ını az önce .zip olarak indirmiştik, oluşturduğumuz dizine indirdiğimiz zip dosyasını çıkarıyoruz.
  <pre><code>unzip gr-gsm-packaging.zip
  cd gr-gsm-packaging
  mkdir build
  cd build
  cmake ..
  make
  make install
  ldconfig
  cd
  mkdir .gnuradio
  cd .gnuradio/
  nano config.conf
  </code></pre>
  * Son kod ile oluşturduğumuz bu config.conf dosyasının içine :
  <pre><code>[grc]
  local_blocks_path=/usr/local/share/gnuradio/grc/blocks
  </code></pre>
  satırlarını ekleyip kaydediyoruz.

Kurulumu başarıyla tamamladıktan sonra kullanımına gelelim :
   [https://github.com/ptrkrysik/gr-gsm/wiki/Usage](https://github.com/ptrkrysik/gr-gsm/wiki/Usage) adresinden kullanım kitaçığına erişebilirsiniz.

Aşağıdaki uygulamalar gr-gsm tabanlıdır:

  * grgsm_decode : eski adıyla airprobe_decode.py , C0 kanalların decode edilmesi içindir.<br/>
  * grgsm_livemon : eski adıyla airprobe_rtlsdr.py , Wireshark ile bir C0 kanalının analiz edilmesi ve interaktif olarak izlenmesi için kullanılır.<br/>
  * grgsm_scanner : eski adıyla airprobe_rtlsdr_scanner.py , Bu program ise GSM bandlarını tarayarak ekrana informationları getirir.<br/>
Aşağıdaki programlar yardımcı scriptler konumundadır :<br/>

  * grgsm_capture : eksi adıyla airprobe_rtlsdr_capture.py , GSM sinyallerini yakalra ve grgsm_decode'un anlayabileceği bir forma getirir.<br/>
  * grgsm_channelize : eski adıyla grgsm_channelize.py ,Genişband olarak yayılan GSM sinyallerini ayırır.<br/>

## C0 GSM Sinyallerini görüntüleme :  
    grgsm_livemon
  yazıyoruz.Programın bu penceresi spektrumların genliği ve real-time'da işlenen sinyaller hakkında bize bilg verir.Merkezi frekans hareketli kaydırıcı tarafından değiştirilebilir. GSM sinyalleri 200kHz bant genişliğine sahiptir. Fc slider'ı (kaydırıcı çubuk, buton) broadcast channel'ın taşıyıcı frekansa ayarladıktan sonra program hemen bir içerik ekrana basacaktır.Eğer bu işe yaramadıysa ppm slider'ı farklı bir pozisyona getirin.
  grgsm_livemon gerçek zamanlı olarak C0 GSM sinyallerini decode eder. C0 kanalı kullanıcıların datasını,bütün şebekelerin bilgisini, konfigürasyonunu taşır. Bu program tıpkı gönderilen sinyalin kaynağı (kaynak cihaz,telefon) gibi ucuz RTL-SDR alıcısını kullanır. grgsm_livemon'u çalıştırmakk için konsola:

## GSM Sinyallerini yakalama ve bir  dosyaya kaydetme
  Bu program yakalanmış sinyaleri bir dosyaya kadetme genişliği sağlar. Hem raw data formatında hem de gr-gsm'in brust formatında kayıt imkanı sağlar.
  <pre><code>grgsm_decode
  </code></pre>
  Kaydetme hakkında daha ayrıntılı bilgi sağlamak için programı -h parametresi ile başlatalım.
  <pre><code>grgsm_capture -h
  </code></pre>
## Yakalanan GSM sinyallerini grgsm_decode ile çözmek
  grgsm_decode programı grgsm_capture ile kaydedilmiş GSM mesajları decode eder. Programı başlatmak için
  <pre><code>grgsm_decode
  </code></pre>
  Program hem cfile hemde brust türünde kaydedilmiş dosyaları destekler ve decode eder. A5 şifrelemeleri için, A5/1, A5/2 ve A5/3  decyrptionlar
desteklenir. grgsm_decode, GSM-FR, GSM-EFR, AMR 12.2, AMR 10.2, AMR 7.95, AMR 7.4, AMR 6.7, AMR 5.9, AMR5.15,AMR 4.75 ses dosyalarının bileşenlerini içerir.
Program hakkında daha detaylı bilgi için
  <pre><code>grgsm_decoder -h
  </code></pre>
parametresini kullanablirsiniz.
Daha detaylı bilgi için [buraya tıklayın](https://github.com/ptrkrysik/gr-gsm/wiki/Usage:-Decoding-How-To)

## GSM Mesajlarını Wireshark ile analiz etme.
Gr-gsm uygulaması GSMTAP formatında UDP 4729 portuna GSM mesajı gönderebilir. Wireshark bu porta gelen istekleri GSMTAP formatında yorumlar.
Debian tabanlı sistemlerde Wireshark aşağıdaki komutla kurulabilir.
<pre><code>sudo apt-get install wireshark
</code></pre>
Wireshark'ı başlatmak ve grgsm_decode'dan elde edilmiş GSMTAP packetlerini analiz etmek için aşağıdaki komut verilebilir.
<pre><code>sudo wireshark -k -f udp -Y gsmtap -i lo
</code></pre>
## Kanallar arası geçiş yapmak
GSM sinyalleri bölünmüş sinyallerdir ve bu sinyallerde işimize yarayanı bulmak için bazı işlemler yaparız.
Örneğin aşağıdaki komut my_wideband_capture.cfile adındaki kaydedilmiş 925.2 MHz merkezli (ARFCN 975) dosyadan , her 1 Msps'de  12 tane sonuç dosyası çıkarır ARFCNs 975-1023.
<pre><code>  grgsm_channelize.py -s 20e6 -c my_wideband_capture.cfile -f 925.2e6 990 991 992 993 994 995 1019 1020 1021 1022 1023
</code></pre>
