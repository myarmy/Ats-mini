# ATS-Mini TR

ATS-Mini (ESP32-S3 + SI4732) alıcısı için sıfırdan yazılmış, Türkçe arayüzlü temiz firmware ve Wi-Fi uzaktan kontrol paneli.

**Durum: Faz 1, henüz cihazda denenmedi.** Kod derlenip donanımda doğrulanmadı. Hata çıkarsa çıktıyı paylaşın, birlikte düzeltelim.

## Özellikler (Faz 1)
- FM ve AM (MW + 7 kısa dalga bandı), frekans, tarama, ses, AGC, AM bant genişliği
- Cihaz kendi Wi-Fi ağını açar: **ATS-Mini-TR** / şifre **12345678** (değiştirmek için `firmware/src/config.h`)
- Tarayıcıdan `http://192.168.4.1` adresinde Türkçe kontrol paneli
- Kodlayıcı: döndür = ayar, kısa bas = Frekans → Ses → Bant
- Ayarlar (bant, frekans, ses) kaydedilir

## Henüz yok (sonraki fazlar)
- LSB / USB / CW (SSB yaması gerekir), spektrum taraması, ev Wi-Fi’sine bağlanma, Türkçe karakterli LCD yazı tipi
- Ses akışı: ses cihazın hoparlöründen/jakından çıkar, tarayıcıya aktarılmaz

## Derleme (bilgisayara kurulum gerekmez)
1. Bu dosyaları GitHub deponuza yükleyin.
2. **Actions** sekmesinde “Firmware derle” çalışır.
3. Bittiğinde sayfanın altındaki **Artifacts → ats-mini-tr-bin** dosyasını indirin.

## Yüklemeden önce YEDEK alın
```
esptool.py --chip esp32s3 -p COM5 read_flash 0 0x1000000 yedek-16mb.bin
```
(COM5 yerine kendi portunuz. Linux/Mac: /dev/ttyACM0)

## Temiz yükleme (eski yazılımdan kalıntı bırakmaz)
```
esptool.py --chip esp32s3 -p COM5 erase_flash
esptool.py --chip esp32s3 -p COM5 write_flash 0x0 ats-mini-tr-tam.bin
```
Bootloader moduna girmek için kodlayıcı tuşuna basılı tutarak USB’yi takın (BOOT = GPIO0).

## Ekran sorun giderme
`firmware/src/display.h` ve `config.h` içinde: `LCD_ROTATION`, `offset_x`, `invert`. Pil yüzdesi bölücü oranı varsayımdır.

## Pin haritası
Kaynak: esp32-si4732.github.io/ats-mini/hardware.html (ekran 8-bit paralel ST7789). Kod bu projeye özgündür; SI4735 (PU2CLR) ve LovyanGFX kütüphaneleri bağımlılık olarak indirilir, lisansları kendilerine aittir.
