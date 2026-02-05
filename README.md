# סדנת אשת חיל - מרכז נשמה

Landing page for "Eshet Chayil" workshop by Merkaz Neshama.

## Live Site
https://eshet-chayil.merkazneshama.co.il

## Features
- Full Hebrew RTL support
- Responsive design (mobile-first)
- Form submission to Directus CRM
- Email notifications
- WhatsApp integration
- Smooth scroll animations
- FAQ accordion

## Tech Stack
- Pure HTML5 + CSS3 + Vanilla JavaScript
- Google Fonts (Heebo + Frank Ruhl Libre)
- Hosted on Caddy with auto SSL
- Backend: Directus CRM

## Structure
```
├── index.html          # Main landing page
├── images/
│   ├── logo.png        # Merkaz Neshama logo
│   ├── hero-heart.png  # Hero section image
│   ├── rabbi.jpg       # Rabbi Shmuel Givritz
│   ├── tree-of-life-bg.png
│   ├── women-temple.png
│   └── ...
└── README.md
```

## Form Integration
Form submissions are sent to:
- **Primary:** Directus CRM (`workshop_registrations` collection)
- **Fallback:** WhatsApp (if Directus fails)

## Contact
- Email: hoshenyehuda@gmail.com
- Phone: 054-320-0050
