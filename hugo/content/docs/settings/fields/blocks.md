---
title: Blocks
publishdate: 2018-04-23 00:00:00 +0000
expirydate: 2030-01-01 00:00:00 +0000
date: 2018-04-23 00:00:00 +0000
---

The **Blocks** field type is a repeatable array of field groups. Unlike the [Repeatable Field Group](/docs/settings/fields/repeatable-field-group), however, the field groups within a Blocks field do not all have to be the same. Additionally, the field groups usable by a Blocks field are not defined ad-hoc: they must first be set up as [Partial Front Matter Templates](/docs/settings/front-matter-templates/#partial-templates).

## Options
![Blocks field options](/uploads/2018/04/blocks-field-ui.png)

- *Label* &mdash; the human-friendly label shown above the input field in the editor
- *Name* &mdash; the key stored in your content's front matter, used to access it in your templates
- *Description* &mdash; a human-friendly description of what the field does and/or instructions for your editors
- *Hidden* &mdash; hides the field in the editor; but allows developers to set default values or maintain the field for legacy purposes.
- *Templates* &mdash; select one or more [Partial Front Matter Templates](/docs/settings/front-matter-templates/#partial-templates) that can be added to the Blocks field.

## Field UI
![Blocks content interface](/uploads/2018/04/blocks-content-ui.png)

Before adding a block, the user selects which type of block they wish to add.

Blocks can be reordered in the UI by dragging and dropping.

## Templating

Items in the Blocks array will contain a `template` parameter that corresponds to the partial FMT they were created from.

The intended use is to iterate over the elements in your Blocks field and render different templates based on the value of the `template` field.

#### Hugo

Hugo's `partial` function includes a file from the `layout/partials` directory and allows us to pass a context. Passing `.` from inside the loop will pass all of the individual block's front matter into the partial template.

```
{{ range .Params.page_sections }}
    {{ if eq "hero-section" .template }}
        {{ partial "blocks/hero" . }}
    {{ end }}
    {{ if eq "call-to-action" .template }}
        {{ partial "blocks/cta" . }}
    {{ end }}
{{ end }}
```

#### Jekyll

In this Jekyll example, the individual block's front matter will be available to the included template via the `block` variable.

```
{% for block in page.page_sections %}
    {% assign template = block.template %}
    {% case template%}
    {% when 'hero-section' %}
        {% include blocks/hero.html %}
    {% when 'call-to-action' %}
        {% include blocks/cta.html %}
    {% endcase %}
{% endfor %}
```

## Config Files
You can configure this field in _Front Matter Template_ [Config Files](/docs/settings/config-files/) as follows:

```yaml
type: blocks
name: page_sections
label: Page Sections
template_types:
- example-partial-fmt-1
- example-partial-fmt-2
```

### Example
```yaml
page_sections:
    - template: hero-section
      title: Hello
      subtitle: World
    - template: call-to-action
      heading: Get Started
      button_url: https://forestry.io/signup
      button_text: Sign up for Forestry
```
