# DigitalDesignComputerArchitecture

## Bölüm 1: Dijital Mantığın Temelleri (Bit ve Sayılar)

### 1.1. Bit Nedir?
* **Bit**, "Binary Digit" (İkili Rakam) kelimelerinin kısaltmasıdır.
* Bir bilgisayardaki **en temel** ve **en küçük** veri birimidir.
* Sadece iki değeri temsil edebilir: **0** (kapalı, yanlış, düşük voltaj) veya **1** (açık, doğru, yüksek voltaj).
* Fiziksel olarak, bir **Transistör** kullanılarak bir anahtar (1=açık, 0=kapalı) gibi veya bir transistör+kondansatör ikilisiyle (1=dolu, 0=boş) depolanır.

### 1.2. Sayı Temsili ve İşaret Biti
* **Sorun:** Bilgisayarlar sadece 0 ve 1'leri bilir. Negatif sayıları ("-") nasıl temsil edecekler?
* **Çözüm:** **İşaret Biti (Sign Bit)** kullanılır. Bir sayı grubundaki (örn: 8-bit) **en soldaki bit** bu iş için ayrılır.
    * **0** = Pozitif sayı
    * **1** = Negatif sayı

### 1.3. İki'ye Tümleyen (Two's Complement)
Modern bilgisayarların negatif sayıları temsil etme yöntemidir. Sadece bir "işaret biti" kullanmaktan daha verimlidir çünkü "çift sıfır" (+0 ve -0) sorununu çözer ve toplama/çıkarma işlemlerini basitleştirir.

* **Matematiksel Kuralı:** N-bitlik bir sistemde, en soldaki bitin (a_N-1) "ağırlığı" pozitif değil, -(2^(N-1))'dir.
    * Değer = a_N-1 * (-(2^(N-1))) + (diğer bitlerin toplamı)
* **4-bit Örneği (Aralık: -8 ile +7 arası):**
    * `0111` = 0 * (-8) + 1*4 + 1*2 + 1*1 = **+7** (En pozitif)
    * `1000` = 1 * (-8) + 0*4 + 0*2 + 0*0 = **-8** (En negatif)
    * `1111` = 1 * (-8) + 1*4 + 1*2 + 1*1 = **-1**

### 1.4. Taşma (Overflow) ve İşaret Genişletme (Sign Extension)
* **Taşma (Overflow):** Bir işlemin sonucunun, ayrılan bit aralığına sığmamasıdır (örn: 4-bit sistemde 5+4 = 9 yapmaya çalışmak).
    * İki'ye Tümleyen'de bu, **mantıksız bir işaret değişikliği** olarak tespit edilir:
        * `Pozitif + Pozitif = Negatif` -> **Taşma!**
        * `Negatif + Negatif = Pozitif` -> **Taşma!**
* **İşaret Genişletme (Sign Extension):** Bir sayıyı daha büyük bir bit alanına (örn: 8-bit'ten 32-bit'e) taşırken değerini koruma yöntemidir.
    * **Kural:** Boş kalan tüm yeni bitler, sayının orijinal **işaret biti** ile doldurulur.
        * `+5` (4-bit): `0101` -> (8-bit): `00000101` (sol taraf 0 ile dolar)
        * `-5` (4-bit): `1011` -> (8-bit): `11111011` (sol taraf 1 ile dolar)

---

## Bölüm 2: Lojik Kapılar (Mantığın Yapı Taşları)

Mantık kapıları, 1 ve 0'lar üzerinde basit kararlar veren temel devrelerdir.

| Kapı   | İsim Kökeni        | Temel Anlamı ve Görevi                               |
| :----- | :----------------- | :--------------------------------------------------- |
| **NAND** | **N**OT-**AND** (VE-DEĞİL)   | AND'in tersi. Sadece tüm girişler 1 ise 0 verir.     |
| **NOR** | **N**OT-**OR** (VEYA-DEĞİL)  | OR'un tersi. Sadece tüm girişler 0 ise 1 verir.      |
| **XOR** | **X**-**OR** (Özel VEYA)    | "Farklılık Detektörü". Girişler **farklıysa** 1 verir. |
| **XNOR** | **X**-**NOR** (Özel VEYA-DEĞİL) | "Eşitlik Detektörü". Girişler **aynıysa** 1 verir.    |

* **Neden XOR'a İhtiyaç Var?**
    1.  **Aritmetik:** İki bitin toplama işlemindeki "Toplam" (Sum) biti, bir XOR kapısıdır.
    2.  **Karşılaştırma:** XNOR kapısı, iki sayının eşit olup olmadığını kontrol eder.
    3.  **Veri Manipülasyonu:** Bir biti '1' ile XOR'lamak, o biti ters çevirir (flip).
* **Neden AND3, NOR3 Var?**
    * Mantıken iki adet 2-girişli kapı ile aynı iş yapılabilir, ancak 3-girişli tek bir kapı kullanmak:
        1.  **Daha Hızlıdır** (yayılma gecikmesini azaltır).
        2.  **Daha Verimlidir** (çip üzerinde daha az yer kaplar).
* **Buffer (Tampon):**
    * Mantıksal olarak hiçbir şey yapmaz (Giriş=Çıkış).
    * **Elektriksel** bir iş yapar: Sinyali **güçlendirir**. Zayıf bir çıkışın (örn: mikroişlemci) yüksek akım çeken bir yükü (örn: çok sayıda LED) sürebilmesini sağlar.

---

## Bölüm 3: Fiziksel Gerçeklik (Voltaj, Gürültü, Marj)

### 3.1. Lojik Seviyeler (VDD ve Lojik Değer)
* **Lojik Değer (Soyut):** "1" ve "0" matematiksel kavramlardır ("Doğru", "Yanlış").
* **Gerilim (Fiziksel):** Bu soyut kavramları temsil eden fiziksel sinyallerdir.
* **Sözleşme:**
    * **Lojik 1** -> **VDD**'ye (pozitif güç kaynağı, örn: +5V) yakın voltajla temsil edilir.
    * **Lojik 0** -> **GND**'ye (toprak, 0V) yakın voltajla temsil edilir.

### 3.2. Gürültü ve Gürültü Marjı
* **Gürültü (Noise):** Sinyale bulaşan, istenmeyen voltajlardır.
* **Kapasitif Kuplaj:** Bir sinyal kablosunun, yakındaki başka bir kabloya (veya GND'ye) fiziksel olarak yakın durması, istenmeyen bir "parazitik kapasitör" oluşturur. Bu kapasitör, dış gürültünün sinyale "atlaması" için bir köprü görevi görür.
* **Gürültü Marjı (Noise Margin):** Bir devrenin, sinyal bozulsa bile doğru çalışmaya devam etmesini sağlayan **güvenlik payıdır**.
    * **V_OH (Voltage Output High):** Sürücünün "1" olarak göndermeyi garanti ettiği **en düşük** voltaj.
    * **V_IH (Voltage Input High):** Alıcının "1" olarak kabul edeceği **en düşük** voltaj.
    * **V_OL (Voltage Output Low):** Sürücünün "0" olarak göndermeyi garanti ettiği **en yüksek** voltaj.
    * **V_IL (Voltage Input Low):** Alıcının "0" olarak kabul edeceği **en yüksek** voltaj.
* **Güvenlik Payları:**
    * **High Noise Margin (NM_H) = V_OH - V_IH**
    * **Low Noise Margin (NM_L) = V_IL - V_OL**

---

## Bölüm 4: Transistörler (Kapıların İnşası)

### 4.1. Transistör Nedir?
* Tüm mantık kapılarının temel yapı taşıdır.
* Dijital mantıkta, **voltaj kontrollü bir anahtar** olarak davranır.
* 3 terminali vardır: **Gate (g)**, **Drain (d)**, **Source (s)**.
* **Gate (Kapı):** Anahtarı kontrol eden sinyal girişidir.
* **Drain/Source:** Anahtarın iki ucudur.

### 4.2. Yarıiletken Fiziği
* Transistörler **Silikon**'dan yapılır (bir yarı iletken).
* Saf silikon elektriği iyi iletmez.
* İletken hale getirmek için **Katkılama (Doping)** işlemi yapılır:
    * **n-tipi:** Silikona **Arsenik (As)** gibi bir katkı eklenir. Bu, fazladan "serbest elektronlar" (negatif yükler) yaratır.
    * **p-tipi:** Silikona **Bor (B)** gibi bir katkı eklenir. Bu, "elektron eksiklikleri" yani **deşikler** (pozitif yükler) yaratır.

---

## Bölüm 5: CMOS Mimarisi (Modern Tasarım Felsefesi)

**CMOS** ("Tamamlayıcı" MOS), bu iki tip malzemeden yapılan iki zıt transistörü bir arada kullanır.

### 5.1. İki Uzman: nMOS ve pMOS
* **nMOS Transistörü (n-tipi malzeme):**
    * **Görevi:** Çıkışı **Lojik 0**'a (GND) çekmek.
    * **Kuralı:** `Gate = 1` (pozitif voltaj) aldığında **AÇILIR (ON)**.
    * **Fiziksel Çalışması:** `Gate`'e verilen pozitif voltaj, `p-tipi` gövdedeki elektronları çekerek `source` ile `drain` arasında bir "kanal" (köprü) oluşturur.
* **pMOS Transistörü (p-tipi malzeme):**
    * **Görevi:** Çıkışı **Lojik 1**'e (VDD) çekmek.
    * **Kuralı:** `Gate = 0` (düşük voltaj) aldığında **AÇILIR (ON)**.
    * **Fiziksel Çalışması (Görecelilik):** `Source`'u VDD'ye (+5V) bağlıdır. `Gate`'ine 0V vermek, `Gate`'i `Source`'a göre **negatif** (-5V) yapar. Bu negatif alan, `source` ile `drain` arasında pozitif deşiklerden bir "kanal" oluşturur.

### 5.2. CMOS Mimarisi (Pull-up & Pull-down)
Bir CMOS kapısı (örn: NOT Kapısı), bu iki uzmanı mükemmel bir "tamamlayıcı" düzende birleştirir:

1.  **pMOS Ağı (Pull-up):**
    * Devrenin **üst yarısıdır**.
    * pMOS'un **`Source`'u VDD'ye** bağlanır.
    * pMOS'un **`Drain`'i OUTPUT**'a bağlanır.
    * Görevi çıkışı "yukarı çekmektir" (1 yapmak).

2.  **nMOS Ağı (Pull-down):**
    * Devrenin **alt yarısıdır**.
    * nMOS'un **`Source`'u GND'ye** bağlanır.
    * nMOS'un **`Drain`'i OUTPUT**'a bağlanır.
    * Görevi çıkışı "aşağı çekmektir" (0 yapmak).

3.  **INPUT ve OUTPUT:**
    * **INPUT** sinyali, her iki transistörün de **`Gate`**'ine aynı anda bağlanır.
    * **OUTPUT** sinyali, her iki transistörün **`Drain`**'lerinin birleştiği ortak noktadan alınır.

### 5.3. Çalışma Prensibi (NOT Kapısı)
Bu mimari sayesinde, aynı giriş sinyaline zıt tepki verirler:

| INPUT (Gate'lere gider) | pMOS Tepkisi (Kuralı: 0=ON)    | nMOS Tepkisi (Kuralı: 1=ON)      | OUTPUT (Drain'lerin ortak noktası) |
| :---------------------- | :----------------------------- | :------------------------------- | :--------------------------------- |
| **0** (Düşük Voltaj)      | **AÇIK (ON)** (Çıkışı VDD'ye bağlar) | **KAPALI (OFF)** (Çıkışın GND ile bağı kesilir) | **1 (High)** |
| **1** (Yüksek Voltaj)     | **KAPALI (OFF)** (Çıkışın VDD ile bağı kesilir) | **AÇIK (ON)** (Çıkışı GND'ye bağlar)     | **0 (Low)** |

Bu tasarım, iki ağdan her zaman *sadece birinin* açık olmasını sağlar, bu da CMOS'u inanılmaz derecede **güç verimli** yapar (beklemedeyken kısa devre olmaz).

---

## Bölüm 6: Tasarım Dilleri (HDL)

* **HDL (Donanım Tanımlama Dili) Nedir?**
    * Modern dijital devreler (milyarlarca transistör) elle çizilemez.
    * **Verilog** ve **VHDL**, bu devrelerin **planını** veya **mimarisini** kod olarak tanımlayan dillerdir.
* **Farkı Nedir? (Programlama vs. Donanım Tanımlama)**
    * **Programlama (C, Python):** CPU'ya "adım adım ne yapacağını" söyleyen *talimatlardır*. **Sırayla** çalışır.
    * **HDL (Verilog, VHDL):** Bir çipin içinde "hangi kapıların birbirine nasıl bağlanacağını" tarif eden *plandır*. **Paralel** olarak çalışır (tanımlanan her kapı aynı anda var olur).
