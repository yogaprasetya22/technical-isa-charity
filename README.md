# ISA Charity Clone

Proyek ini adalah pembuatan ulang website [ISA Charity](https://isa-charity.webflow.io/) menggunakan Vue 3, dengan fokus pada bagian "Tentang Kami". Implementasi mencakup elemen interaktif dan animasi halus menggunakan GSAP.

## Gambaran Proyek

- **Package Manager**: Bun 1.1.36
- **Framework**: Vue 3 (Options API)
- **Build Tool**: Vite
- **Library Animasi**: GSAP
- **Styling**: SASS
- **Desain Responsif**: Flexbox, Grid, dan media queries

## Struktur Proyek

```
src/
├── assets/
│   ├── images/
│   ├── fonts/
│   └── styles/
├── components/
│   ├── HeaderSection.vue
│   ├── Hero.vue
│   ├── ProjectsSection.vue
│   ├── HelpSection.vue
│   ├── Team.vue
│   ├── Supporters.vue
│   ├── Partners.vue
│   └── FooterSection.vue
├── pages/
│   └── About-us.vue
```

## Petunjuk Instalasi

### Prasyarat

- Bun 1.1.36 atau versi lebih baru
- Node.js 18+
- Git

### Instalasi

Clone repository:
```bash
git clone https://github.com/yogaprasetya22/technical-isa-charity.git

cd technical-isa-charity
```

Install dependensi menggunakan Bun:
```bash
bun install
```

### Menjalankan Proyek

Mulai server pengembangan:
```bash
bun run dev
```

Build untuk produksi:
```bash
bun run build
```

## Implementasi Animasi

Proyek menggunakan GSAP untuk berbagai animasi:
- Munculnya elemen saat di-scroll
- Animasi bertahap untuk tim dan pendukung
- Efek hover interaktif
- Transisi halaman yang halus

## Konfigurasi Kustom

Lihat [Referensi Konfigurasi Vite](https://vitejs.dev/config/) untuk opsi kustomisasi build.

## Deployment

- Vercel

## Catatan Tambahan

---

**Pengembang**: Yoga Prasetya  
**Repositori**: [github.com/yogaprasetya22/technical-isa-charity](https://github.com/yogaprasetya22/technical-isa-charity)  
**Demo Live**: [Live](https://isa-charity-clone.vercel.app/)

---
