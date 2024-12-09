[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/4ZAJL3PP)

# Laporan Akhir Final Project OOP D

## 1. Informasi Umum

A. **Nama Game** : Weapmath - War of Math and Culture

B. **Anggota Kelompok** :

1. **Alvin Zanua Putra** - 5025231064
2. **Adrian Aziz Santoso** - 5025221229
3. **Muhammad Farrel Fathin Wibowo** - 5025231233

C. **Tech Stack**:

[Bahasa Pemrograman C#, Framework Unity API, Tools yang digunakan adalah Unity Editor, Visual Studio Code Editor, Assets Store Itch Io]

# 2. Deskripsi Game

## 2.1 Konsep Game

**Genre** : Educational Game Battle dengan tema Budaya Indonesia
**Gameplay/Rule**:

Game bertemakan perang adat yang menggabungkan unsur matematika dan budaya Indonesia. Pemain dapat memilih karakter dengan pakaian adat dari berbagai daerah. Setiap pertarungan melibatkan pemecahan soal matematika. Tingkat kesulitan soal disesuaikan dengan level kelas pemain (SMP).

- **Objective** :

  Mengalahkan lawan dengan menjawab soal matematika dengan benar
  Mengumpulkan koin untuk membuka karakter dan rumah adat baru
  Mempelajari budaya Indonesia melalui visualisasi karakter dan bangunan adat hingga soal matematika

- **Single/Multi Player** :

  - Single Player: Melawan Bot komputer yang di program sendiri
  - Multi Player: Pertarungan 1 vs 1 secara online

## 2.2 Fitur Utama

1. **Fitur Single Player Mode** :

   - Pemain dapat berlatih melawan komputer
   - Tingkat kesulitan soal matematika adaptif
   - Sistem reward berupa koin untuk setiap kemenangan

2. **Fitur Multi Player Mode** :

   - Sistem matchmaking online
   - Pertarungan realtime antar pemain
   - Host server untuk koneksi antar pemain

3. **Sistem Ekonomi & Progression** :

   - Sistem koin sebagai mata uang dalam game
   - Sistem unlock karakter dan rumah adat
   - Inapp purchase untuk mendapatkan koin tambahan
   - Achievement system

4. **Sistem Profil Pemain** :

   - Customizable username
   - Pemilihan gender
   - Pemilihan tingkat kelas
   - Unique ID generation dengan base64 encoding
   - Tracking statistik pemain

5. **Fitur Kustomisasi** :

   - Berbagai pilihan karakter dengan pakaian adat
   - Pilihan rumah adat dari berbagai daerah
   - Sistem unlock berbasis koin
   - Preview karakter dan rumah adat

# 3. Implementasi Fitur Wajib

## 3.1 Save/Load System

## A. **Implementasi** :

1. Fitur Untuk **menyimpan data** di bagian profile player berupa username, id, gender, tingkat kelas.
2. Fitur Untuk **menyimpan data karakter** yang dipilih oleh user saat ini.
3. Fitur Untuk **menyimpan rumah adat** yang dipilih oleh user saat ini.
4. Fitur Untuk **menyimpan koin atau achievement** yang didapatkan user setelah memenangkan game.
5. Fitur Untuk **menyimpan koin top up** yang dilakukan user melalui Google Play yaitu app in purchase.
6. Fitur Untuk **menyimpan audio settings** yang dipilih oleh user saat ini.

## B. **Konsep OOP**:

1.  **Encapsulation**

        - useran access modifiers (public, private, protected)
        - Data hiding untuk variabel sensitif
        - Getter dan setter methods untuk kontrol akses

2.  **Inheritance**

        - Base class untuk karakter
        - Child classes untuk variasi karakter
        - Inheritance dari MonoBehaviour

3.  **Polymorphism**

        - Override methods untuk behavior spesifik
        - Interface implementation untuk system handlers

## C. **Penerapan SOLID**:

1.  **Single Responsibility Principle**

        - Kelas SaveLoader fokus pada penyimpanan data
        - Kelas AudioSettings khusus menangani audio
        - Kelas UsernameChange spesifik untuk manajemen username

2.  **Open/Closed Principle**

        - Sistem karakter yang extensible
        - Modular achievement system

3.  **Interface Segregation**

        - User interface IDetailedStoreListener
        - Pemisahan concern untuk network functionality

## D. **Code Snippet**:

### **1. SaveLoader.cs** :

```bash
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;
using UnityEngine.UI;

public class SaveLoader : MonoBehaviour
{
    const string glyphs= "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()";

    [SerializeField] GameObject c1, c2, c3, c4, c5, c6, c7, c8, c9;
    [SerializeField] GameObject h1, h2, h3;
    [SerializeField] GameObject sc1, sc2, sc3, sc4, sc5, sc6, sc7, sc8, sc9;
    [SerializeField] GameObject sh1, sh2, sh3;
    [SerializeField] GameObject checkmark;
    [SerializeField] TextMeshProUGUI namesText;
    [SerializeField] TextMeshProUGUI gradeText;
    [SerializeField] TextMeshProUGUI idText;
    [SerializeField] TextMeshProUGUI genderText;
    [SerializeField] TextMeshProUGUI coinAmountText;
    [SerializeField] Button buyButton;
    [SerializeField] int c2price, c3price, c4price, c5price, c6price, c7price, c8price, c9price;
    [SerializeField] TextMeshProUGUI priceText;
    int charIndex = 0;
    int houseIndex = 0;
    bool isHouse = false;
    bool c2Own = false;
    bool c3Own = false;
    bool c4Own = false;
    bool c5Own = false;
    bool c6Own = false;
    bool c7Own = false;
    bool c8Own = false;
    bool c9Own = false;
    bool h2Own = false;
    bool h3Own = false;
    public bool proVersion = false;
    void Start()
    {
        charIndex = PlayerPrefs.GetInt("charIndex", 0);
        Debug.Log(charIndex);
        if (charIndex < 0 || charIndex > 8) {
            charIndex = 0;
        }
        houseIndex = PlayerPrefs.GetInt("houseIndex", 0);
        if (houseIndex < 0 || houseIndex > 2) {
            houseIndex = 0;
        }
        Debug.Log(charIndex);
        loadOwnedCharacters();
        applyChange();
        changeGrade(PlayerPrefs.GetInt("grade", 7));
        changeGender(PlayerPrefs.GetString("gender", "..."));
        idAssign();
        updateCoinAmount();
    }

    void Update(){
        if (isHouse){
            changeHouse();
        }
        else if (!isHouse) {
            changeChar();
        }
        selTextEnabler();
    }

    public void updateCoinAmount(){
        int coinOwned = PlayerPrefs.GetInt("coinOwned", 0);
        coinAmountText.text = coinOwned.ToString();
    }

    public void buyItem(){
        int coinOwned = PlayerPrefs.GetInt("coinOwned", 0);
        if (!isHouse){
            if (charIndex == 1){
                if (coinOwned >= c2price){
                    coinOwned = c2price;
                    PlayerPrefs.SetInt("isOwnC2", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 2) {
                if (coinOwned >= c3price){
                    coinOwned = c3price;
                    PlayerPrefs.SetInt("isOwnC3", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 3) {
                if (coinOwned >= c4price){
                    coinOwned = c4price;
                    PlayerPrefs.SetInt("isOwnC4", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 4) {
                if (coinOwned >= c5price){
                    coinOwned = c5price;
                    PlayerPrefs.SetInt("isOwnC5", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 5) {
                if (coinOwned >= c6price){
                    coinOwned = c6price;
                    PlayerPrefs.SetInt("isOwnC6", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 6) {
                if (coinOwned >= c7price){
                    coinOwned = c7price;
                    PlayerPrefs.SetInt("isOwnC7", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 7) {
                if (coinOwned >= c8price){
                    coinOwned = c8price;
                    PlayerPrefs.SetInt("isOwnC8", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (charIndex == 8) {
                if (coinOwned >= c9price){
                    coinOwned = c9price;
                    PlayerPrefs.SetInt("isOwnC9", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
        }
        else if (isHouse) {
            if (houseIndex == 1) {
                if (coinOwned >= 250){
                    coinOwned = 250;
                    PlayerPrefs.SetInt("isOwnH2", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
            else if (houseIndex == 2) {
                if (coinOwned >= 250){
                    coinOwned = 250;
                    PlayerPrefs.SetInt("isOwnH3", 1);
                    PlayerPrefs.SetInt("coinOwned", coinOwned);
                    updateCoinAmount();
                }
            }
        }
        loadOwnedCharacters();
    }

    private void loadOwnedCharacters() {
        int isOwnC2 = PlayerPrefs.GetInt("isOwnC2", 0);
        int isOwnC3 = PlayerPrefs.GetInt("isOwnC3", 0);
        int isOwnC4 = PlayerPrefs.GetInt("isOwnC4", 0);
        int isOwnC5 = PlayerPrefs.GetInt("isOwnC5", 0);
        int isOwnC6 = PlayerPrefs.GetInt("isOwnC6", 0);
        int isOwnC7 = PlayerPrefs.GetInt("isOwnC7", 0);
        int isOwnC8 = PlayerPrefs.GetInt("isOwnC8", 0);
        int isOwnC9 = PlayerPrefs.GetInt("isOwnC9", 0);
        if (isOwnC2 == 1 || proVersion) c2Own = true;
        if (isOwnC3 == 1 || proVersion) c3Own = true;
        if (isOwnC4 == 1 || proVersion) c4Own = true;
        if (isOwnC5 == 1 || proVersion) c5Own = true;
        if (isOwnC6 == 1 || proVersion) c6Own = true;
        if (isOwnC7 == 1 || proVersion) c7Own = true;
        if (isOwnC8 == 1 || proVersion) c8Own = true;
        if (isOwnC9 == 1 || proVersion) c9Own = true;

        int isOwnH2 = PlayerPrefs.GetInt("isOwnH2", 0);
        int isOwnH3 = PlayerPrefs.GetInt("isOwnH3", 0);
        if (isOwnH2 == 1 || proVersion) h2Own = true;
        if (isOwnH3 == 1 || proVersion) h3Own = true;
    }

    private void idAssign(){
        string idGet = PlayerPrefs.GetString("id", "0");
        if (idGet == "0"){
            setID();
        }
        idText.text = "ID: " + PlayerPrefs.GetString("id");
    }

    private void setID(){
        string myString = "";
        for(int i = 0; i < 12; i++){
            myString += glyphs[Random.Range(0, glyphs.Length)];
        }
        PlayerPrefs.SetString("id", myString);
    }

    public void changeGrade(int gr){
        PlayerPrefs.SetInt("grade", gr);
        gradeText.text = "Kelas " + gr;
    }

    public void changeGender(string gender){
        PlayerPrefs.SetString("gender", gender);
        genderText.text = gender;
    }

    public void next() {
        if(!isHouse){
            charIndex = charIndex + 1;
            charIndex = charIndex % 9;
        }
        else if(isHouse){
            houseIndex = houseIndex + 1;
            houseIndex = houseIndex % 3;
        }
    }

    public void prev() {
        if(!isHouse){
            charIndex = charIndex  1;
            if (charIndex < 0) {
                charIndex = 8;
            }
        }
        else if (isHouse){
            houseIndex = houseIndex  1;
            if (houseIndex < 0) {
                houseIndex = 2;
            }
        }
    }

    public void changeTab(){
        isHouse = !isHouse;
    }

    public void applyChange(){
        print(charIndex);
        switch (charIndex) {
            case 0:
                disableChar();
                c1.SetActive(true);
                PlayerPrefs.SetInt("charIndex", charIndex);
                break;
            case 1:
                if (c2Own)  {
                    disableChar();
                    c2.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 2:
                if (c3Own){
                    disableChar();
                    c3.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 3:
                if (c4Own){
                    disableChar();
                    c4.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 4:
                if (c5Own){
                    disableChar();
                    c5.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 5:
                if (c6Own){
                    disableChar();
                    c6.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 6:
                if (c7Own){
                    disableChar();
                    c7.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 7:
                if (c8Own){
                    disableChar();
                    c8.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            case 8:
                if (c9Own){
                    disableChar();
                    c9.SetActive(true);
                    PlayerPrefs.SetInt("charIndex", charIndex);
                }
                break;
            default:
                break;
        }
        switch (houseIndex) {
            case 0:
                disableHouse();
                h1.SetActive(true);
                PlayerPrefs.SetInt("houseIndex", houseIndex);
                break;
            case 1:
                if (h2Own) {
                    disableHouse();
                    h2.SetActive(true);
                    PlayerPrefs.SetInt("houseIndex", houseIndex);
                }
                break;
            case 2:
                if (h3Own) {
                    disableHouse();
                    h3.SetActive(true);
                    PlayerPrefs.SetInt("houseIndex", houseIndex);
                }
                break;
            default:
                break;
        }
    }


    private void disableHouse() {
        h1.SetActive(false);
        h2.SetActive(false);
        h3.SetActive(false);
    }
    private void disableChar() {
        c1.SetActive(false);
        c2.SetActive(false);
        c3.SetActive(false);
        c4.SetActive(false);
        c5.SetActive(false);
        c6.SetActive(false);
        c7.SetActive(false);
        c8.SetActive(false);
        c9.SetActive(false);
    }

    private void selTextEnabler(){
        if (isHouse){
            if (houseIndex == PlayerPrefs.GetInt("houseIndex")){
                checkmark.SetActive(true);
            }
            else if (houseIndex != PlayerPrefs.GetInt("houseIndex")){
                checkmark.SetActive(false);
            }
        }
        if (!isHouse){
            if (charIndex == PlayerPrefs.GetInt("charIndex")){
                checkmark.SetActive(true);
            }
            else if (charIndex != PlayerPrefs.GetInt("charIndex")){
                checkmark.SetActive(false);
            }
        }
    }

    private void changeHouse() {
        priceText.text = "250";
        switch (houseIndex) {
            case 0:
                namesText.text = "kalimantan";
                sh1.SetActive(true);
                sh2.SetActive(false);
                sh3.SetActive(false);
                buyButton.gameObject.SetActive(false);
                break;
            case 1:
                namesText.text = "sulawesi";
                sh1.SetActive(false);
                sh2.SetActive(true);
                sh3.SetActive(false);

                if (!h2Own){
                    sh2.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (h2Own){
                    sh2.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 2:
                namesText.text = "jawa";
                sh1.SetActive(false);
                sh2.SetActive(false);
                sh3.SetActive(true);
                if (!h3Own){
                    sh3.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (h3Own){
                    sh3.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            default:
                break;
        }
    }

    private void changeChar() {
        switch (charIndex) {
            case 0:
                namesText.text = "jawa barat";
                sc1.SetActive(true);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(false);
                buyButton.gameObject.SetActive(false);
                break;
            case 1:
                namesText.text = "sumatera barat";
                sc1.SetActive(false);
                sc2.SetActive(true);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(false);
                priceText.text = c2price.ToString();
                if (!c2Own){
                    sc2.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c2Own){
                    sc2.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 2:
                namesText.text = "sumatera utara";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(true);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(false);
                priceText.text = c3price.ToString();
                if (!c3Own){
                    sc3.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c3Own){
                    sc3.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 3:
                namesText.text = "palembang (L)";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(true);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(false);
                priceText.text = c4price.ToString();
                if (!c4Own){
                    sc4.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c4Own){
                    sc4.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 4:
                namesText.text = "palembang (P)";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(true);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(false);
                priceText.text = c5price.ToString();
                if (!c5Own){
                    sc5.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c5Own){
                    sc5.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 5:
                namesText.text = "betawi (L)";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(true);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(false);
                priceText.text = c6price.ToString();
                if (!c6Own){
                    sc6.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c6Own){
                    sc6.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 6:
                namesText.text = "betawi (P)";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(true);
                sc8.SetActive(false);
                sc9.SetActive(false);
                priceText.text = c7price.ToString();
                if (!c7Own){
                    sc7.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c7Own){
                    sc7.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 7:
                namesText.text = "sunda (L)";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(true);
                sc9.SetActive(false);
                priceText.text = c8price.ToString();
                if (!c8Own){
                    sc8.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c8Own){
                    sc8.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            case 8:
                namesText.text = "sunda (P)";
                sc1.SetActive(false);
                sc2.SetActive(false);
                sc3.SetActive(false);
                sc4.SetActive(false);
                sc5.SetActive(false);
                sc6.SetActive(false);
                sc7.SetActive(false);
                sc8.SetActive(false);
                sc9.SetActive(true);
                priceText.text = c9price.ToString();
                if (!c9Own){
                    sc9.GetComponent<SpriteRenderer>().color = Color.black;
                    buyButton.gameObject.SetActive(true);
                }
                else if (c9Own){
                    sc9.GetComponent<SpriteRenderer>().color = Color.white;
                    buyButton.gameObject.SetActive(false);
                }
                break;
            default:
                break;
        }
    }
}
```

### Penjelasan Code :

`Class: SaveLoader : Untuk menyimpan, memuat, dan mengelola data pemain, seperti karakter, rumah, dan item yang dimiliki saat ini. `

`1. Start() : Fungsi dipanggil ketika ada objek inisialisasi yaitu Memuat data yang tersimpan (PlayerPrefs) seperti indeks karakter dan rumah, Memastikan indeks valid, mastikan indexnya valid, Memuat status kepemilikan item (loadOwnedCharacters()), Menerapkan perubahan UI (applyChange()), Memperbarui tampilan data pemain (kelas, gender, koin)`

`2. Update() : Untuk Mengubah tampilan rumah atau karakter tergantung pada tab yang aktif, nampilin status seleksi dengan checkmark (selTextEnabler()).`

`3. updateCoinAmount() : Fungsi publik untuk memperbarui jumlah koin yang ditampilkan, Data diambil dari PlayerPrefs dan diperbarui ke UI (coinAmountText).`

`4. buyItem() : ungsi publik untuk membeli karakter atau rumah, logikanya Mengecek apakah tab aktif adalah karakter atau rumah. Cek apa koin yang dimiliki cukup. Jika cukup, item ditandai sebagai dimiliki (PlayerPrefs.SetInt) dan koin diperbarui. Refresh status kepemilikan item (loadOwnedCharacters()).`

`5. loadOwnedCharacters() : Fungsi private untuk memuat status kepemilikan karakter dan rumah, Data diambil dari PlayerPrefs dan disimpan dalam variabel boolean. Jika item dimiliki atau versi pro, variabel boolean diubah menjadi true.`

`6. idAssign() : Fungsi private untuk menetapkan ID pemain, Data diambil dari PlayerPrefs. Jika ID belum ada, ID baru dibuat dan disimpan.`

`7. setID() : Fungsi private untuk membuat ID baru, ID terdiri dari 12 karakter acak dari variabel glyphs. ID disimpan di PlayerPrefs.`

`8. changeGrade() : Fungsi publik untuk mengubah kelas pemain, Data disimpan di PlayerPrefs dan diperbarui ke UI (gradeText).`

`9. changeGender() : Fungsi publik untuk mengubah gender pemain, ungsi publik untuk mengubah dan menyimpan gender pemain. Data gender disimpan di PlayerPrefs dan diperbarui ke UI (genderText).`

`10. next() : next() Fungsi publik untuk mengubah indeks karakter/rumah ke item berikutnya pake index modulo 9 karena ada 3 rumah dan 9 karakter. `

`11. prev() : Fungsi publik untuk mengubah indeks karakter/rumah ke item sebelumnya. `

`12. changeTab() : Fungsi publik untuk mengubah tab antara karakter dan rumah. `

`13. applyChange() : Fungsi publik untuk menerapkan perubahan karakter atau rumah, memeriksa indeks karakter dan rumah yang dipilih dan menampilkan karakter atau rumah. `

`14. disableHouse() : Fungsi privat untuk menonaktifkan semua rumah (menyembunyikannya). `

`15. disableChar() : Fungsi privat untuk menonaktifkan karakter (menyembunyikannya karena tidak perlu diketahui user). `

`16. selTextEnabler() : Fungsi privat untuk menampilkan status seleksi dengan checkmark, menampilkan checkmark jika item dipilih. `

`17. changeHouse() : Fungsi privat untuk mengubah tampilan rumah, menampilkan rumah yang dipilih dan harga rumah. `

`18. changeChar() : Fungsi privat untuk mengubah tampilan karakter, menampilkan karakter yang dipilih dan harga karakter. `

### Rangkuman :

A. Prinsip OOP yang diterapkan:

1. Encapsulation

   - Fungsi Privat:
     - loadOwnedCharacters(): Mengelola status kepemilikan item tanpa langsung mengekspos data internal.
     - disableHouse() dan disableChar(): Menyembunyikan logika untuk menonaktifkan objek dari luar class.
     - Keuntungan: Detail implementasi tersembunyi, memudahkan pemeliharaan kode.
   - Fungsi Publik:
     - buyItem(): Hanya mengekspos cara membeli item tanpa menunjukkan bagaimana item diatur di PlayerPrefs.

2. Inheritance
   Pada class SaveLoader, inheritance tidak diterapkan secara eksplisit karena class ini berdiri sendiri. Namun, SaveLoader mewarisi metode dasar dari MonoBehaviour (class bawaan Unity), seperti Start() dan Update().
   Contohnya : MonoBehaviour.Start() di-overriding untuk memuat data awal.

3. Polymorphism
   Dalam SaveLoader, ini terlihat pada cara fungsi menangani objek berbeda berdasarkan konteks, seperti tab karakter atau rumah. Contohnya :

   - buyItem(): - Menggunakan logika polymorphism untuk menentukan apakah pembelian berlaku untuk karakter atau rumah berdasarkan tab yang aktif. - Menggunakan kondisi if (tabCharacter) dan else untuk menangani dua jenis objek.
   - Dynamic Method Calls: - Fungsi seperti changeTab() dan applyChange() memanggil fungsi berbeda tergantung konteks (karakter atau rumah).

4. Abstraction
   SaveLoader menyembunyikan detail implementasi seperti cara menyimpan data di PlayerPrefs, Fungsi-fungsi publik hanya mengekspos cara mengakses data tanpa menunjukkan detailnya. Contohnya :
   - applyChange():
     Menyembunyikan detail bagaimana indeks diubah dan hanya memberikan akses untuk memperbarui tampilan.
   - selTextEnabler():
     Abstraksi logika menampilkan atau menyembunyikan status seleksi.

B. Prinsip SOLID yang diterapkan :

1. Single Responsibility Principle (SRP)
   yaitu setiap class atau fungsi harus memiliki satu tanggung jawab spesifik.
   Contohnya di bagian : - buyItem():
   Hanya menangani logika pembelian item. - updateCoinAmount():
   Hanya menangani pembaruan jumlah koin. - Fungsi-fungsi disableHouse() dan disableChar():
   Cuma Fokus pada penonaktifkan semua rumah atau karakter.
   Kekurangan:
   Class SaveLoader melanggar SRP karena menangani terlalu banyak tanggung jawab sekaligus: Pengelolaan data karakter dan rumah, Interaksi UI, Pengelolaan data pemain di PlayerPrefs.

2. Open/Closed Principle (OCP)
   Class harus terbuka untuk ekstensibilitas, tetapi tertutup untuk modifikasi langsung.
   Contoh di bagian: - PlayerPrefs:
   Data disimpan di PlayerPrefs, yang mungkin perubahan eksternal tanpa memodifikasi logika intinya.

## **2. UsernameChange.cs** :

```bash
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;
using UnityEngine.UI;

public class UsernameChange : MonoBehaviour
{
    [SerializeField] TMP_InputField usernameInput;
    [SerializeField] TextMeshProUGUI username;
    [SerializeField] TextMeshProUGUI usernameProfile;

    private void Start() {
        string savedUsername = PlayerPrefs.GetString("username");
        if (savedUsername != "") {
            username.text = savedUsername;
            usernameProfile.text = savedUsername;
        }
    }

    public void changeUsername() {
        if (usernameInput != null) {
            PlayerPrefs.SetString("username", usernameInput.text);
            username.text = usernameInput.text;
            usernameProfile.text = usernameInput.text;
        }
    }
}
```

### Penjelasan Code :

`Untuk Field(Vairiablenya) : `<br>

`[SerializeField] TMP_InputField usernameInput Merupakan referensi ke input field tempat user memasukkan nama user baru.` <br>
`[SerializeField] TextMeshProUGUI username Menyimpan referensi ke elemen teks di UI yang menampilkan nama user saat ini.`<br>
`[SerializeField] TextMeshProUGUI usernameProfile Sama seperti username, tetapi digunakan untuk menampilkan nama user di bagian profil.`

`1. Start() : Fungsi untuk memuat nama user yang tersimpan dari PlayerPrefs, Jika nama user ada, nama user ditampilkan di UI.`<br>
`2. changeUsername() : Fungsi publik untuk mengubah nama user, Menyimpan nama user baru ke PlayerPrefs, Memperbarui nama user di UI.`

### Rangkuman :

A. Prinsip OOP yang diterapkan:

1. Encapsulation

- [SerializeField]: Field usernameInput, username, dan  
  usernameProfile dibuat private dan hanya dapat diatur melalui editor Unity. Ini menjaga agar properti tersebut tidak dapat diakses langsung oleh class lain.
  - Fungsi changeUsername() untuk memperbarui nama user, hanya dapat diakses dari luar class dan tidak memerlukan akses langsung ke variabel usernameInput.

2. Inheritance
   Inheritance tidak diterapkan secara eksplisit pada class ini. Tapi, class ini ada warisanya dari MonoBehaviour. Yaitu buat memungkinkan UsernameChange untuk memanfaatkan fungsi bawaan Unity seperti Start().
3. Abstraction
   Abstraction diterapkan dalam fungsi: changeUsername():
   Ada interface sederhana untuk mengganti username tanpa menunjukkan logika pengelolaan data di dalamnya, tapi ini masih terikat langsung dengan PlayerPrefs.

B. Prinsip SOLID yang diterapkan :

1. Single Responsibility Principle (SRP)

   - Mengelola perubahan nama user.
   - Menyimpan dan memuat nama user dari PlayerPrefs.

2. Dependency Inversion Principle (DIP)
   Pelanggaran: Class ini bergantung langsung pada PlayerPrefs, yang membuatnya sulit untuk diuji atau diubah penyimpanan datanya.

## **3. AudioSettings.cs** :

```bash
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class AudioSettings : MonoBehaviour
{
    public bool musicEnabled;
    public bool sfxEnabled;
    [SerializeField] AudioSource msc, sfx;
    [SerializeField] Toggle mscT, sfxT;

    // Start is called before the first frame update
    void Start()
    {
        int mscE = PlayerPrefs.GetInt("msc", 1);
        int sfxE = PlayerPrefs.GetInt("sfx", 1);
        if (mscE == 1){
            musicEnabled = true;
            mscT.isOn = true;
        }
        else if (mscE == 0){
            musicEnabled = false;
            mscT.isOn = false;
        }
        if (sfxE == 1){
            sfxEnabled = true;
            sfxT.isOn = true;
        }
        else if (sfxE == 0){
            sfxEnabled = false;
            sfxT.isOn = false;
        }
    }

    // Update is called once per frame
    void Update()
    {

        if (musicEnabled){
            msc.enabled = true;
        }
        else if (!musicEnabled){
            msc.enabled = false;
        }

        if (sfxEnabled){
            sfx.enabled = true;
        }
        else if (!sfxEnabled){
            sfx.enabled = false;
        }
    }

    public void changeMsc(bool enabler){
        musicEnabled = enabler;
        if (musicEnabled) {
            PlayerPrefs.SetInt("msc", 1);
        }
        else if (!musicEnabled) {
            PlayerPrefs.SetInt("msc", 0);
        }
    }

    public void changeSfx(bool enabler){
        sfxEnabled = enabler;
        if (sfxEnabled) {
            PlayerPrefs.SetInt("sfx", 1);
        }
        else if (!sfxEnabled) {
            PlayerPrefs.SetInt("sfx", 0);
        }
    }
}
```

### Penjelasan Code :

`1. Saat aplikasi dimulai:`<br>
`- Start() memuat pengaturan musik dan efek suara dari PlayerPrefs.`
`- Toggle UI/tombolnya diperbarui agar sesuai dengan pengaturan.`

`2. Saat pengguna mengubah toggle musik atau efek suara:`<br>
`- Fungsi changeMsc() atau changeSfx() dipanggil melalui event handler toggle.`
`- Status pengaturan diperbarui di PlayerPrefs.`

`3. Saat aplikasi berjalan:`<br>
`- Update() secara dinamis menyesuaikan status AudioSource berdasarkan pengaturan yang dipilih pengguna.`

### Rangkuman :

A. Prinsip OOP yang diterapkan:

1. Encapsulation
   - Field seperti msc dan mscT diberi atribut [SerializeField], sehingga hanya dapat diakses melalui editor Unity atau metode di dalam class.
   - Buat membatasi akses langsung dan melindungi integritas datanya.
2. Abstraction
   - Seperti penyimpanan pengaturan di PlayerPrefs disembunyikan dari pengguna class.
   - Namun, abstraksi dapat ditingkatkan dengan memisahkan logika penyimpanan dari class ini.

B. Prinsip SOLID yang diterapkan :

1. Single Responsibility Principle (SRP)
   - Class ini memiliki satu tanggung jawab: mengelola pengaturan audio.
   - Semua fungsi berfokus pada pengaturan musik dan efek suara, tanpa mencampur tugas lainnya.

## 4. **Player Controller.cs** :

```bash
using System.Collections;
using System.Collections.Generic;
using Unity.Netcode;
using UnityEngine;

public class PlayerChanger : NetworkBehaviour
{
    GameManager gm;

    void Start()
    {
        gm = (GameManager)FindObjectOfType(typeof(GameManager));
        if (IsServer)
        {
            gm.changePlayer(1);
            gm.startMatch();
        }
        else if (IsClient)
        {
            gm.changePlayer(2);
        }
        gm.waitingPlayer();
    }
}
```

### Penjelasan Code :

`1. Start() : Fungsi ini dipanggil ketika script pertama kali dijalankan, Mengambil referensi ke class GameManager menggunakan FindObjectOfType(), Memeriksa apakah instance saat ini adalah server atau client menggunakan IsServer dan IsClient dari Unity Netcode:` 

- `Jika server:`
`Memanggil changePlayer(1) untuk mengatur pemain ke tipe "1", Memulai pertandingan dengan startMatch().` 

- `Jika client:`
`Memanggil changePlayer(2) untuk mengatur pemain ke tipe "2".`

`2. Memanggil waitingPlayer() untuk melakukan pengaturan atau tindakan saat menunggu pemain lain.`

### Rangkuman :

A. Prinsip OOP yang diterapkan:

1. Encapsulation
   Tidak ada variabel yang diberi visibilitas terbatas (private atau protected). Variabel gm harus dienkapsulasi untuk melindungi integritasnya.
2. Abstraction
   Interaksi dengan GameManager dilakukan melalui metode seperti changePlayer() dan startMatch(). Ini menyembunyikan detail implementasi.
3. Inheritance dan Polymorphism
   Pewarisan dari NetworkBehaviour memungkinkan script untuk berfungsi dalam ekosistem Unity Netcode.

B. Prinsip SOLID yang diterapkan :

1. Single Responsibility Principle (SRP)
   Class ini memiliki tanggung jawab tunggal untuk mengatur perubahan pemain di dalam game.
2. Open/Closed Principle (OCP)
   Class tidak terbuka untuk ekspansi tanpa memodifikasi kode, karena fungsi seperti changePlayer() langsung dikodekan ke tipe pemain.
3. Dependency Inversion Principle (DIP)
   Class secara langsung bergantung pada GameManager. Menggunakan abstraksi seperti interface dapat mengurangi ketergantungan langsung.

## 5. **ShopScripts.cs** :

```bash
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
using UnityEngine.Purchasing;
using UnityEngine.Purchasing.Extension;

[Serializable]
public class ConsumableItem {
    public string Name;
    public string Id;
    public string desc;
    public float price;
}

public class ShopScript : MonoBehaviour, IDetailedStoreListener
{
    IStoreController isc;

    [SerializeField] SaveLoader sl;
    public ConsumableItem c250, c500, c1500;

    void Start(){
        SetupBuilder();
    }

    private void SetupBuilder(){
        var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());

        builder.AddProduct(c250.Id, ProductType.Consumable);
        builder.AddProduct(c500.Id, ProductType.Consumable);
        builder.AddProduct(c1500.Id, ProductType.Consumable);

        UnityPurchasing.Initialize(this, builder);
    }

    public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
    {
        print("Initialized");
        isc = controller;
    }

    public void buy10button(){
        isc.InitiatePurchase(c250.Id);
    }

    public void buy100button(){
        isc.InitiatePurchase(c500.Id);
    }

    public void buy500button(){
        isc.InitiatePurchase(c1500.Id);
    }

    public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs purchaseEvent)
    {
        var product = purchaseEvent.purchasedProduct;

        print("Purchased: " + product.definition.id);

        if (product.definition.id == c250.Id) {
            _buyCoin250();
        }
        else if (product.definition.id == c500.Id) {
            _buyCoin500();
        }
        else if (product.definition.id == c1500.Id) {
            _buyCoin1500();
        }

        return PurchaseProcessingResult.Complete;
    }

    private void _buyCoin250(){
        int coinOwned = PlayerPrefs.GetInt("coinOwned", 0);
        coinOwned += 250;
        PlayerPrefs.SetInt("coinOwned", coinOwned);
        sl.updateCoinAmount();
    }

    private void _buyCoin500(){
        int coinOwned = PlayerPrefs.GetInt("coinOwned", 0);
        coinOwned += 500;
        PlayerPrefs.SetInt("coinOwned", coinOwned);
        sl.updateCoinAmount();
    }

    private void _buyCoin1500(){
        int coinOwned = PlayerPrefs.GetInt("coinOwned", 0);
        coinOwned += 1500;
        PlayerPrefs.SetInt("coinOwned", coinOwned);
        sl.updateCoinAmount();
    }


    public void OnInitializeFailed(InitializationFailureReason error)
    {
        print("initialization failure: " + error);
    }

    public void OnInitializeFailed(InitializationFailureReason error, string message)
    {
        print("initialization failure: " + error + " |||| " + message);
    }

    public void OnPurchaseFailed(Product product, PurchaseFailureReason failureReason)
    {
        print("purchase failed " + product.definition.id + " |||| " + failureReason);
    }


    public void OnPurchaseFailed(Product product, PurchaseFailureDescription failureDescription)
    {
        print("purchase failed " + product.definition.id + " |||| " + failureDescription);
    }
}
```

### Penjelasan Code :

`1. Start() : Memanggil SetupBuilder() untuk mengatur konfigurasi item yang dapat dibeli`<br>

`2. SetupBuilder() : Membuat instance ConfigurationBuilder untuk Unity Purchasing.Menambahkan item ke konfigurasi menggunakan ID dari item konsumsi (c250, c500, c1500)`<br>

`3. OnInitialized() : Dipanggil ketika sistem pembelian telah berhasil diinisialisasi. Menyimpan referensi ke controller toko (isc).`<br>

`4. buy10button(), buy100button(), buy500button() : Memulai pembelian untuk masing-masing item (250, 500, atau 1500 koin).`<br>

`5. ProcessPurchase() : Memproses pembelian setelah berhasil: Memeriksa ID item yang dibeli, Menambahkan jumlah koin sesuai dengan item yang dibeli, Memanggil metode untuk memperbarui jumlah koin di UI.`<br>

`6. _buyCoin250(), _buyCoin500(), _buyCoin1500() : Menambah jumlah koin yang dimiliki berdasarkan item yang dibeli, Menyimpan perubahan ke PlayerPrefs, Memanggil updateCoinAmount() pada SaveLoader untuk memperbarui UI.`<br>

`7. OnInitializeFailed() : Menangani kegagalan inisialisasi sistem pembelian, Mencetak alasan kegagalan ke konsol.`<br>

`8. OnPurchaseFailed() : Menangani kegagalan pembelian, Mencetak ID item dan alasan kegagalan ke konsol.`

### Rangkuman :

A. Prinsip OOP yang diterapkan:

1. Encapsulation :
   Penggunaan atribut [SerializeField] membatasi akses langsung ke variabel seperti sl, c250, dll., sehingga sesuai dengan prinsip enkapsulasi.
2. Abstraction
   Detail pembelian item (seperti interaksi dengan PlayerPrefs) disembunyikan di dalam class ini.
3. Polymorphism
   Mengimplementasikan interface IDetailedStoreListener menunjukkan penerapan polymorphism.

B. Prinsip SOLID yang diterapkan :

1. Single Responsibility Principle (SRP)
   Class ini bertanggung jawab untuk mengelola pembelian di dalam game.
   Namun, mencampur logika pembelian (seperti PlayerPrefs untuk menyimpan koin) ini melanggar SRP.
2. Open/Closed Principle (OCP)
   Menambahkan item baru ke toko memerlukan modifikasi kode (seperti SetupBuilder()), mungkin denganpendekatan berbasis konfigurasi dinamis untuk menambahkan produk tapi masih lumayan bisa lah.
3. Dependency Inversion Principle (DIP)
   Secara langsung bergantung pada PlayerPrefs dan SaveLoader. Menggunakan abstraksi atau interface dapat membantu mengurangi ketergantungan langsung.

### 3.2 Achievement System

**A. Jenis Achievement**:

1. Implementasi State Management ialah berupa save data Current character selection, Active achievements, Coin balanced, dan Match history tanpa UI.

2. Error Handling yang diimplementasikan ialah ketika pada mode multiplayer saat user jaringan lag di game ini terdapat bug jika salah 1 player menang dan player ke 2 lag cara menangani nya ketika user menyerang kita tambahkan Kalau sudah 1 detik langsung balikan variable ke 0 Jadi nggak terhitung kena hit pada proses penyerangan sehingga antara player yang kalah dan menang itu tetap matching bahwa yang menang mendapatkan koin 1 biji dan yang kalah 0.

**B. Konsep OOP**:

1. Encapsulation: Data seperti koin, karakter, dan hasil pertandingan disimpan dalam SaveLoader dan hanya bisa diakses melalui metode khusus.
2. Inheritance: Kelas SaveLoader bisa diperluas untuk menangani data tambahan.
3. Polymorphism: Metode serangan bisa fleksibel sesuai kondisi pemain.
4. Abstraction: Menyembunyikan detail implementasi sehingga mudah dipahami.

**C. Penerapan SOLID**:

1. **SRP** : SaveLoader hanya menangani penyimpanan data, tidak ada campur aduk.
2. **OCP** : Bisa ditambah fungsinya tanpa ubah yang lama.
3. **LSP** : Kelas turunan bisa menggantikan SaveLoader tanpa masalah.
4. **ISP** : Bisa buat antarmuka terpisah seperti ICoinManager.
5. **DIP** : Dengan antarmuka, penyimpanan bisa diganti tanpa ubah kode utama.

**D. Code Snippet**:

# +> State Management :

## **1. SaveLoader.cs** :

```bash
using TMPro;
using UnityEngine;

public class SaveLoader : MonoBehaviour
{
    [SerializeField] private TextMeshProUGUI coinAmountText;

    public void UpdateCoinAmount()
    {
        int coinOwned = Mathf.Max(PlayerPrefs.GetInt("coinOwned", 0), 0);
        coinAmountText.text = coinOwned.ToString();
    }

    public void AddCoins(int amount)
    {
        int currentCoins = PlayerPrefs.GetInt("coinOwned", 0);
        int newTotal = Mathf.Max(currentCoins + amount, 0);
        PlayerPrefs.SetInt("coinOwned", newTotal);
        UpdateCoinAmount();
    }

    public void SaveCharacterSelection(int characterId)
    {
        PlayerPrefs.SetInt("selectedCharacter", characterId);
    }

    public int LoadCharacterSelection()
    {
        return PlayerPrefs.GetInt("selectedCharacter", 0);
    }

    public void SaveMatchHistory(int matchResult)
    {
        PlayerPrefs.SetInt("lastMatchResult", matchResult);
    }
}
```

### Penjelasan Code :
    
`1. UpdateCoinAmount() : Memperbarui jumlah koin yang ditampilkan di UI berdasarkan data yang disimpan di PlayerPrefs.`
    
`2. AddCoins() : Menambahkan jumlah koin ke total koin yang dimiliki, Menyimpan total koin baru ke PlayerPrefs, Memperbarui tampilan jumlah koin.`
    
`3. SaveCharacterSelection() : Menyimpan ID karakter yang dipilih oleh pemain ke PlayerPrefs.`
    
`4. LoadCharacterSelection() : Memuat ID karakter yang dipilih dari PlayerPrefs, Mengembalikan ID karakter yang disimpan.`
    
`5. SaveMatchHistory() : Menyimpan hasil pertandingan terakhir ke PlayerPrefs.`


# +> Error Handling :

## **2. Error Handling.cs** :

```bash
# untuk ini tidak ada tampilan UI nya hanya sebagai conter error saja
private bool isPlayerLagging = false;
private float lagThreshold = 1.0f; // waktu dalam detik

public void HandleAttack()
{
    if (!isPlayerLagging)
    {
        // Melakukan serangan
        PerformAttack();
    }
    else
    {
        Debug.Log("Player sedang lag, serangan tidak dihitung.");
        ResetAttack();
    }
}

private void PerformAttack()
{
    // Eksekusi serangan jika pemain tidak lag
    if (Time.time  lastAttackTime > lagThreshold)
    {
        lastAttackTime = Time.time;
        // Proses serangan berhasil
        UpdateGameStatus();
    }
}

private void ResetAttack()
{
    // Mengatur ulang variabel yang terkait serangan jika lag
    isPlayerLagging = false;
}
```

### Penjelasan Code : 

`1. HandleAttack() : Memeriksa apakah pemain sedang lag sebelum mengeksekusi serangan, Jika pemain tidak lag, serangan dilakukan, Jika pemain lag, serangan diabaikan dan variabel terkait diatur ulang.`

`2. PerformAttack() : Melakukan serangan jika pemain tidak lag, Memeriksa waktu terakhir serangan dan waktu sekarang, Jika waktu terakhir lebih dari ambang batas lag, serangan dieksekusi, Memperbarui status permainan.`

`3. ResetAttack() : Mengatur ulang variabel terkait serangan jika pemain lag, Mengatur isPlayerLagging ke false.`

## 4. Implementasi Fitur Lain

### 4.1 Fitur Loading Sebelum Memasuki Lobby Game

## **LoadingManager.cs** :

```bash
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class LoadingManager : MonoBehaviour
{
    [SerializeField] Image loadFill;
    [SerializeField] string[] words;
    [SerializeField] TextMeshProUGUI textBox;
    float timer;
    int w1, w2;
    bool changed = false;
    int[] stutterr;
    LinkedList<int> stutterer;


    // Start is called before the first frame update
    void Start()

    {
        timer = 0f;
        w1 = Random.Range(0,9);
        w2 = Random.Range(0,9);
        while (w2 == w1){
            w2 = Random.Range(0,9);
        }
        textBox.text = words[w1];

        // int gapper = 1;
        // stutterer.AddFirst(gapper);
        // while (gapper < 10){

        // }
    }

    // Update is called once per frame
    void Update()
    {
        timer += Time.deltaTime;
        // loadFill.fillAmount = timer / 10f;

        if (timer >= 5 && !changed){
            textBox.text = words[w2];
            changed = true;
        }

        if (timer >= 0 && timer <= 2f){
            loadFill.fillAmount = 0.2f / 10f;
        }
        else if (timer >= 2 && timer <= 4f){
            loadFill.fillAmount = 2f / 10f;
        }
        else if (timer >= 4f && timer <= 7f){
            loadFill.fillAmount = 4f / 10f;
        }
        else if (timer >= 7f && timer <= 8f){
            loadFill.fillAmount = 7f / 10f;
        }
        else if (timer >= 8f && timer <= 9f){
            loadFill.fillAmount = 8f / 10f;
        }
        else if (timer >= 9f){
            loadFill.fillAmount = 9.2f / 10f;
        }

        if (timer >= 10f){
            SceneManager.LoadScene(1);
        }
    }
}
```

### Penjelasan Code :

`1. Start() : Dipanggil saat pertama kali object aktif di game yaitu init variabel timer dengan 0 untuk mencatat waktu berlalu, lalu memilih dua indeks acak w1 dan w2 (dengan memastikan keduanya berbeda) untuk menentukan kata-kata yang ditampilkan dari array words saat loading, Menampilkan kata pertama (words[w1]) di textBox.`

`2. Update() : update bar loading dengan mengubah nilai fillAmount dari loadFill (progress bar) berdasarkan nilai timer.`<br>

`- Pembagian rentang waktu:`<br>
    `0-2 detik: fillAmount 0.02 (progres awal).` <br>
    `2-4 detik: fillAmount 0.2.`<br>
        `4-7 detik: fillAmount 0.4.` <br>
        `7-8 detik: fillAmount 0.7.`<br>
        `8-9 detik: fillAmount 0.8.`<br>
        `9-10 detik: fillAmount 0.92.`<br> <br>
     `- Ganti Teks:` <br>
        `Jika timer >= 5 dan teks belum berubah (changed == false), mengganti teks di textBox dengan kata kedua (words[w2]) dan menandai perubahan (changed = true).`<br>
    `- Load Scene Baru:` <br>
        `Jika timer >= 10, memuat scene berikutnya menggunakan SceneManager.LoadScene(1).`


### Rangkuman :

A. Prinsip OOP yang diterapkan:

1. Encapsulation
    Variabel seperti loadFill, words, dan textBox dienkapsulasi dengan [SerializeField], membatasi akses langsung dari luar class. Namun, variabel seperti timer, w1, w2, dan changed bersifat publik secara implisit.
2. Abstraction
    Detail seperti perubahan teks dan progress bar dikelola di dalam class, sehingga kode lain hanya perlu menggunakan class ini tanpa mengetahui isinya.


B. Prinsip SOLID yang diterapkan :

1. Single Responsibility Principle (SRP)
    Class ini hanya bertanggung jawab untuk mengelola proses loading, seperti memperbarui progress bar dan mengganti teks.
2. Open/Closed Principle (OCP)
    Class ini tidak dirancang untuk diperluas tanpa memodifikasi kode. Contohnya, jika Anda ingin mengubah rentang waktu untuk progress bar, Anda perlu mengedit fungsi Update().
3. Dependency Inversion Principle (DIP)
    Class ini secara langsung menggunakan SceneManager, Image, dan TextMeshProUGUI. Menggunakan abstraksi untuk pengelolaan scene atau UI.


### 4.2 Fitur UI Untuk Regain Mana Di Dalam Match

### UIController.cs :

```bash
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using System;

public class UIController : MonoBehaviour
{
    [SerializeField] GameObject questionPanel;
    [SerializeField] GameObject optionsPanel;
    [SerializeField] GameManager gm;
    [SerializeField] PlayerController player;
    [SerializeField] Button fbButton;
    [SerializeField] Button lgButton;
    [SerializeField] Button shButton;
    [SerializeField] Button hlButton;
    [SerializeField] Button qButton;
    [SerializeField] TextMeshProUGUI qText;

    [SerializeField] Image[] g7Set;
    [SerializeField] Image[] g8Set;
    [SerializeField] Image[] g9Set;
    [SerializeField] string[,] answers = new string[20,4];
    [SerializeField] TextMeshProUGUI[] answerText = new TextMeshProUGUI[4];
    int[] cAnswer = new int[14];
    int qIndex;
    int sIndex;
    float questionCD = 2f;

    void Start(){
        //fbButton.GetComponent<Image>().fillAmount = 0.5f;
    }

    private void Update(){
        questionCD += Time.deltaTime;
        if (questionCD >= 3f){
            qButton.interactable = true;
            qText.text = "ANSWER QUESTION / REGAIN MANA";
        }
        else if (questionCD < 3f){
            qButton.interactable = false;
            qText.text = "COOLDOWN";
        }
    }

    public void changeQset(int set){
        switch (set){
            case 7:
                answers[0,0] = "Gambar A";
                answers[0,1] = "Gambar B";
                answers[0,2] = "Gambar C";
                answers[0,3] = "Gambar A & C";
                cAnswer[0] = 3;

                answers[1,0] = "Lingkaran";
                answers[1,1] = "Persegi";
                answers[1,2] = "Persegi Panjang";
                answers[1,3] = "Segitiga";
                cAnswer[1] = 2;

                answers[2,0] = "46";
                answers[2,1] = "36";
                answers[2,2] = "33";
                answers[2,3] = "32";
                cAnswer[2] = 3;

                answers[3,0] = "50";
                answers[3,1] = "60";
                answers[3,2] = "70";
                answers[3,3] = "32";
                cAnswer[3] = 2;

                answers[4,0] = "5";
                answers[4,1] = "10";
                answers[4,2] = "12";
                answers[4,3] = "9";
                cAnswer[4] = 1;

                answers[5,0] = "7x + 5y";
                answers[5,1] = "7x + 4y";
                answers[5,2] = "5x + 7y";
                answers[5,3] = "4x + 7y";
                cAnswer[5] = 0;

                answers[6,0] = "1000";
                answers[6,1] = "2000";
                answers[6,2] = "3000";
                answers[6,3] = "4000";
                cAnswer[6] = 1;

                answers[7,0] = "8";
                answers[7,1] = "10";
                answers[7,2] = "12";
                answers[7,3] = "14";
                cAnswer[7] = 2;

                answers[8,0] = "10";
                answers[8,1] = "20";
                answers[8,2] = "30";
                answers[8,3] = "40";
                cAnswer[8] = 1;

                answers[9,0] = "30";
                answers[9,1] = "60";
                answers[9,2] = "115";
                answers[9,3] = "224";
                cAnswer[9] = 1;

                sIndex = 7;
                break;

            case 8:
                answers[0,0] = "6";
                answers[0,1] = "7";
                answers[0,2] = "8";
                answers[0,3] = "9";
                cAnswer[0] = 1;

                answers[1,0] = "12";
                answers[1,1] = "14";
                answers[1,2] = "15";
                answers[1,3] = "16";
                cAnswer[1] = 2;

                answers[2,0] = "3000";
                answers[2,1] = "3500";
                answers[2,2] = "4500";
                answers[2,3] = "5000";
                cAnswer[2] = 3;

                answers[3,0] = "4000 + 3000";
                answers[3,1] = "2500 + 2000";
                answers[3,2] = "3000 + 2000";
                answers[3,3] = "3500 + 1500";
                cAnswer[3] = 2;

                answers[4,0] = "10";
                answers[4,1] = "20";
                answers[4,2] = "30";
                answers[4,3] = "40";
                cAnswer[4] = 0;

                answers[5,0] = "134";
                answers[5,1] = "144";
                answers[5,2] = "154";
                answers[5,3] = "164";
                cAnswer[5] = 2;

                answers[6,0] = "40";
                answers[6,1] = "44";
                answers[6,2] = "52";
                answers[6,3] = "56";
                cAnswer[6] = 2;

                answers[7,0] = "120";
                answers[7,1] = "130";
                answers[7,2] = "140";
                answers[7,3] = "150";
                cAnswer[7] = 0;

                answers[8,0] = "9 akar 6";
                answers[8,1] = "8 akar 7";
                answers[8,2] = "23";
                answers[8,3] = "43";
                cAnswer[8] = 1;

                sIndex = 8;
                break;

            case 9:
                answers[0,0] = "Lingkaran";
                answers[0,1] = "Persegi Panjang";
                answers[0,2] = "Segitiga";
                answers[0,3] = "Oval";
                cAnswer[0] = 0;

                answers[1,0] = "32 cm^2";
                answers[1,1] = "96 cm^2";
                answers[1,2] = "48 cm^2";
                answers[1,3] = "50 cm^2";
                cAnswer[1] = 2;

                answers[2,0] = "30 cm";
                answers[2,1] = "34 cm";
                answers[2,2] = "32 cm";
                answers[2,3] = "40 cm";
                cAnswer[2] = 1;

                answers[3,0] = "2";
                answers[3,1] = "3";
                answers[3,2] = "4";
                answers[3,3] = "1";
                cAnswer[3] = 0;

                answers[4,0] = "24 cm^2";
                answers[4,1] = "96 cm^2";
                answers[4,2] = "72 cm^2";
                answers[4,3] = "48 cm^2";
                cAnswer[4] = 1;

                answers[5,0] = "40 cm";
                answers[5,1] = "96 cm";
                answers[5,2] = "56 cm";
                answers[5,3] = "14 cm^2";
                cAnswer[5] = 0;

                answers[6,0] = "Sudut berpelurus";
                answers[6,1] = "Sudut siku-siku";
                answers[6,2] = "Sudut tumpul";
                answers[6,3] = "Sudut lancip";
                cAnswer[6] = 3;

                answers[7,0] = "Setiap kotaknya ani membagi menjadi 8 bagian, sehingga 8 bagian dikali 3 kotak akan menghasilkan 24 potong, sisanya Ani";
                answers[7,1] = "Setiap kotaknya ani membagi menjadi 7 bagian, sehingga 7 bagian dikali 3 kotak akan menghasilkan 21 bagian(potong)";
                answers[7,2] = "Ani hanya membagi kepada teman dekatnya saja yakni 7 orang, sehingga dia membagi nya menjadi 7 bagian saja";
                answers[7,3] = "Ani tidak suka membagikan makannaya kepada temannya, tanpa sepengetahuan ibunya, karena dia suka dengan brem";
                cAnswer[7] = 1;

                answers[8,0] = "126 cm2";
                answers[8,1] = "132 cm2";
                answers[8,2] = "140 cm2";
                answers[8,3] = "156 cm2";
                cAnswer[8] = 0;

                answers[9,0] = "3/2 Liter ";
                answers[9,1] = "2 Liter ";
                answers[9,2] = "6 Liter ";
                answers[9,3] = "27/2 Liter ";
                cAnswer[9] = 2;

                answers[10,0] = "Garis Berpotongan";
                answers[10,1] = "Garis Sejajar";
                answers[10,2] = "Garis Berimpit";
                answers[10,3] = "Garis Tegak Lurus";
                cAnswer[10] = 1;

                answers[11,0] = "Sudut Lancip";
                answers[11,1] = "Sudut Tumpul";
                answers[11,2] = "Sudut Siku-siku";
                answers[11,3] = "Sudut Berpelurus";
                cAnswer[11] = 0;

                answers[12,0] = "25°";
                answers[12,1] = "50°";
                answers[12,2] = "75°";
                answers[12,3] = "30°";
                cAnswer[12] = 3;

                answers[13,0] = "Refleksi";
                answers[13,1] = "Rotasi";
                answers[13,2] = "Dilatasi";
                answers[13,3] = "Traslasi";
                cAnswer[13] = 0;

                sIndex = 9;
                break;

            default:
                break;
        }
        changeQuestion();
    }

    public void changeQuestion(){
        Image[] questionImage = new Image[20];
        if (sIndex == 7){
            questionImage = g7Set;
            qIndex = UnityEngine.Random.Range(0, 10);
        }
        else if (sIndex == 8){
            questionImage = g8Set;
            qIndex = UnityEngine.Random.Range(0, 9);
        }
        else if (sIndex == 9){
            questionImage = g9Set;
            qIndex = UnityEngine.Random.Range(0, 14);
        }

        foreach (var p in questionImage){
            p.gameObject.SetActive(false);
        }
        questionImage[qIndex].gameObject.SetActive(true);

        for (int i = 0; i < 4; i++){
            answerText[i].text = answers[qIndex, i];
        }
        // cAnswer = CA;
    }

    public void answer(int choice){
        if (choice == cAnswer[qIndex]){
            gm.correctAnswer();
        }
        else{
            gm.wrongAnswer();
            questionCD = 0;
        }
        changeQuestion();
        closeQuestionPanel();
    }

    public void openQuestionPanel(){
        questionPanel.SetActive(true);
        optionsPanel.SetActive(false);
    }

    public void closeQuestionPanel(){
        questionPanel.SetActive(false);
        optionsPanel.SetActive(true);
    }

    public void offOptions(){
        optionsPanel.SetActive(false);
    }

    public void setFill(float timer){
        fbButton.GetComponent<Image>().fillAmount = timer/1f;
        lgButton.GetComponent<Image>().fillAmount = timer/2f;
        shButton.GetComponent<Image>().fillAmount = timer/1f;
        hlButton.GetComponent<Image>().fillAmount = timer/0.5f;
    }

    public void enableButton(int buttonIndex){
        switch (buttonIndex){
            case 1:
                fbButton.interactable = true;
                break;
            case 2:
                lgButton.interactable = true;
                break;
            case 3:
                shButton.interactable = true;
                break;
            case 4:
                hlButton.interactable = true;
                break;

            default:
                break;
        }
    }

    public void disableButton(int buttonIndex){
        switch (buttonIndex){
            case 1:
                fbButton.interactable = false;
                break;
            case 2:
                lgButton.interactable = false;
                break;
            case 3:
                shButton.interactable = false;
                break;
            case 4:
                hlButton.interactable = false;
                break;

            default:
                break;
        }
    }
}
```

### Penjelasan Code : 

`1. Start() : Untuk panggil ketika objek pertama kali diinisialisasi di dalam scene dan loader data.`

`2. Update() :  dipanggil setiap frame dalam Unity. Di sini, fungsi ini mengelola cooldown untuk tombol pertanyaan (qButton) yaitu questionCD dihitung setiap frame dengan menambah waktu (Time.deltaTime), jika questionCD lebih dari 3 detik, maka tombol akan dapat diklik dan teksnya akan berubah menjadi "ANSWER QUESTION / REGAIN MANA", jika belum mencapai 3 detik, tombol akan dinonaktifkan dan teks menjadi "COOLDOWN"`

`3. changeQset(int set) : Mengatur pertanyaan dan jawaban untuk set pertanyaan tingkat grade kelas yang dipilih (7, 8, atau 9).`

`4. changeQuestion() : Mengubah pertanyaan yang ditampilkan di panel pertanyaan berdasarkan set pertanyaan saat ini (sIndex) secara random dan indeks pertanyaan saat ini (qIndex), Setelah memilih soal dan jawaban yang tepat, fungsi ini akan mengupdate tampilan jawaban (answerText[])`

`5. answer(int choice) : Mengelola jawaban yang diberikan oleh pemain: Jika jawaban benar, panggil fungsi correctAnswer() dari GameManager, Jika jawaban salah, panggil fungsi wrongAnswer() dari GameManager dan reset questionCD.`

`6. openQuestionPanel() : Menampilkan panel pertanyaan dan menyembunyikan panel opsi.`

`7. closeQuestionPanel() : Menyembunyikan panel pertanyaan dan menampilkan panel opsi.`

`8. offOptions() : Menyembunyikan panel opsi.`

`9. setFill(float timer) : Mengatur fillAmount dari progress bar untuk setiap tombol skill berdasarkan timer.`

`10. enableButton(int buttonIndex) : Mengaktifkan tombol skill berdasarkan indeks tombol.`

`11. disableButton(int buttonIndex) : Menonaktifkan tombol skill berdasarkan indeks tombol.`


### 4.3 Fitur Validasi Serangan Bot Di Mode Single Player

### SPBotController.cs :

```bash
using System.Collections;
using System.Collections.Generic;
using TMPro;
using Unity.VisualScripting;
using UnityEngine;
using UnityEngine.UI;

public class SPBotController : MonoBehaviour
{
    [SerializeField] SPGameManager gm;
    [SerializeField] GameObject fireball, shield;
    [SerializeField] Animator[] chars = new Animator[9];
    [SerializeField] Animator current_char;
    [SerializeField] Image health_bar, mana_bar;
    [SerializeField] TextMeshProUGUI health_counter, mana_counter, health_change_text, mana_change_text;
    [SerializeField] AudioSource src;
    [SerializeField] AudioClip fbSFX, lgSFX, hlSFX, shSFX;
    [SerializeField] Transform spell_origin;

    int d20Roll;
    float time_between_attacks = 99.0f;
    public float hp = 100f;
    float mp = 10f;
    public bool player_fireball_out = false;
    bool dead = false;

    // Start is called before the first frame update
    void Start(){
        hp = 100f;
        mp = 10f;
        set_character();
    }

    // Update is called once per frame
    void Update(){
        time_between_attacks -= Time.deltaTime;
        update_bars();

        if (time_between_attacks <= 0 && !dead && !gm.player_dead){
            roll_for_attack();
        }
    }

    void update_bars() {
        health_bar.fillAmount = hp / 100f;
        mana_bar.fillAmount = mp / 100f;
        health_counter.text = hp.ToString();
        mana_counter.text = mp.ToString();
    }

    void set_character() {
        for (int i = 0; i < 9; i++) {
            chars[i].gameObject.SetActive(false);
        }
        int char_selector = Random.Range(0,9);
        chars[char_selector].gameObject.SetActive(true);
        current_char = chars[char_selector];
    }

    public void audioSetter(int state){
        if (state == 0){
            src.enabled = false;
        }
        else if (state == 1){
            src.enabled = true;
        }
    }

    void add_mana(){
        change_mana(+30);
    }

    void change_mana(int mana_change){
        mp += mana_change;
        if (mp > 100){
            mp = 100;
        }
        else if (mp < 0) {
            mp = 0;
        }
        StartCoroutine(Mana_Text(mana_change));
    }

    IEnumerator Mana_Text(int mana_change) {
        mana_change_text.text = mana_change.ToString() + " MANA";
        mana_change_text.gameObject.SetActive(true);
        yield return new WaitForSeconds(1);
        mana_change_text.gameObject.SetActive(false);
    }

    void heal_spell(){
        change_mana(-40);
        change_health(+30);
        src.clip = hlSFX;
        src.Play();
    }

    public void change_health(int health_change) {
        hp += health_change;
        if (hp > 100) {
            hp = 100;
        }
        if (hp <= 0) {
            hp = 0;
            current_char.GetComponent<SpriteRenderer>().color = Color.black;
            current_char.speed = 0;
            dead = true;
            gm.death_call(2);
        }
        StartCoroutine(Health_Text(health_change));
    }

    IEnumerator Health_Text(int health_change){
        health_change_text.text = health_change.ToString() + " HP";
        health_change_text.gameObject.SetActive(true);
        yield return new WaitForSeconds(1);
        health_change_text.gameObject.SetActive(false);
    }

    public void roll_for_timer(){
        d20Roll = Random.Range(1,21);
        if (d20Roll <= 5){
            time_between_attacks = 5.0f;
        }
        else if (d20Roll <= 10) {
            time_between_attacks = 3.0f;
        }
        else if (d20Roll <= 16) {
            time_between_attacks = 2.0f;
        }
        else if (d20Roll > 16) {
            time_between_attacks = 1.0f;
        }
    }

    public void shield_trigger() {
        time_between_attacks = 99f;
        StartCoroutine(shield_routine());
    }

    IEnumerator shield_routine() {
        GameObject s = Instantiate(shield, transform.position, transform.rotation);
        yield return new WaitForSeconds(5);
        Destroy(s);
        roll_for_timer();
    }

    public void lightning_trigger() {
        change_mana(-60);
        StartCoroutine(lightning_routine_p1(2));
    }

    IEnumerator lightning_routine_p1 (int p) {
        current_char.SetTrigger("Cast");
        yield return new WaitForSeconds(0.75f);
        src.clip = lgSFX;
        src.Play();
        gm.lightning_call(2);
    }

    public void fireball_trigger() {
        change_mana(-30);
        StartCoroutine(fireball_routine());
    }

    IEnumerator fireball_routine() {
        current_char.SetTrigger("Cast");
        src.clip = fbSFX;
        src.Play();
        yield return new WaitForSeconds(0.75f);
        summon_fireball();
    }

    void summon_fireball(){
        GameObject fb = Instantiate(fireball, spell_origin.position, spell_origin.rotation);
        Rigidbody2D rb = fb.GetComponent<Rigidbody2D>();
        rb.rotation = 180f;
        rb.AddForce(spell_origin.right * -10f, ForceMode2D.Impulse);
        change_mana(-20);
    }

    private void OnCollisionEnter2D(Collision2D other) {
        if (other.gameObject.tag == "Fireball") {
            change_health(-30);
        }
    }

    void roll_for_attack() {

        roll_for_timer();

        if (mp < 10) {
            add_mana();
        }

        else if (mp < 20) {
            if (player_fireball_out) {
                shield_trigger();
            }
            else if (!player_fireball_out) {
                add_mana();
            }
        }

        else if (mp < 40) {
            if (player_fireball_out) {
                shield_trigger();
            }
            else if (!player_fireball_out) {
                d20Roll = Random.Range(1, 21);
                if (d20Roll > 10) {
                    fireball_trigger();
                }
                else if (d20Roll <= 10) {
                    add_mana();
                }
            }
        }

        else if (mp < 60) {
            if (player_fireball_out) {
                shield_trigger();
            }
            else if (!player_fireball_out) {
                if (hp <= 60) {
                    heal_spell();
                }
                else if (hp > 60) {
                    d20Roll = Random.Range(1, 21);
                    if (d20Roll > 10) {
                        fireball_trigger();
                    }
                    else if (d20Roll <= 10) {
                        add_mana();
                    }
                }
            }
        }

        else if (mp >= 60) {
            if (player_fireball_out) {
                shield_trigger();
            }
            else if (!player_fireball_out) {
                if (hp <= 60) {
                    heal_spell();
                }
                else if (hp > 60) {
                    d20Roll = Random.Range(1, 21);
                    if (d20Roll > 10) {
                        fireball_trigger();
                    }
                    else if (d20Roll <= 10) {
                        lightning_trigger();
                    }
                }
            }
        }

    }
}
```

### Penjelasan Code :

`1. Start() : Dipanggil sekali saat objek pertama kali diinisialisasi. Di sini, nilai HP (health points) dan MP (mana points) diatur ke nilai default. Juga memanggil set_character() untuk memilih karakter acak yang akan digunakan oleh bot.`

`2. Update() : Dipanggil setiap frame. Di sini, timer antara serangan (time_between_attacks) dikurangi dengan waktu yang berlalu setiap frame (Time.deltaTime), jika waktu antar serangan mencapai 0, bot akan mencoba untuk melakukan serangan dengan memanggil roll_for_attack().`

`3. update_bars() : Mengatur fillAmount dari health_bar dan mana_bar berdasarkan nilai HP dan MP saat ini. Juga menampilkan nilai HP dan MP di health_counter dan mana_counter. `

`4. set_character () : Fungsi ini memilih karakter acak dari array chars (berisi 9 animator) untuk digunakan oleh bot, setelah memilih karakter, animator yang sesuai akan diaktifkan dan yang lainnya dinonaktifkan.`

`5. audioSetter(int state) : mengatur status audio berdasarkan parameter state. Jika state adalah 0, audio dinonaktifkan; jika state adalah 1, audio diaktifkan.`

`6. add_mana() : Untuk menambahkan 30 MP ke bot dengan memanggil change_mana().`

`7. change_mana(int mana_change) : Mengubah nilai MP bot berdasarkan mana_change, memastikan nilai MP tidak melebihi 100 atau kurang dari 0. Juga menampilkan teks perubahan MP selama 1 detik.`

`8. Mana_Text(int mana_change) : Menampilkan teks perubahan MP selama 1 detik.`

`9. heal_spell() : Untuk menggunakan spell heal, mengurangi 40 MP dan menambahkan 30 HP ke bot. Juga memainkan efek suara heal.`

`10. change_health(int health_change) : Mengubah nilai HP bot berdasarkan health_change, memastikan nilai HP tidak melebihi 100 atau kurang dari 0. Juga menampilkan teks perubahan HP selama 1 detik.`

`11. Health_Text(int health_change) : Menampilkan teks perubahan HP selama 1 detik.`

`12. roll_for_timer() : Menggulirkan dadu d20 untuk menentukan waktu antara serangan berikutnya, nilai yang lebih rendah mengatur interval waktu lebih lama, sementara nilai lebih tinggi mengurangi waktu antar serangan.`

`13. shield_trigger() : Menghentikan timer serangan dan memulai shield_routine().`

`14. shield_routine() : Menampilkan efek shield selama 5 detik, kemudian menggulirkan dadu d20 untuk menentukan waktu serangan berikutnya.`

`15. lightning_trigger() : Mengurangi 60 MP dan memulai lightning_routine_p1().`

`16. lightning_routine_p1(int p) : Memainkan animasi casting dan efek suara lightning, kemudian memanggil lightning_call() dari GameManager.`

`17. fireball_trigger() : Mengurangi 30 MP dan memulai fireball_routine().`

`18. fireball_routine() : Memainkan animasi casting dan efek suara fireball, kemudian memanggil summon_fireball().`

`19. summon_fireball() : Membuat instance fireball di spell_origin dan memberikan dorongan ke arah yang berlawanan.`

`20. OnCollisionEnter2D(Collision2D other) : Ketika bot bertabrakan dengan fireball, mengurangi 30 HP.`

`21. roll_for_attack() : Menggulirkan dadu d20 untuk menentukan serangan apa yang akan dilakukan bot berdasarkan kondisi MP dan HP saat ini.`


## 4.4 Fitur Pencarian MatchMaking Di Mode Multiplayer

### SimpleMatchMaking.cs :

```bash
using System;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using Unity.Netcode;
using Unity.Netcode.Transports.UTP;
using Unity.Services.Authentication;
using Unity.Services.Core;
using Unity.Services.Lobbies;
using Unity.Services.Lobbies.Models;
using Unity.Services.Relay;
using Unity.Services.Relay.Models;
using UnityEngine;

#if UNITY_EDITOR
using ParrelSync;
#endif

public class SimpleMatchmaking : MonoBehaviour
{
    [SerializeField] private GameObject _buttons;
    [SerializeField] private GameObject _panel;
    [SerializeField] private GameObject _waitingText;

    private Lobby _connectedLobby;
    private QueryResponse _lobbies;
    private UnityTransport _transport;
    private string JoinCodeKey = "j";
    private string _playerId;

    private void Awake() => _transport = FindObjectOfType<UnityTransport>();

    public void gradeSelect(int grade){
        JoinCodeKey = grade.ToString();
        CreateOrJoinLobby();
    }

    public async void CreateOrJoinLobby()
    {
        await Authenticate();

        _connectedLobby = await QuickJoinLobby() ?? await CreateLobby();

        if (_connectedLobby != null)
        {
            _buttons.SetActive(false);
            //_waitingText.SetActive(true);
        }
    }

    private async Task Authenticate()
    {
        var options = new InitializationOptions();

#if UNITY_EDITOR
        // Remove this if you don't have ParrelSync installed.
        // It's used to differentiate the clients, otherwise lobby will count them as the same
        options.SetProfile(ClonesManager.IsClone() ? ClonesManager.GetArgument() : "Primary");
#endif

        await UnityServices.InitializeAsync(options);

        await AuthenticationService.Instance.SignInAnonymouslyAsync();
        _playerId = AuthenticationService.Instance.PlayerId;
    }

    private async Task<Lobby> QuickJoinLobby()
    {
        try
        {
            // Attempt to join a lobby in progress
            var lobby = await Lobbies.Instance.QuickJoinLobbyAsync();

            // If we found one, grab the relay allocation details
            var a = await RelayService.Instance.JoinAllocationAsync(lobby.Data[JoinCodeKey].Value);

            // Set the details to the transform
            SetTransformAsClient(a);

            // Join the game room as a client
            NetworkManager.Singleton.StartClient();
            return lobby;
        }
        catch (Exception e)
        {
            Debug.Log($"No lobbies available via quick join");
            return null;
        }
    }

    private async Task<Lobby> CreateLobby()
    {
        try
        {
            const int maxPlayers = 2;

            // Create a relay allocation and generate a join code to share with the lobby
            var a = await RelayService.Instance.CreateAllocationAsync(maxPlayers);
            var joinCode = await RelayService.Instance.GetJoinCodeAsync(a.AllocationId);

            // Create a lobby, adding the relay join code to the lobby data
            var options = new CreateLobbyOptions
            {
                Data = new Dictionary<string, DataObject> { { JoinCodeKey, new DataObject(DataObject.VisibilityOptions.Public, joinCode) } }
            };
            var lobby = await Lobbies.Instance.CreateLobbyAsync("Useless Lobby Name", maxPlayers, options);

            // Send a heartbeat every 15 seconds to keep the room alive
            StartCoroutine(HeartbeatLobbyCoroutine(lobby.Id, 15));

            // Set the game room to use the relay allocation
            _transport.SetHostRelayData(a.RelayServer.IpV4, (ushort)a.RelayServer.Port, a.AllocationIdBytes, a.Key, a.ConnectionData);

            // Start the room. I'm doing this immediately, but maybe you want to wait for the lobby to fill up
            NetworkManager.Singleton.StartHost();
            return lobby;
        }
        catch (Exception e)
        {
            Debug.LogFormat("Failed creating a lobby");
            return null;
        }
    }

    private void SetTransformAsClient(JoinAllocation a)
    {
        _transport.SetClientRelayData(a.RelayServer.IpV4, (ushort)a.RelayServer.Port, a.AllocationIdBytes, a.Key, a.ConnectionData, a.HostConnectionData);
    }

    private static IEnumerator HeartbeatLobbyCoroutine(string lobbyId, float waitTimeSeconds)
    {
        var delay = new WaitForSecondsRealtime(waitTimeSeconds);
        while (true)
        {
            Lobbies.Instance.SendHeartbeatPingAsync(lobbyId);
            yield return delay;
        }
    }

    private void OnDestroy()
    {
        try
        {
            StopAllCoroutines();
            // todo: Add a check to see if you're host
            if (_connectedLobby != null)
            {
                if (_connectedLobby.HostId == _playerId) Lobbies.Instance.DeleteLobbyAsync(_connectedLobby.Id);
                else Lobbies.Instance.RemovePlayerAsync(_connectedLobby.Id, _playerId);
            }
        }
        catch (Exception e)
        {
            Debug.Log($"Error shutting down lobby: {e}");
        }
    }
}
```

### Penjelasan Code :

`1. Awake() : Dipanggil saat objek pertama kali diinisialisasi. Di sini, mencari UnityTransport di scene.`

`2. gradeSelect(int grade) : Mengatur JoinCodeKey berdasarkan grade yang dipilih dan memanggil CreateOrJoinLobby().`

`3. CreateOrJoinLobby() : Memanggil Authenticate(), kemudian mencoba untuk melakukan QuickJoinLobby(), jika gagal, maka CreateLobby() akan dipanggil.`

`4. Authenticate() : Menginisialisasi UnityServices, melakukan sign-in anonim, dan mendapatkan PlayerId.`

`5. QuickJoinLobby() : Mencoba untuk bergabung dengan lobby yang sedang berlangsung, jika berhasil, mendapatkan detail relay allocation dan memanggil SetTransformAsClient().`

`6. CreateLobby() : Membuat relay allocation, mendapatkan join code, membuat lobby, mengatur relay allocation untuk game room, dan memulai game room sebagai host.`

`7. SetTransformAsClient(JoinAllocation a) : Mengatur relay data untuk client berdasarkan detail relay allocation.`

`8. HeartbeatLobbyCoroutine(string lobbyId, float wait TimeSeconds) : Mengirimkan heartbeat ping ke lobby secara berkala.`

`9. OnDestroy() : Dipanggil saat objek dihancurkan, mencoba untuk menghapus lobby atau mengeluarkan pemain dari lobby.`


## 5. Screenshot dan Demo

### A. Inisialisasi Project Di Unity Menggunakan Tools Untuk Game Dua Dimensi

![images](/assets/images/7.jpg)

### B. Pengujian Awal PvP (Player versus Player) dan PvE (Player versus environment) Di Dalam GameManager

![images](/assets/images/1.jpg)

### C. Farming Assets Dari Itch.io

![images](/assets/images/3.jpg)

### D. Pemilihan Tombol Serangan Dan Bertahan Ketika Di Dalam Match Game

![images](/assets/images/4.jpg)

### E. Pengujian Serangan Dari Player 1 Ke Player 2 Dengan Defend Dan Bertahan Hingga Salah Satu Player Menang

![images](/assets/images/2.jpg)

### F. Pengujian Tombol Quick Match Untuk Membuat Player1 Sebagai Host Server Dan Player2 Sebagai Client

![images](/assets/images/8.jpg)

### G. Memasukan Assets Semua 9 karakter Ke Dalam Game

![images](/assets/images/10.jpg)

### H. Pembuatan Jurus Serangan Bola Api Menggunakan Vector 2D Dan Rigidbody 2D

![images](/assets/images/5.jpg)

### I. Pembuatan UI Lobby hingga Tombol Shop Untuk Membeli Koin Dengan Uang Asli Melalui Google Play

#### NB : kalau di setting versi uji coba tidak perlu memasukan uang asli sudah bisa Membeli Koin dari Google Playnya atau jika mendownload dari GDrive bisa langsung membeli koinnya

![images](/assets/images/11.jpg)

### J. Testing Mode Single Player Untuk Memastikan Bot/komputer Berjalan Dengan Baik

![images](/assets/images/12.jpg)

### K. Pembuatan Soal Matematika Sebagai Regain Mana

![images](/assets/images/13.jpg)

### L. Penataan Layer Dua Dimensi Untuk Memastikan Z-Index Dari Setiap Objek Sudah Benar

![images](/assets/images/6.jpg)

### M. Pengujian Multiplayer Dari Player 1 Dan Player 2 Menggunakan Github Pararel Sync Sebagai Server

![images](/assets/images/14.jpg)

### N. Testing LoadSystem Jika Player Berhasil Memenangkan Pertandingan Dan Mendapatkan Koin 1

![alt text](/assets/images/15.jpg)

### O. Visualisasi Integrasi Tombol

![alt text](/assets/images/16.jpg)

### P. Pembuatan Scene Loading

![alt text](/assets/images/18.jpg)

### Q. Build Game Dengan Gradle Android

![alt text](/assets/images/17.jpg)

### R. Configurasi API Level 34 Agar Support Android 12 Ke Atas

![alt text](/assets/images/19.jpg)

### S. Setting Activate App In Purchase

![alt text](/assets/images/21.jpg)

### T. App In Purchase Untuk Membeli Koin Dengan Uang Asli

![alt text](/assets/images/20.jpg)

### U. Testing Pembelian Dengan Uji Coba Disini Tertera "zanualvin06@gmail.com" Sebagai Tester

![alt text](/assets/images/22.jpg)

https://github.com/user-attachments/assets/d4e60c96-9d25-4177-8a91-d2df66b331b5

### V. Preview Ke-9 Karakter WeapMath

| ![alt text](/assets/src/char1.gif) | ![alt text](/assets/src/char2.gif) | ![alt text](/assets/src/char3.gif) |
|------------------------------------|------------------------------------|------------------------------------|
| ![alt text](/assets/src/char4.gif) | ![alt text](/assets/src/char5.gif) | ![alt text](/assets/src/char6.gif) |
| ![alt text](/assets/src/char7.gif) | ![alt text](/assets/src/char8.gif) | ![alt text](/assets/src/char9.gif) |



<br>


- **Link Demo Video**:

### [Demo Youtube : Klik Disini !](https://www.youtube.com/@alvinzanuaputra/videos/)

- [Part 1](https://youtu.be/OFidW1Qtl8A)
  
- [Part 2](https://youtu.be/AQ2bvv_KlFA)

## 6. Panduan Instalasi dan Menjalankan Game

### A. **Opsi 1** : "Instalasi dan Menjalankan Game di Localhost (PC/Laptop) dengan Unity Editor"

1. **Persyaratan**:
   - Unity Hub
   - Unity Editor
   - Text Editor (Visual Studio Code, Visual Studio, Sublime Text, dan lain-lain)
   - Git
2. **Langkah**:

   - Clone repository ini

        ```bash
        https://github.com/alvinzanuaputra/finalpro-pbo-Csharp.git
        ```

   - Buka Unity Hub
   - Install Unity Editor versi "2022.3.30f1"
   - Klik "Add" dan pilih folder hasil cloning repository
   - Klik project yang sudah di-add
   - Klik "Open" untuk membuka project di Unity Editor
   - Untuk testing open scene "Main Menu" di folder "Scenes"
   - Kemudian untuk testing multiplayernya

     1. Tekan Tombol Window
     2. Tekan Package Manager
     3. Tekan Tombol Plus +
     4. Pilih Add package from git URL
     5. Masukkan URL berikut :

        ```bash
        https://github.com/VeriorPies/ParrelSync.git?path=/ParrelSync
        ```

     6. Kemudian Klik Add dan Tunggu hingga selesai lalu klik update.
     7. Close Tab lalu tekan tombol Pararel Sync di atas, tekan package manager, lalu klik open in new editor.

### B. **Opsi 2** : "Dapat dijalankan di Android"

### 1. Download APK di link berikut :

- Versi Ini Bisa Membeli Koin Dengan Uang Asli :
[Download .AAB Di Play Store](https://play.google.com/store/apps/details?id=com.weapmath.weapmath)


### 2. Download APK di link berikut :

- Versi Ini Bisa Membeli Koin Akan Terjadi Error : 
[Download .APK Di GDrive](https://drive.google.com/uc?export=download&id=1XRM31Xyy8xYd6-0C0g-d7BTRf93XSsr4) 

#### NB :

**Kenapa ada 2 versi?** 
"Versi pertama" dapat membeli koin dengan uang asli karena dihubungkan dengan API Unity melalui Google Play di .gradle-nya, sedangkan "Versi kedua" tidak terkait karena dari link eksternal atau direktori sendiri jadi .gradle tidak terhubung dengan API Unity yang nantinya akan diteruskan ke Google Play Store.

## 7. Kendala dan Solusi

1. **Kendala 1**:

   - Kesulitan menangani multiplayernya karena harus menghubungkan ke server dan mengatur waktu untuk menghindari lag.

2. **Kendala 2**:
   - Solusi: Membuat sistem yang dapat mengatur waktu dan menangani error handling saat terjadi lag dengan jeda 1 detik, tapi mungkin tidak sesempurna itu karena tergantung koneksi internet juga.

## 8. Kesimpulan dan Pembelajaran

- **Kesimpulan**:

  Dapat disimpulkan bahwa game ini merupakan game yang dapat dimainkan secara online dan offline dengan fitur multiplayernya yang dapat diakses melalui server lokal atau server eksternal. Game ini juga memiliki fitur pembelian koin dengan uang asli melalui Google Play Store dan fitur regen mana dengan menjawab soal matematika. Game ini juga memiliki fitur error handling yang baik untuk mengatasi lag saat bermain walaupun tidak sesempurna itu.

- **Pembelajaran**:

  Pembelajaran yang didapat dari pembuatan game ini adalah bagaimana cara membuat game yang dapat dimainkan secara online dan offline, bagaimana cara menghubungkan game ke server lokal atau server eksternal, bagaimana cara membuat fitur pembelian koin dengan uang asli melalui Google Play Store, dan bagaimana cara membuat fitur regen mana dengan menjawab soal matematika dengan sesuatu yang sangat kompleks, yang paling mengesankan adalah bagaimana berintergrasi dengan API Unity dan Google Play Store.

## 9. Referensi Game

#### A. [Klik Disini : Marbel Gim Belajar Budaya](https://play.google.com/store/apps/details?id=com.educastudio.marbelbudayanusantara&hl=id)

#### B. [Klik Disini : Cerdas Cermat SMP](https://play.google.com/store/apps/details?id=com.solitekids.cerdascermatsmp&hl=id)

## 10. Source Code

**Untuk Source Code Project Game Weapmath Ini Dapat Dilihat Di Link Github Berikut Ini :**

#### [Klik Disini : alvinzanuaputragithub-finalpro-pbo-c#](https://github.com/alvinzanuaputra/finalpro-pbo-Csharp)

<br>
<br>
<br>

<div align="center">
<h3>Sekian Terima Banyak Atas Ilmu Yang Diberikan Kepada <br>
   Pak Bintang, Pak Nafkhan, dan Mas Ovaldo</h3>
</div>
