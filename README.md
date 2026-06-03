# Digital Content Platform Architecture (UML Modeling)

This repository contains a comprehensive system analysis and design project developed as a **Final Term Project for the Computer Programming** course. 

The project models the core software architecture, user behaviors, and modular components of a digital streaming platform (similar to Netflix) using standard **Unified Modeling Language (UML)** diagrams.


##  Object-Oriented Programming (OOP) Core Concepts Applied

The system architecture translates real-world requirements into software structures using key OOP principles showcased in the diagrams:

* **Abstraction:** The `İçerik` (Content) class is designed as an *abstract class*, serving as a base template for concrete implementations.
* **Inheritance:** `Yönetici` (Admin) inherits from `Kullanıcı` (User), extending system privileges while eliminating code duplication.
* **Composition (Strong Ownership):** The relationship between `Kullanıcı` (User) and `Profil` (Profile) represents a strict lifecycle dependency; a Profile cannot exist without a User.
* **Aggregation (Loose Ownership):** The link between `İzleme Listesi` (Watchlist) and `İçerik` (Content) ensures that deleting a personal list does not destroy the content entries in the system database.

##  System Diagrams & Technical Breakdowns

> *Note: The diagram visuals are labeled in Turkish as per course requirements. So, I explain all diagram with english terms.*

### 1. Class Diagram 
![Class Diagram](image_881285.jpg)
**Objective:** This diagram displays the static structure of the system and contains 11 main classes. Each class has its own specific attributes and methods to manage the platform's data.

* **`Kullanıcı` (User) Class:** This class represents the standard user. It includes attributes such as `#userID(kullaniciID)`, `+nameSurname(isimSoyisim)`, `+email(email)`, `-password(sifre)`, and `+telephoneNo(telefon)`. It contains methods like `+login(girisYap)`, `+forgotPassword(sifremiUnuttum)`, `+createProfile(profilOlustur)`, `+logOut(cikisYap)`, `+subscribe(aboneOl)`, and `+signUp(kayitOl)`.
* **`Yönetici` (Admin) Class:** This class inherits directly from the `Kullanıcı` class. It includes a specific attribute `+roleLevel(yetkiSeviyesi)` and contains methods for administrative tasks such as `+addContent(icerikEkle)`, `+deleteContent(icerikSil)`, `+definePromoCode(indirimKoduTanımla)`, and `+accessManagementSystem(yonetimSistemineEris)`.
* **`Yönetim Sistemi` (Management System) Class:** This class manages the automated backend operations. It contains attributes like `+systemStatus(sistemDurumu)` and `+totalRevenue(toplamGelir)`. Its methods include `+generateReport(raporOlustur)`, `+backup(yedekle)`, `+maintenance(bakimYap)`, `+deleteUser(kullaniciSil)`, `+autoBilling(otomatikOdemeCek)`, `+verifyPayment(odemeDogrula)`, `+verifyUser(kullaniciDogrula)`, `+publishContent(icerigiYayinaAl)`, and `+unpublishContent(icerigiYayindanKaldir)`.
* **`Abonelik` (Subscription) Class:** Manages user plans with attributes like `+planType(planTuru)`, `+price(fiyat)`, `+startDate(baslangicTarihi)`, and `+endDate(bitisTarihi)`. It uses methods like `+collectPayment(odemeAl)`, `+cancel(iptalEt)`, `+changePlan(planDegistir)`, and `+selectPlan(planSec)`.
* **`Ödeme` (Payment) Class:** Connected to the subscription class. It holds details like `-cardNumber(kartNo)`, `+amount(tutar)`, `+paymentDate(odemeTarihi)`, `+approvalStatus(onayDurumu)`, and `+promoCode(indirimKodu)`. It processes transactions with methods like `+makePayment(odemeYap)`, `+refund(iadeAl)`, `+downloadInvoice(faturaIndir)`, `+applyDiscount(indirimUygula)`, `+enterCardInfo(kartBilgisiGir)`, and `+verify3DSecure(3DSecureDogrula)`.
* **`Profil` (Profile) Class:** Connected to the User class with a 1-to-many relationship. It contains `+profileName(profilAdi)`, `+ageLimit(yasSiniri)`, and `+languagePreference(dilTercihi)`. Its methods allow actions like `+watch(izle)`, `+getWatchlist(listeyiGetir)`, `+addComment(yorumYap)`, `+deleteComment(yorumSil)`, `+editProfile(profilDuzenle)`, and `+downloadContent(icerikIndir)`.
* **`İçerik` (Content) Abstract Class:** This is an abstract class indicated by `<<abstract>>`. It holds core attributes like `#title(isim)`, `#genre(turi)`, `#releaseYear(yayinYili)`, and `#averageRating(ortalamaPuani)`. Its structural methods are `+play(oynat)`, `+stop(durdur)`, `+watchTrailer(fragmanIzle)`, `+download(indir)`, `+showComments(yorumlarıGoster)`, and `+selectSubtitle(altyaziSec)`.
* **`Film` (Movie) & `Dizi` (TV Series) Classes:** Both concrete classes inherit directly from `İçerik`. `Film` introduces `+duration(sure)` and `+director(yonetmen)`. `Dizi` introduces `+seasonCount(sezonSayisi)` and `+episodeCount(bolumSayisi)`.
* **`İzleme Kaydı` (Watch Log) Class:** Tracks a profile's progress on a content with attributes `+watchDate(izlemeTarihi)`, `+remainingMinutes(kalanDakika)`, and `+isFinished(bitirdiMi)`.
* **`İzleme Listesi` (Watchlist) Class:** Contains the attribute `+listName(listeAdi)` and methods `+add(ekle)` and `+remove(cikar)` to manage content items.
* **`Yorum` (Comment/Review) Class:** Stores user reviews with `+commentID(yorumID)`, `+commentText(yorumMetni)`, `+date(tarih)`, and `+score(puan)`. It includes methods like `+edit(duzenle)` and `+publish(yayinla)`.



### 2. Use Case Diagram 
![Use Case Diagram](image_880f7e.jpg)
**Objective:** This diagram illustrates the functional requirements of the digital platform by defining the boundaries between 3 main actors and various use cases.

* **`Kullanıcı` (User) Actor:** The primary consumer. This actor interacts with primary use cases such as `Search Content(İçerik Ara)` (which specializes into `Search Movie(Film Ara)` and `Search TV Series(Dizi Ara)`), `Watch Movie/TV Series(Film/Dizi İzle)`, `Download Content(İçerik İndir)`, `Log Out(Sistemden Çıkış)`, `Forgot Password(Şifremi Unuttum)`, `Create Profile(Profil Oluştur)`, and `Register(Kayıt Ol)`. The `Watch Movie/TV Series(Film/Dizi İzle)` use case contains an `<<include>>` relationship to `Login(Sisteme Giriş)`, and extends (`<<extend>>`) to features like `Subtitle Selection(Altyazı Seçimi)`, `Add to Watchlist(Listeye Ekle)`, `Add Comment(Yorum Yap)`, and `Watch Trailer(Fragman İzle)`.
* **`Yönetici` (Admin) Actor:** Inherits the core permissions of the User actor but interacts with exclusive admin use cases: `Delete Content(İçerik Sil)`, `Add Content(İçerik Ekle)`, `Define Promo Code(İndirim Kodu Tanımla)`, and `Access Management System(Yönetim Sistemine Eriş)`.
* **`Yönetim Sistemi` (Management System) Actor:** An automated backend actor that handles autonomous background processes. It triggers use cases like `Verify Payment(Ödeme Doğrula)`, `System Report(sistem raporu)`, `Auto Billing(Otomatik Ödeme Çek)`, `Manage Users(Kullanıcıları Yönet)`, `Unpublish Content(İçeriği Yayından Kaldır)`, `Publish Content(İçeriği Yayına Al)`, `Verify Account/Subscription(Hesap/Abonelik Doğrula)`, and `Database Backup / Maintenance(Veritabanı Yedekle / Bakım)`.


### 3. Object Diagram 
![Object Diagram](image_880f5c.png)
**Objective:** This diagram represents a real-world concrete runtime instance of the system classes, modeling a specific user interaction scenario at a single point in time.

* **`kullanici_1: Kullanıcı` Instance:** A concrete user named "Ayşe Kaya" with specific data: ID `11111`, email `aysekaya@gmail.com`, and phone `555 555 5555`.
* **`abonelik_1: Abonelik` & `odeme_1: Ödeme` Instances:** Shows that her subscription type is `Premium` with a price of `199.90`. The payment object shows the amount `199.90`, payment date `01.06.2024`, and an approved status (`Onaylandı`).
* **`profil_1: Profil` Instance:** Ayşe's active profile is named "Ana Profil", language preference is set to "Türkçe", and the age limit is `25`.
* **`inception_filmi: Film` Instance:** The profile is actively linked to a concrete Movie object named "Inception", directed by Christopher Nolan, with a duration of `120` minutes, genre "Bilim Kurgu", and an average score of `8.8`.
* **`izleme_kaydı_1`, `favoriler`, and `yorum_1` Instances:** Displays runtime data where she has watched `102` minutes of the film, added it to a watchlist named "Favorilerim", and published a 10-point comment saying "Harika Bir Film!".
* **Administrative Instances:** Includes background instances like `yonetici_1` acting as a content manager (`içerik yöneticisi`) and `yonetim sistemi_1` running on system version `7.12.095`.

---

### 4. Component Diagram 
![Component Diagram](image_880f62.png)
**Objective:** This diagram illustrates the structural organization and software dependencies of the platform's components, divided into 4 main modular blocks.

* **`Sistem Arayüzleri (Front-End)` Component:** The presentation layer containing two main structural pieces: `User Application(Kullanıcı Uygulaması)` and `Admin Dashboard(Yönetici Paneli)`.
* **`Yönetim Çekirdeği (Back-End)` Component:** The core subsystem containing the `Yönetim_Sistemi` component, managing internal validations, authorizations (`System Control Access(Sistem Kontrol Erişimi)`), and sessions (`Session / Identity Verification(Oturum / Kimlik Doğrulama)`).
* **`İçerik ve Oynatma İşlemleri` Component:** Dedicated to media processing. It contains the `Media Player(İçerik Oynatıcı)` and `Search Engine(İçerik Arama Motoru)`, managing the streaming pipeline (`Content Stream(İçerik Akışı)`).
* **`Finans ve Abonelik Modülü` Component:** Governs business monetization, containing the `Payment System(Ödeme Sistemi)` and `Subscription Manager(Abonelik Yöneticisi)` linked to external billing interfaces.



## Repository Structure

* `Diagram-Images/`  Contains high-resolution visual exports (`.jpg`, `.png`).
* `Source-Files/`  Original editable vector source files for architectural diagrams.

*Note: This project was developed as part of a university group assignment; however, the entire architecture and all diagrams were individually designed and independently conceptualized by me.*
