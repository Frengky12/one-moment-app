# Prompt Stitch AI — Desain UI Aplikasi Kamera Acara

Berdasarkan [PRD.md](./PRD.md) bagian 6 (Requirement Desain). Gunakan **Prompt Fondasi** dulu di percakapan Stitch pertama agar gaya visualnya konsisten, baru lanjutkan dengan prompt per layar di bawahnya (idealnya dalam sesi/project yang sama supaya Stitch mempertahankan konteks gaya).

Semua prompt ditulis dalam Bahasa Inggris karena hasil generate AI design tools umumnya lebih konsisten dengan input Inggris — tapi kamu bisa translate ke Indonesia isi teksnya nanti pas kasih instruksi konten aktual (label tombol, dll) ke tim/developer.

---

## 0. Prompt Fondasi (jalankan pertama, sekali di awal project)

```
Design a mobile-first web app called "One Moment" — a disposable-camera experience for weddings and events, used by vendors (wedding organizers and photographers) who resell it to their clients under their own branding.

Visual identity: warm, nostalgic, analog-film aesthetic. Think of a real disposable camera roll — soft film grain texture, slightly muted warm color palette (cream, warm white, soft amber accents), rounded corners on cards, subtle vignette on photo previews. Typography should feel editorial and friendly, not corporate — a mix of a warm serif for headings and a clean sans-serif for body text. Avoid generic SaaS "tech blue" styling entirely.

Two distinct visual contexts within the same app:
1. Vendor dashboard (WO/photographer): clean, functional, efficient — still warm-toned but more structured and data-dense, optimized for quick task completion on desktop and mobile.
2. Guest-facing pages: playful, immersive, camera-app feel — full-bleed imagery, minimal chrome, big touch targets, feels like using a real camera app, not a form.

Guest-facing pages must visually foreground the vendor's own logo and brand color (injected dynamically) — our own product branding should be subtle/small, since vendors resell this as their own tool.

Design for mobile viewport first (375px), then adapt to tablet/desktop for the vendor dashboard only (guest pages stay mobile-first always, most guests use it on their phone at the venue).
```

---

## 1. Landing / Marketing Page (untuk approach vendor)

```
Design a marketing landing page for "One Moment", targeting wedding organizers and photographers as B2B customers (not couples directly).

Hero section: headline emphasizing "give your clients a disposable-camera experience they can rebrand as their own," with a subheadline mentioning no app download needed for guests. Include a mockup of the guest camera screen with a soft film-roll frame styling, similar to a photo strip.

Sections needed: how it works (3 steps: vendor creates event → guest scans QR → photos reveal together), value proposition for vendors specifically (white-label branding, recurring subscription instead of per-event fees, live slideshow feature for receptions), pricing tiers for vendors (monthly subscription cards), testimonials from vendors, and a final CTA to "Start free trial."

Style: warm analog-film aesthetic per foundation prompt, but with more marketing polish — larger typography, generous whitespace, photo-strip style image galleries showing sample guest photos.
```

---

## 2. Vendor — Login / Register

```
Design a login and registration screen for vendors (wedding organizers/photographers) accessing their dashboard.

Simple centered card layout on a warm cream background with subtle film-grain texture. Fields: business name (register only), email, password, plus a "magic link" alternative login option. Include a toggle between Login and Register modes on the same screen.

Keep this screen calm and minimal — this is a professional tool entry point, less playful than guest-facing screens, but still warm-toned (not generic blue SaaS auth screen).
```

---

## 3. Vendor — Dashboard (List Event)

```
Design the main vendor dashboard showing a list of all events the vendor manages.

Layout: top bar with vendor's own logo (uploaded by them) and business name, a prominent "Create New Event" button. Below, a grid/list of event cards — each card shows event title, date, guest count, photo count, status badge (active/ended), and a small thumbnail collage of recent guest photos. Include a search/filter bar for vendors managing many events (10-20+).

Empty state: friendly illustration + "Create your first event" prompt for new vendors.

Style: clean and functional per foundation prompt's dashboard guidance, optimized for scanning many events quickly. Works on both desktop and mobile.
```

---

## 4. Vendor — Create / Edit Event Form

```
Design a multi-step (or single-scroll) form for a vendor to create a new event.

Fields needed: event title, auto-generated URL slug (editable), film preset selector (show 6 visual swatches: FunSaver, QuickSnap, Portra 400, Ektar 100, HP5 Plus, CineStill 800T — each swatch should preview the color grading style on a sample photo), photo limit per guest (stepper input), reveal mode selector (three options as cards: "Live," "After event ends," "Scheduled time" with a date/time picker appearing conditionally), and a branding section where the vendor uploads their logo and picks a brand color that will appear on guest-facing pages.

Include a live preview panel (or preview button) showing how the guest-facing landing page will look with the chosen preset and branding applied.

This form should feel quick to complete — target under 1 minute, so keep steps minimal and defaults sensible.
```

---

## 5. Vendor — Event Detail

```
Design an event detail page for a vendor viewing a single event's results.

Show: event title and status, key stats (total guests joined, total photos, reveal status), a large QR code with a "Download QR" button, a photo gallery grid of all uploaded photos (with a toggle for revealed/hidden if reveal hasn't happened yet), and a prominent "Download all photos (.zip)" button. Include a manual "Reveal now" action button if reveal_mode allows manual trigger.

Style: same warm dashboard aesthetic, data presented clearly with the photo grid as the visual centerpiece.
```

---

## 6. Guest — Landing Page (Join Event)

```
Design the guest-facing landing page a guest sees immediately after scanning the event QR code.

This page must foreground the VENDOR's branding (their logo and brand color prominently), with the event title displayed like a film roll label (e.g., "satualbum 400" style branding metaphor but using the vendor's own identity instead). Below, a single, large, friendly input field: "What's your name?" and a big primary button "Join the roll" / "Masuk ke album."

Show a subtle counter or indicator like "24 photos so far" and "6 hours left" to create light urgency/excitement, styled like a film canister label.

This is the very first impression for a guest with zero friction expected — no other fields, no login, just name and one tap. Mobile-only viewport (375px).
```

---

## 7. Guest — Camera Screen

```
Design the camera capture screen guests use to take photos, styled to feel like using a real disposable camera app.

Full-bleed camera viewfinder (live camera preview), with the selected film preset's color grading subtly previewed as an overlay tint so guests can see the film aesthetic before shooting. Minimal UI chrome: a shutter button styled like a physical camera button (bottom center, large touch target), a small counter showing "X photos left" (based on their per-guest limit), and the vendor's small logo watermark in a corner.

No gallery/preview of taken photos should be visible to the guest at this stage (since photos stay hidden until reveal) — after tapping the shutter, show a brief satisfying "captured" animation (like a mechanical shutter/flash effect) then return to the viewfinder, reinforcing that the photo is now hidden until reveal.

Mobile viewport only, portrait orientation, camera permission prompt state also needed.
```

---

## 8. Guest — Gallery (Revealed Photos)

```
Design the gallery view guests see once photos are revealed, styled like an unrolled film strip / contact sheet.

Layout: masonry or grid of photos with each photo showing the guest's name who took it (small caption), filterable by "All guests" or a specific guest name via horizontal chip/tab selector. Tapping a photo opens a fullscreen lightbox view with a download button.

Include a header showing the event title (with vendor branding) and total photo/guest count. Style should feel like flipping through a physical printed photo album — warm, tactile, nostalgic per foundation prompt.
```

---

## 9. Live Slideshow (Venue Display)

```
Design a fullscreen, TV/projector-optimized slideshow screen meant to be displayed live at the event venue during the reception.

New guest photos should appear to "slide in" or "develop" onto the screen one at a time (or in a dynamic collage layout) as they're taken in real time, with the guest's name appearing briefly as a caption. Include the event title and vendor logo subtly placed in a corner, not distracting from the photos themselves.

This screen has no interactive controls visible — it's meant purely for passive viewing on a large display, so design for a 16:9 landscape TV/projector aspect ratio, large legible typography for captions, and a calm ambient background (avoid harsh white background; use the warm film-toned background from the foundation style) so it looks good projected in a dim reception venue.
```

---

## 10. Vendor — Billing / Subscription

```
Design a billing and subscription management page for vendors.

Show: current plan card (plan name, price, renewal date, status badge), a "Change plan" section with tiered subscription options as comparison cards (e.g., Starter / Growth / Studio, differentiated by number of events included per month), payment method section showing Midtrans-supported options (QRIS, GoPay, bank transfer, card icons), and a billing history table (date, amount, status, invoice download link).

Style: clean, trustworthy, professional — slightly more restrained than other dashboard screens since this involves payment, but keep the warm color palette rather than switching to a cold/clinical fintech look.
```

---

## Cara pakai

1. Buka project baru di Stitch, paste **Prompt Fondasi (bagian 0)** duluan sebagai prompt pertama.
2. Lanjutkan generate layar satu per satu (bagian 1-10) di project/thread yang sama, supaya Stitch mempertahankan gaya visual yang konsisten antar layar.
3. Kalau hasil generate meleset dari yang diharapkan, tambahkan referensi konkret di prompt (misal: "make it feel more like [nama app referensi]" atau upload screenshot satualbum sebagai reference image kalau Stitch mendukung image input).
4. Setelah semua layar digenerate, cocokkan lagi ke PRD bagian 6 (daftar 10 layar) untuk pastikan tidak ada yang terlewat.
