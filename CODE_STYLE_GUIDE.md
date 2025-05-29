# Code Style Guide

This guide outlines the coding conventions and best practices for this Shopify Skeleton theme.

## Liquid Conventions

### Syntax and Formatting

**Multi-line Liquid Logic**
```liquid
{% liquid
  assign current_variant = product.selected_or_first_available_variant
  assign product_form_id = 'product-form-' | append: section.id
%}
```

**Inline Filters**
```liquid
{{ shop.name | link_to: routes.root_url }}
{{ 'general.shop' | t }}
```

**Conditionals**
```liquid
{% if section.settings.show_header %}
  {% unless shop.customer_accounts_optional %}
    <!-- content -->
  {% endunless %}
{% endif %}
```

### Variable Naming
- Use lowercase with underscores: `current_variant`, `product_form_id`
- Be descriptive: `collection_product_count` not `count`
- Use `assign` within `{% liquid %}` blocks for multiple assignments

### Comments
```liquid
{% comment %}
  Header section with configurable menu, logo, and cart.
  Includes responsive navigation and search functionality.
{% endcomment %}

{% # Single line comment for quick notes %}
```

## CSS Conventions

### Naming Convention (BEM-inspired)
```css
/* Block */
.header { }

/* Element (double underscore) */
.header__title { }
.footer__links { }

/* Modifier (double dash) */
.text--title { }
.button--primary { }
```

### CSS Variables
```css
/* Color variables */
--color-background: {{ settings.color_background }};
--color-text: {{ settings.color_text }};

/* Typography */
--font-body: {{ settings.font_body | font_face }};
--font-heading: {{ settings.font_heading | font_face }};

/* Layout */
--page-max-width: 1200px;
--page-gutter: 16px;

/* Component-specific */
--style-border-radius: 8px;
--content-max-width: 600px;
```

### Component Styles
```liquid
{% stylesheet %}
  .section-{{ section.id }} {
    /* Component-specific styles */
  }
  
  @media (min-width: 768px) {
    /* Tablet and desktop styles */
  }
{% endstylesheet %}
```

## File Organization

### Section Structure
```liquid
<!-- 1. HTML Markup -->
<section class="header">
  <!-- content -->
</section>

<!-- 2. Component Styles -->
{% stylesheet %}
  /* Styles specific to this section */
{% endstylesheet %}

<!-- 3. Schema Definition -->
{% schema %}
{
  "name": "t:sections.header.name",
  "settings": [
    /* ... */
  ]
}
{% endschema %}
```

### Schema Structure
```json
{
  "name": "t:sections.section_name.name",
  "blocks": [
    /* Define reusable blocks first */
  ],
  "settings": [
    /* Settings in logical groups */
  ],
  "presets": [
    {
      "name": "t:sections.section_name.preset_name"
    }
  ],
  "disabled_on": {
    "templates": ["password"]
  }
}
```

## Naming Conventions

### Files
- **Sections**: `header.liquid`, `product-grid.liquid`
- **Snippets**: `image.liquid`, `css-variables.liquid`
- **Templates**: Match Shopify resources: `product.json`, `collection.json`
- **Assets**: `critical.css`, `icon-cart.svg`

### Translation Keys
```liquid
{{ 'general.header' | t }}
{{ 'labels.menu' | t }}
{{ 'actions.add_to_cart' | t }}
```

## Best Practices

### Performance
```liquid
<!-- Preload critical assets -->
{{ 'critical.css' | asset_url | stylesheet_tag: preload: true }}

<!-- Responsive images with sizing -->
{{ image | image_url: width: 300 | image_tag: loading: 'lazy' }}
```

### Accessibility
```liquid
<!-- Always include alt text -->
{{ image | image_tag: alt: image.alt }}

<!-- Use semantic HTML -->
<nav aria-label="{{ 'general.navigation' | t }}">
  <!-- navigation content -->
</nav>
```

### Modularity
```liquid
<!-- Use snippets for reusable components -->
{% render 'image', 
  image: product.featured_image,
  sizes: '(min-width: 768px) 50vw, 100vw'
%}
```

### Documentation
```liquid
{% doc %}
  Renders a responsive image with proper srcset and sizes.
  
  Accepts:
  - image: Image object (required)
  - sizes: Sizes attribute for responsive images
  - loading: Loading strategy ('lazy' or 'eager')
{% enddoc %}
```

## Grid Layout Pattern
```css
.grid {
  --content-max-width: 600px;
  --content-columns-desktop: 3;
  --content-columns-tablet: 2;
  --content-columns-mobile: 1;
  
  display: grid;
  gap: var(--page-gutter);
  grid-template-columns: repeat(
    var(--content-columns-mobile),
    minmax(0, 1fr)
  );
}

@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(
      var(--content-columns-tablet),
      minmax(0, 1fr)
    );
  }
}
```

## Settings Usage
```liquid
<!-- Always use schema settings for customization -->
{% if section.settings.show_title %}
  <h2>{{ section.settings.title }}</h2>
{% endif %}

<!-- Use CSS variables for single properties -->
<div style="--style-max-width: {{ section.settings.max_width }}px;">
  <!-- content -->
</div>

<!-- Use CSS classes for multiple properties -->
<div class="alignment--{{ section.settings.alignment }}">
  <!-- content -->
</div>
```