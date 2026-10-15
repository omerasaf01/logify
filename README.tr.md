# Logify

[English](README.md) · **Türkçe**

Seq'den ilham alan, geliştirici odaklı yapısal log görüntüleme ve proje yönetimi demosu. Web arayüzünde projeye özel log akışlarını inceleyebilir, olay özelliklerini görüntüleyebilir ve kullanıcı rollerini deneyebilirsiniz.

![Logify demosu — Checkout API log akışı ve olay detayları](docs/images/logify-demo.png)

*Örnek loglar ve açılmış bir olay detayıyla Checkout API. Mevcut arayüz Türkçedir; dokümantasyon İngilizce ve Türkçe olarak sunulur.*

## Projenin durumu

Web arayüzü, hazır örnek veriler ve simüle log akışlarıyla tamamen istemci tarafında çalışan bağımsız bir demodur. Backend'e istek göndermez. Projeler, kullanıcılar, yetkiler ve API anahtarları bellekte tutulur ve sayfa yenilendiğinde sıfırlanır; yalnızca seçili oturum kullanıcısı `sessionStorage` içinde saklanır.

`web-api` dizininde başlangıç aşamasındaki FastAPI uygulaması, yapılandırma ve kullanıcı modeli bulunur. Log alımı, kalıcı veri saklama ve üretim ortamına uygun kimlik doğrulama henüz uygulanmamıştır. Demo rolleri ve anahtarları, hedeflenen iş akışlarını gösterir.

## Özellikler

- Her proje için ayrı log akışı, ortam ve saklama süresi ayarları.
- Yapısal mesaj şablonları, seviye/zaman filtreleri, metin ve özellik araması.
- Açılabilir olay detayları, hata izleri, ham JSON ve kopyalama işlemleri.
- Simüle canlı akış ve tek tıkla beş test logu üretme.
- Proje oluşturma, ayarlar, kullanıcı yönetimi ve `maintainer` / `viewer` yetkileri.
- API anahtarı yönetimi demosu: `ingest` / `read` kapsamları, son kullanma tarihi, iptal ve tek seferlik gizli anahtar gösterimi.
- Yönetici ve üye görünümlerini denemek için hızlı profil değiştirme.

## Web demosunu çalıştırma

Gereksinimler: Node.js 18+ (Vite 5 ile uyumlu) ve npm.

```bash
cd web-ui
npm ci
npm run dev
```

Vite'ın terminalde gösterdiği yerel adresi açın (genellikle `http://localhost:5173`). API veya veritabanı gerekmez.

**`omer@logify.dev`** e-posta adresi ve **`logify`** parolasıyla giriş yapın ya da **Hızlı Giriş** bölümünden aktif bir profil seçin. Log akışını incelemek için **Checkout API** projesini açın. Özellikleri görmek için bir log satırını seçin. Yeni olaylar üretmek için **Canlı Akışı Başlat** veya **+5 Test Logu** düğmelerini kullanın.

### Derleme ve kontrol

Bu komutları `web-ui` dizininde çalıştırın:

```bash
npm run typecheck  # TypeScript kontrolü
npm run build      # TypeScript kontrolü + dist/ dizinine üretim derlemesi
npm run preview    # Üretim derlemesini yerel olarak sunma
```

## Arama örnekleri

| Sorgu | Anlamı |
| --- | --- |
| `timeout` | Mesaj metninde ve hata izlerinde arama yapar. |
| `StatusCode=500` | Özellik değerini eşleştirir (`StatusCode:500` de kullanılabilir). |
| `StatusCode!=200` | Özellik değerini dışlar. |
| `Elapsed>250` | Sayısal karşılaştırma yapar. |
| `Elapsed<=100` | Sayısal üst sınır uygular. |

Sorguya filtre eklemek için olayın özellik tablosundaki **= Filtrele** veya **≠ Dışla** düğmelerini kullanın.

## Dizin yapısı

```text
logify/
├── README.md           # İngilizce dokümantasyon
├── README.tr.md        # Türkçe dokümantasyon
├── docs/images/        # Demo ekran görüntüsü
├── web-ui/             # React + TypeScript + Vite demosu
│   └── src/
│       ├── components/ # Atomlar, moleküller, organizmalar, şablonlar
│       ├── pages/      # Giriş, projeler, olaylar, anahtarlar, erişim, ayarlar, kullanıcılar
│       ├── data/       # Örnek veriler ve log üreticisi
│       ├── store/      # Bellekte tutulan durum ve işlemler
│       └── lib/        # Sorgu ayrıştırma, yetkiler, biçimlendirme, anahtarlar
└── web-api/            # Python / FastAPI backend iskeleti
```

## Backend geliştirme

Backend **Python 3.14+** gerektirir; FastAPI, SQLModel, Pydantic ve Uvicorn kullanır. uv kuruluysa ayrı olarak çalıştırabilirsiniz:

```bash
cd web-api
uv sync
uv run uvicorn web_api:app --host 127.0.0.1 --port 8000 --reload
```

FastAPI dokümantasyonu `http://127.0.0.1:8000/docs` adresindedir. Uygulamada henüz özel API uç noktaları yoktur ve API, arayüze bağlı değildir.
