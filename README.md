# raspy-ultrasonic-distance

| Nama                         | Nrp        |
| ---------------------------- | ---------- |
| Oscaryavat Viryavan          | 5027241053 |
| Ahsani Rahman  | 5027241099 |

---

## Komponen yang Digunakan

| No | Komponen | Jumlah | Keterangan |
|----|-----------|---------|------------|
| 1 | Raspberry Pi (3/4/Zero) | 1 | Sebagai pengendali utama |
| 2 | Sensor Ultrasonik HC-SR04 | 1 | Mengukur jarak objek menggunakan gelombang ultrasonik |
| 3 | Kabel Jumper | 4 | Untuk koneksi antar pin |
| 4 | Breadboard | 1 | (Opsional) untuk mempermudah penyusunan rangkaian |

---

## Langkah-Langkah Rangkaian

### Pin HC-SR04

| Pin HC-SR04 | Deskripsi | Terhubung ke Pin Raspberry Pi |
|--------------|------------|-------------------------------|
| VCC | Tegangan 5V | 5V (pin 2 atau 4) |
| GND | Ground | GND (pin 6) |
| TRIG | Pemicu sinyal ultrasonik | GPIO 23 (pin fisik 16) |
| ECHO | Menerima pantulan sinyal | GPIO 24 (pin fisik 18) |

### Koneksi Sederhana         
 
 GPIO23 (pin16) ----> TRIG (HC-SR04) <br>
 GPIO24 (pin18) <---- ECHO (HC-SR04) <br>
 5V (pin2)      ----> VCC <br>
 GND (pin6)     ----> GND <br>

 ## 🧾 Penjelasan Kode

### 1️ Import Library dan Inisialisasi Pin
```python
import RPi.GPIO as GPIO
import time

TRIG = 23
ECHO = 24
```
- RPi.GPIO: library resmi Raspberry Pi untuk mengontrol pin GPIO.
- Pin TRIG (GPIO23) digunakan untuk mengirim sinyal ultrasonik.
- Pin ECHO (GPIO24) digunakan untuk menerima pantulan sinyal.

### 2️ Pengaturan Mode GPIO
```python
GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)
GPIO.setmode(GPIO.BCM) → menggunakan nomor pin berbasis GPIO, bukan nomor fisik.
```
- GPIO.OUT → TRIG akan mengirim sinyal.
- GPIO.IN → ECHO akan menerima sinyal.

### 3️ Mengirim Pulsa Ultrasonik
```python
GPIO.output(TRIG, False)
time.sleep(0.5)

GPIO.output(TRIG, True)
time.sleep(0.00001)
GPIO.output(TRIG, False)
```
- Menonaktifkan TRIG selama 0.5 detik agar sensor stabil.
- Memberi pulsa HIGH selama 10 mikrodetik untuk mengaktifkan sensor.
- Setelah itu TRIG dimatikan (LOW).

### 4️ Menerima Pantulan Sinyal
```python
while GPIO.input(ECHO) == 0:
    pulse_start = time.time()
while GPIO.input(ECHO) == 1:
    pulse_end = time.time()
```
- Saat ECHO = LOW, sensor menunggu gelombang dipantulkan kembali.
- Saat ECHO = HIGH, sensor sedang menerima pantulan.
- Waktu antara pulse_start dan pulse_end digunakan untuk menghitung jarak.

### 5️ Menghitung Jarak
```python
pulse_duration = pulse_end - pulse_start
distance = pulse_duration * 17150  # kecepatan suara/2
distance = round(distance, 2)
```
Rumus jarak:

<img width="488" height="86" alt="Screenshot 2025-10-15 115600" src="https://github.com/user-attachments/assets/a07e30c8-4950-4c09-a865-4c9cb973ec7a" />

- Kecepatan suara di udara ≈ 34300 cm/s, dibagi dua karena gelombang berangkat dan kembali.
- Hasil dibulatkan menjadi dua angka desimal.

### 6️ Menampilkan Hasil
```python
print("Distance:", distance, "cm")
```
- Menampilkan hasil pembacaan jarak secara real-time di terminal.

### 7️ Menghentikan Program dengan Aman
```python
except KeyboardInterrupt:
    print("\nStopping program.")
    GPIO.cleanup()
```

- Menangkap perintah Ctrl + C agar program berhenti dengan aman.
- GPIO.cleanup() memastikan semua pin GPIO dikembalikan ke kondisi default.

### Cara Menjalankan Program
- Pastikan Raspberry Pi sudah menyala dan kamu masuk ke terminal.
- Simpan kode di file bernama ultrasonic.py.

Jalankan perintah berikut:
```
bash
python3 ultrasonic.py
```

Contoh output:

```Distance: 21.45 cm
Distance: 10.32 cm
Distance: 7.80 cm
```

<img src="https://github.com/user-attachments/assets/0b7e7ede-cc88-487a-a42d-881d7438714a" width = "400" height="700">
