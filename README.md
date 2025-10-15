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

### Diagram Koneksi Sederhana
