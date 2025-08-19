# Online Bankacılık Rest Api (Spring Boot)

Bu proje, Spring Boot framework'ü kullanarak modern bir online bankacılık sistemi geliştirmeyi amaçlamaktadır. Proje, RESTful API'ler aracılığıyla müşteri yönetimi, hesap işlemleri ve bankacılık hizmetlerini gerçekleştirir ve JSON formatında veri alışverişi yapar.

## Teknolojiler

- **Java 17+**
- **Spring Boot 3.x**
- **Spring Web (REST API)**
- **Spring Data JPA**
- **H2 Database (In-Memory)**
- **Maven**
- **Spring Boot DevTools**
- **Spring Validation**

## Proje Yapısı

```
src/main/java/com/bankacilik/
├── OnlineBankacılikSistemiApplication.java
├── controller/
│   ├── MusteriController.java
│   ├── HesapController.java
│   └── IslemController.java
├── entity/
│   ├── Musteri.java
│   ├── Hesap.java (Abstract Entity)
│   ├── VadesizHesap.java
│   └── VadeliHesap.java
├── repository/
│   ├── MusteriRepository.java
│   ├── HesapRepository.java
│   ├── VadesizHesapRepository.java
│   └── VadeliHesapRepository.java
├── service/
│   ├── MusteriService.java
│   ├── HesapService.java
│   ├── IslemService.java
│   └── impl/
│       ├── MusteriServiceImpl.java
│       ├── HesapServiceImpl.java
│       └── IslemServiceImpl.java
├── dto/
│   ├── MusteriRequestDto.java
│   ├── MusteriResponseDto.java
│   ├── HesapRequestDto.java
│   ├── HesapResponseDto.java
│   ├── IslemRequestDto.java
│   └── IslemResponseDto.java
├── enums/
│   ├── HesapTuru.java
│   └── IslemTuru.java
└── exception/
    ├── MusteriBulunamadiException.java
    ├── HesapBulunamadiException.java
    ├── YetersizBakiyeException.java
    ├── GecersizMiktarException.java
    └── GlobalExceptionHandler.java
```

## Özellikler

### REST API Endpoints

#### Müşteri İşlemleri
- **POST /api/musteriler** - Yeni müşteri ekleme
- **GET /api/musteriler** - Tüm müşterileri listeleme
- **GET /api/musteriler/{id}** - ID'ye göre müşteri getirme
- **PUT /api/musteriler/{id}** - Müşteri bilgilerini güncelleme
- **DELETE /api/musteriler/{id}** - Müşteri silme

#### Hesap İşlemleri
- **POST /api/hesaplar** - Yeni hesap açma (vadesiz/vadeli)
- **GET /api/hesaplar** - Tüm hesapları listeleme
- **GET /api/hesaplar/{hesapNo}** - Hesap numarasına göre hesap getirme
- **GET /api/hesaplar/musteri/{musteriId}** - Müşteriye ait tüm hesapları listeleme
- **DELETE /api/hesaplar/{hesapNo}** - Hesap kapatma

#### Bankacılık İşlemleri
- **POST /api/islemler/para-yatir** - Para yatırma
- **POST /api/islemler/para-cek** - Para çekme
- **GET /api/islemler/bakiye/{hesapNo}** - Bakiye görüntüleme
- **POST /api/islemler/faiz-isle/{hesapNo}** - Vadeli hesaba faiz işleme
- **GET /api/islemler/hesap-ozeti/{hesapNo}** - Hesap özeti

## Kullanılan Spring Boot Konuları

- **Spring Boot Starter Web** (REST API geliştirme)
- **Spring Data JPA** (Veritabanı işlemleri)
- **Inheritance Strategies** (@Inheritance, @DiscriminatorColumn)
- **Entity Relationships** (@OneToMany, @ManyToOne)
- **Service katmanı ve Dependency Injection**
- **DTO Pattern** (Data Transfer Object)
- **Custom Exception Handling** (@ControllerAdvice)
- **Bean Validation** (@Valid, @NotNull, @NotBlank, @Min)
- **Enum kullanımı**
- **Transaction Management** (@Transactional)

## Kurulum ve Çalıştırma

### Gereksinimler
- Java 17 veya üzeri
- Maven 3.6+

### Adımlar
1. Projeyi klonlayın veya indirin
2. Terminal/Command Prompt'ta proje klasörüne gidin
3. Aşağıdaki komutu çalıştırın:
```bash
mvn spring-boot:run
```
4. Uygulama http://localhost:8080 adresinde çalışmaya başlayacaktır
5. H2 veritabanı konsolu: http://localhost:8080/h2-console

## API Kullanım Örnekleri

### 1. Müşteri Ekleme (POST)
**URL:** `POST http://localhost:8080/api/musteriler`

**Request Body (JSON):**
```json
{
  "adSoyad": "Ali Veli",
  "tcKimlikNo": "12345678901",
  "telefon": "05551234567",
  "email": "ali.veli@email.com"
}
```

**Response:**
```json
{
  "musteriId": 1,
  "adSoyad": "Ali Veli",
  "tcKimlikNo": "12345678901",
  "telefon": "05551234567",
  "email": "ali.veli@email.com",
  "hesapSayisi": 0,
  "kayitTarihi": "2024-01-15T10:30:00"
}
```

### 2. Hesap Açma (POST)
**URL:** `POST http://localhost:8080/api/hesaplar`

**Vadesiz Hesap Request:**
```json
{
  "musteriId": 1,
  "hesapTuru": "VADESIZ",
  "ilkBakiye": 1000
}
```

**Vadeli Hesap Request:**
```json
{
  "musteriId": 1,
  "hesapTuru": "VADELI",
  "ilkBakiye": 5000,
  "faizOrani": 0.05,
  "vadeAy": 12
}
```

**Response:**
```json
{
  "hesapNo": "1001",
  "musteriId": 1,
  "hesapTuru": "VADESIZ",
  "bakiye": 1000,
  "acilisTarihi": "2024-01-15T10:30:00",
  "aktif": true
}
```

### 3. Para Yatırma (POST)
**URL:** `POST http://localhost:8080/api/islemler/para-yatir`

**Request Body (JSON):**
```json
{
  "hesapNo": "1001",
  "miktar": 500,
  "aciklama": "Maaş yatırma"
}
```

**Response:**
```json
{
  "islemId": "TXN-001",
  "hesapNo": "1001",
  "islemTuru": "YATIRMA",
  "miktar": 500,
  "oncekiBakiye": 1000,
  "yeniBakiye": 1500,
  "islemTarihi": "2024-01-15T10:30:00",
  "aciklama": "Maaş yatırma",
  "basarili": true
}
```

### 4. Para Çekme (POST)
**URL:** `POST http://localhost:8080/api/islemler/para-cek`

**Request Body (JSON):**
```json
{
  "hesapNo": "1001",
  "miktar": 200,
  "aciklama": "ATM'den çekim"
}
```

**Response:**
```json
{
  "islemId": "TXN-002",
  "hesapNo": "1001",
  "islemTuru": "CEKME",
  "miktar": 200,
  "oncekiBakiye": 1500,
  "yeniBakiye": 1300,
  "islemTarihi": "2024-01-15T10:35:00",
  "aciklama": "ATM'den çekim",
  "basarili": true
}
```

### 5. Bakiye Görüntüleme (GET)
**URL:** `GET http://localhost:8080/api/islemler/bakiye/1001`

**Response:**
```json
{
  "hesapNo": "1001",
  "hesapTuru": "VADESIZ",
  "guncelBakiye": 1300,
  "musteriAdSoyad": "Ali Veli",
  "sonIslemTarihi": "2024-01-15T10:35:00"
}
```

### 6. Müşteri Hesaplarını Listeleme (GET)
**URL:** `GET http://localhost:8080/api/hesaplar/musteri/1`

**Response:**
```json
[
  {
    "hesapNo": "1001",
    "hesapTuru": "VADESIZ",
    "bakiye": 1300,
    "acilisTarihi": "2024-01-15T10:30:00",
    "aktif": true
  },
  {
    "hesapNo": "1002",
    "hesapTuru": "VADELI",
    "bakiye": 5000,
    "faizOrani": 0.05,
    "vadeAy": 12,
    "vadeTarihi": "2025-01-15",
    "acilisTarihi": "2024-01-15T10:40:00",
    "aktif": true
  }
]
```

### 7. Vadeli Hesaba Faiz İşleme (POST)
**URL:** `POST http://localhost:8080/api/islemler/faiz-isle/1002`

**Response:**
```json
{
  "islemId": "TXN-003",
  "hesapNo": "1002",
  "islemTuru": "FAIZ_ISLEME",
  "miktar": 250,
  "oncekiBakiye": 5000,
  "yeniBakiye": 5250,
  "faizOrani": 0.05,
  "islemTarihi": "2024-01-15T10:45:00",
  "basarili": true
}
```

## Hata Yönetimi

### Örnek Hata Yanıtları

**Yetersiz Bakiye:**
```json
{
  "hata": "Yetersiz Bakiye",
  "mesaj": "Hesap bakiyesi yetersiz. Mevcut bakiye: 100.0 TL, İstenilen miktar: 500.0 TL",
  "hatakodu": "INSUFFICIENT_BALANCE",
  "zaman": "2024-01-15T10:30:00"
}
```

**Hesap Bulunamadı:**
```json
{
  "hata": "Hesap Bulunamadı",
  "mesaj": "Hesap numarası: 9999 bulunamadı",
  "hatakodu": "ACCOUNT_NOT_FOUND",
  "zaman": "2024-01-15T10:30:00"
}
```

**Geçersiz Miktar:**
```json
{
  "hata": "Geçersiz Miktar",
  "mesaj": "İşlem miktarı 0'dan büyük olmalıdır",
  "hatakodu": "INVALID_AMOUNT",
  "zaman": "2024-01-15T10:30:00"
}
```

## Entity Tasarımı

### Hesap Inheritance Hierarchy
```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "hesap_turu")
public abstract class Hesap {
    @Id
    private String hesapNo;
    
    @ManyToOne
    @JoinColumn(name = "musteri_id")
    private Musteri musteri;
    
    private BigDecimal bakiye;
    private LocalDateTime acilisTarihi;
    private boolean aktif;
    
    // Abstract methods
    public abstract void paraYatir(BigDecimal miktar);
    public abstract void paraCek(BigDecimal miktar);
}

@Entity
@DiscriminatorValue("VADESIZ")
public class VadesizHesap extends Hesap {
    // Vadesiz hesap specific implementation
}

@Entity
@DiscriminatorValue("VADELI")
public class VadeliHesap extends Hesap {
    private BigDecimal faizOrani;
    private Integer vadeAy;
    private LocalDate vadeTarihi;
    
    public void faizIsle() {
        // Faiz hesaplama logic
    }
}
```

## Enum Kullanımı

```java
public enum HesapTuru {
    VADESIZ("Vadesiz Hesap"),
    VADELI("Vadeli Hesap");
    
    private final String aciklama;
    
    HesapTuru(String aciklama) {
        this.aciklama = aciklama;
    }
}

public enum IslemTuru {
    YATIRMA("Para Yatırma"),
    CEKME("Para Çekme"),
    FAIZ_ISLEME("Faiz İşleme"),
    TRANSFER("Transfer");
    
    private final String aciklama;
    
    IslemTuru(String aciklama) {
        this.aciklama = aciklama;
    }
}
```

## Veritabanı

### H2 Console Erişimi
- **URL:** http://localhost:8080/h2-console
- **JDBC URL:** jdbc:h2:mem:bankacılikdb
- **Username:** sa
- **Password:** (boş)

### Örnek Veri (data.sql)
```sql
-- Müşteri verileri
INSERT INTO musteri (musteri_id, ad_soyad, tc_kimlik_no, telefon, email, kayit_tarihi) VALUES
(1, 'Ali Veli', '12345678901', '05551234567', 'ali.veli@email.com', CURRENT_TIMESTAMP),
(2, 'Zeynep Kara', '12345678902', '05557654321', 'zeynep.kara@email.com', CURRENT_TIMESTAMP);

-- Hesap verileri
INSERT INTO hesap (hesap_no, musteri_id, hesap_turu, bakiye, acilis_tarihi, aktif) VALUES
('1001', 1, 'VADESIZ', 1500.00, CURRENT_TIMESTAMP, true),
('1002', 1, 'VADELI', 5000.00, CURRENT_TIMESTAMP, true);

-- Vadeli hesap ek bilgileri
UPDATE hesap SET faiz_orani = 0.05, vade_ay = 12, vade_tarihi = '2025-01-15' WHERE hesap_no = '1002';
```

## Geliştirme Önerileri

1. **Security:** JWT ile kimlik doğrulama ekleyin
2. **Audit:** İşlem geçmişi takibi
3. **Notification:** Email/SMS bildirimleri
4. **Rate Limiting:** API rate limiting
5. **Caching:** Redis ile cache katmanı
6. **Monitoring:** Actuator ile health check
7. **Testing:** Unit ve integration testler

## Bonus Özellikler (İleri Seviye)

- **Transfer İşlemleri:** Hesaplar arası para transferi
- **Kredi Kartı Entegrasyonu:** Sanal kart oluşturma
- **QR Kod Ödemeleri:** QR kod ile ödeme
- **Döviz İşlemleri:** Multi-currency desteği
- **Yatırım Hesapları:** Hisse senedi, altın hesapları
- **Otomatik Ödeme Talimatları:** Standing orders
- **Mobile Banking API:** Mobile app desteği

## Proje Değerlendirme Kriterleri

1. **OOP Prensipleri:** Inheritance, Polymorphism, Abstraction uygulanmış mı?
2. **Entity Design:** JPA inheritance strategies doğru kullanılmış mı?
3. **Exception Handling:** Custom exception'lar ve global handler uygulanmış mı?
4. **Business Logic:** Bankacılık kuralları doğru implement edilmiş mi?
5. **API Design:** RESTful principles'a uygun mu?
6. **Data Validation:** Input validation'lar uygulanmış mı?
7. **Transaction Management:** ACID properties sağlanıyor mu?

## Öğrenme Hedefleri

Bu projeyi tamamlayarak öğrenciler şunları öğrenecek:
- Spring Boot ile complex domain modeling
- JPA Inheritance strategies (Single Table, Joined, Table per Class)
- Advanced exception handling techniques
- Transaction management
- Business rule implementation
- Financial domain modeling
- Security considerations in banking systems
- API design for complex business scenarios
