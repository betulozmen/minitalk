# Minitalk

Minitalk projesi, UNIX sinyalleri kullanarak iki process arasında iletişim kuran bir client-server uygulamasıdır.


Bu projede amaç, yalnızca `SIGUSR1` ve `SIGUSR2` sinyallerini kullanarak bir **client**'tan bir **server**'a string göndermektir. Her karakter 0 ve 1'e çevrilip sinyal olarak iletilir, server bu bitleri tekrar karaktere dönüştürüp ekrana yazar.

- Her 8 sinyal = 1 karakter 

---

## Kullanılan Bazı Fonksiyonlar
 
### `getpid()`
Çalışan programın **Process ID**'sini (PID) döndürür. Her process'in benzersiz bir numarası vardır, server bu numarayı ekrana yazdırır ki client nereye sinyal göndereceğini bilsin.
 
### `signal()`
Belirli bir sinyal geldiğinde **hangi fonksiyonun çalışacağını** işletim sistemine bildirir.
 
### `kill()`
Bir process'e sinyal **gönderir**. İsmine rağmen sadece öldürmez, her türlü sinyal gönderilebilir.
 
### `pause()`
Bir sinyal gelene kadar programı **uyku moduna** alır. CPU kullanmadan bekler.

### `usleep()`
Programı belirtilen **mikrosaniye** kadar uyutur. Client sinyalleri arasında beklemek için kullanır.

---
 
## Sinyal Nedir?

Unix ve Unix benzeri işletim sistemlerinde sinyaller bulunmaktadır. Sinyaller, işletim sisteminin işlem yönetimi ve iletişimi için önemli bir mekanizmadır. Bir işletim sistemi, bir süreci veya işlemi durdurmak, devam ettirmek, sonlandırmak veya başka eylemler gerçekleştirmek için sinyalleri kullanabilir. Yani C programlama dilinde sinyaller, işletim sistemi veya kullanıcı tarafından bir programın çalışması sırasında gönderilen, programın normal işleyişini etkileyen olayları temsil eder.

| Sinyal | Numara | Açıklama | Bu Projede |
|---|---|---|---|
| `SIGUSR1` | 10 | Kullanıcı tanımlı sinyal | **1 biti** temsil eder. |
| `SIGUSR2` | 12 | Kullanıcı tanımlı sinyal | **0 biti** temsil eder. |
| `SIGINT` | 2 | Ctrl+C | Server'ı durdurur. |
| `SIGTERM` | 15 | `kill PID` | Server'ı durdurur .|
| `SIGKILL` | 9 | `kill -9 PID` | Zorla öldürür, engellenemez. |
 
---
 
## Derleme ve Çalıştırma

### Derleme

```bash
make
```

### Çalıştırma

**Terminal 1 — Server'ı başlat:**
```bash
./server
# Server PID: 4891  ← burda çıkan numarayı kopyalayın.
```

**Terminal 2 — Client ile mesaj gönder:**
```bash
./client 4891 "Merhaba Dünya"
```

Server terminalinde mesaj görünür:
```
Merhaba Dünya
```

---

## Temizleme

```bash
make clean    # binary dosyaları siler.
make re       # temizler ve yeniden derler.
```

---

##  Önemli Notlar

- Server önce başlatılmalıdır, aksi takdirde client nereye sinyal göndereceğini bilemez.
- Server `Ctrl+C` ile durdurulabilir (`SIGINT` sinyali).
- Yalnızca ASCII karakterler (0-127) desteklenmektedir.
