# Markdown Viewer

The widget can be used to show [GitHub Flavored Markdown](https://github.github.com/gfm/).

## Model

The content can be a plain markdown string or encoded as Base64. The latter is preferred.

```json
{
    "type": "markdownviewer",
    "content": "# Markdown Viewer\n\n- This is a markdown text."
}
```

Base64 encoded content:

```json
{
    "type": "markdownviewer",
    "content": "IyBNYXJrZG93biBFeGFtcGxlCgpUaGlzIGlzIGFuIGV4YW1wbGUgdGV4dC4KCiMjIFN5c3RlbSBDb21wb25lbnQgT3ZlcnZpZXcKCnwgTmFtZSB8IERlc2NyaXB0aW9uIHwKfCAtLS0tIHwgLS0tIHwKTWFzdGVyIENvcmUgfCBDZW50cmFsIENvcmUgc2VydmljZSB0eXBpY2FsbHkgZGVwbG95ZWQgaW4gdGhlIChwcml2YXRlKSBjbG91ZC4KTG9jYWwgQ29yZSB8IENvcmUgd2hpY2ggaXMgdHlwaWNhbGx5IGRlcGxveWVkIGF0IGEgc2l0ZS4KQ29ubmVjdG9yIHwgUmV0cmlldmVzIGRhdGEgZnJvbSBhbmQgc2VuZHMgZGF0YSB0byBvbmUgb3IgbXVsdGlwbGUgZGF0YSBzb3VyY2VzLgpTZXJ2ZXIgfCBFeHBvc2VzIGRhdGEgYWNjb3JkaW5nIHRvIHRoZSBsYXRlc3QgT1BDIHNwZWNpZmljYXRpb25zLgpXZWIgQVBJIHwgU2VydmljZSB3aXRoIGEgcG93ZXJmdWwgSFRUUCwgV2ViU29ja2V0IGFuZCBzdGF0aWMgY29udGVudCBpbnRlcmZhY2UuCg=="
}
```

Tip: Write your complete Markdown text in [Visual Studio Code](https://code.visualstudio.com) and convert it to Base64 by using the [Encode Decode](https://marketplace.visualstudio.com/items?itemName=mitchdenny.ecdc) extension.

## Options

```jsonc
{
    "options": {
        "linkTarget": "_blank"
    }
}
```

## Mermaid

This widget supports [Mermaid](https://mermaid-js.github.io) code blocks.

<!-- markdownlint-disable MD048 -->
~~~md
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```
~~~
<!-- markdownlint-enable MD048 -->

### Mermaid Options

The theme of mermaid graphs can be set with one of the following values : `default`, `neutral`, `forest` or `dark`.

See [Mermaid Options](https://mermaid-js.github.io/mermaid/#/theming?id=theme-variables-reference-table)

```jsonc
{
    "mermaidOptions": {
        "theme": "forest",
        "themeVariables": {}
    },
}
```
