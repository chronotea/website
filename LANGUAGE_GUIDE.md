# Multi-Language Implementation Guide

## Overview

Your website now supports multiple languages using a URL parameter-based system. Users can switch between languages using buttons or by visiting URLs like:
- `https://chronothe.fr/` (defaults to French)
- `https://chronothe.fr/?lang=fr` (French)
- `https://chronothe.fr/?lang=en` (English)

## Current Languages

- **French (fr)** - Default language
- **English (en)** - English translation

## How It Works

1. **URL Parameter Detection**: The system reads the `?lang=` parameter from the URL
2. **Default Language**: If no parameter is provided, French is used by default
3. **Dynamic Content Replacement**: JavaScript replaces text content based on the selected language
4. **Shareable Links**: Users can share language-specific links

## Adding a New Language

To add a new language (e.g., Spanish, German, Italian), follow these steps:

### Step 1: Add Language Button

In `index.html`, add a new button in the language switcher (around line 55):

```html
<div class="language-switcher">
    <button class="button is-small" onclick="switchLanguage('fr')">FR</button>
    <button class="button is-small" onclick="switchLanguage('en')">EN</button>
    <button class="button is-small" onclick="switchLanguage('es')">ES</button> <!-- New Spanish button -->
</div>
```

### Step 2: Add Translations Object

In the `<script>` section at the bottom of `index.html` (around line 293), add a new language object in the `translations` constant:

```javascript
const translations = {
    fr: {
        subtitle: "Elephant ChronoThé — l'assistant de thé parfait",
        section1_title: "A qui ça sert?",
        // ... existing French translations
    },
    en: {
        subtitle: "Elephant ChronoThé — the perfect tea assistant",
        section1_title: "Who is it for?",
        // ... existing English translations
    },
    es: {  // New Spanish translations
        subtitle: "Elephant ChronoThé — el asistente de té perfecto",
        section1_title: "¿Para quién es?",
        section1_text: "Nunca más olvide su bolsita/infusor de té en la taza — confíe en el Elephant.\n\nLo retira automáticamente después del tiempo preestablecido.",
        section2_title: "Fácil",
        section2_text1: "Adjunte su infusor o bolsita de té al Elephant.",
        section2_text2: "Configure el tiempo de infusión y toque para comenzar.",
        section2_text3: "Al final de la infusión, el Elephant se levanta y le permite recuperar su té cuando quiera. Su té, incluso tibio, será perfecto.",
        section3_title: "Diseño compacto",
        section3_text: "Su base es del tamaño de una caja de té.",
        section4_title: "No necesita enchufe eléctrico",
        section4_text: "¡Funciona con una batería que dura al menos una semana!",
        section5_title: "Ecológico",
        section5_text: "¡Diseñado y fabricado en Francia, 100% artesanal!\nHecho de madera sostenible francesa, cola de madera francesa, acero francés y aceite de linaza 100% natural francés.",
        newsletter_title: "Manténgase informado",
        newsletter_email: "Correo electrónico",
        newsletter_name: "Su nombre",
        newsletter_button: "¡Inscríbeme!"
    }
};
```

### Step 3: Update Language Detection (Optional)

If you want to support automatic language detection from browser settings, modify the `getLanguage()` function (around line 335):

```javascript
function getLanguage() {
    const urlParams = new URLSearchParams(window.location.search);
    const lang = urlParams.get('lang');

    // List of supported languages
    const supportedLangs = ['fr', 'en', 'es'];

    if (supportedLangs.includes(lang)) {
        return lang;
    }

    // Optional: Auto-detect from browser
    const browserLang = navigator.language.slice(0, 2);
    if (supportedLangs.includes(browserLang)) {
        return browserLang;
    }

    return 'fr'; // Default to French
}
```

## Translation Keys Reference

Here are all the translation keys you need to provide for each language:

| Key | Description | Example (English) |
|-----|-------------|-------------------|
| `subtitle` | Main subtitle | "Elephant ChronoThé — the perfect tea assistant" |
| `section1_title` | Section 1 title | "Who is it for?" |
| `section1_text` | Section 1 description | "Never forget your tea bag..." |
| `section2_title` | Section 2 title | "Easy" |
| `section2_text1` | Section 2 step 1 | "Attach your infuser..." |
| `section2_text2` | Section 2 step 2 | "Set the infusion time..." |
| `section2_text3` | Section 2 step 3 | "At the end of the infusion..." |
| `section3_title` | Section 3 title | "Compact design" |
| `section3_text` | Section 3 description | "Its base is the size..." |
| `section4_title` | Section 4 title | "No electrical outlet needed" |
| `section4_text` | Section 4 description | "It runs on a battery..." |
| `section5_title` | Section 5 title | "Eco-friendly" |
| `section5_text` | Section 5 description | "Designed and made in France..." |
| `newsletter_title` | Newsletter heading | "Stay informed" |
| `newsletter_email` | Email input placeholder | "Email" |
| `newsletter_name` | Name input placeholder | "Your name" |
| `newsletter_button` | Subscribe button text | "Sign me up!" |

## SEO Considerations

The current implementation updates meta tags dynamically for better SEO:
- `<meta name="description">`
- `<meta property="og:description">`
- `<meta name="twitter:description">`

For even better SEO, consider:
1. Using separate HTML pages per language (`/en/`, `/fr/`, etc.)
2. Adding `<link rel="alternate" hreflang="en" href="...">` tags
3. Creating a sitemap with language variations

## Future Enhancements

### Option 1: Separate JSON Files
For cleaner code organization, move translations to separate files:

```
/lang/
  ├── en.json
  ├── fr.json
  └── es.json
```

Then load them dynamically with `fetch()`.

### Option 2: Language Detection
Add automatic language detection based on browser settings or geolocation.

### Option 3: Language Persistence
Store the user's language preference in `localStorage` to remember their choice across sessions.

```javascript
function switchLanguage(lang) {
    localStorage.setItem('preferredLanguage', lang);
    const url = new URL(window.location);
    url.searchParams.set('lang', lang);
    window.location.href = url.toString();
}

function getLanguage() {
    const urlParams = new URLSearchParams(window.location.search);
    const lang = urlParams.get('lang') || localStorage.getItem('preferredLanguage');
    return (lang === 'en' || lang === 'fr') ? lang : 'fr';
}
```

## Testing

After adding a new language, test:
1. ✅ Language switcher buttons work
2. ✅ Direct URL with `?lang=XX` parameter works
3. ✅ All text elements are translated
4. ✅ Form placeholders and button text are translated
5. ✅ Meta tags are updated for sharing on social media
6. ✅ Active language button is highlighted

## Questions?

If you need help adding a new language or want to implement any of the future enhancements, feel free to ask!
