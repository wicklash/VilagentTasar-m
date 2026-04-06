# Vilagent Desktop UI Prototype

Bu proje, Electron + React (CDN) tabanlı bir AI ajan arayüzü prototipidir.

## Gereksinimler

- Node.js 18+ (önerilir)
- npm

## Kurulum

```bash
npm install
```

## Geliştirme Modunda Çalıştırma

```bash
npm run dev
```

Alternatif:

```bash
npm run start
```

## Windows `.exe` Build Alma

```bash
npm run build:win
```

Build çıktıları `dist/` klasöründe oluşur.

## Proje Yapısı

- `main.js` → Electron ana süreç dosyası
- `ui/index.html` → Arayüz (React + Tailwind CDN)
- `package.json` → scriptler ve bağımlılıklar

## Notlar

- `node_modules/` GitHub'a yüklenmez, kullanıcı `npm install` ile indirir.
- Arayüz simülasyon odaklıdır; gerçek API çağrıları içermez.
