# Using HTML blocks in internationalisation (i18n) locale files

In projects which make good use of locale files to provide the textual content of HTML pages, the need often arises for keys to include HTML elements as well as plain text. However, by default i18n string values are marked as unsafe for HTML usage, so any `<tag>` elements get escaped and display on screen.

To get round this, it is tempting to use Rails' `.html_safe` method to manually clear a string for HTML usage and to avoid it being escaped:

```haml
# views/page.html.haml
.tagline
  = t('header.tagline', subject: subject).html_safe
```

```yml
# locales/en.yml
header:
  tagline: "The best website for <strong>%{subject}</strong>"
```

However, this has the potential to allow `subject` to include rogue HTML (or worse, Javascript XSS vulnerabilities) which could then be rendered in the browser.

An alternative is to name the key with an `_html` suffix. This allows us to use HTML elements within the text of our YAML locale file, but any parameters that are supplied are escaped. So if any of the data we are passing in has been supplied by users, they should still be safe to display.

```haml
# views/page2.html
.tagline
  = t('header.tagline_html', subject: subject)
```

```yml
# locales/en.yml
header:
  tagline_html: "The best website for <strong>%{subject}</strong>"
```

So we retain the flexibility of being able to include HTML in our locale files without risking browser safety. As a bonus, the `_html` suffix makes explicit which locale keys are allowed to include HTML, and means that the inclusion of `.html_safe` within our view layer should be regarded as a code smell wherever we see it.

Source: [Rails guide on i18n](http://guides.rubyonrails.org/i18n.html#using-safe-html-translations)
