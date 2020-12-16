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

This widget supports [Mermaid](https://mermaid-js.github.io) code blocks. Try online with the [Mermaid live editor](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggVERcbiAgICBPUENVQVNWUltcQ2VudHJhbCBPUEMgVUEgU2VydmVyL10gLS0tIE1hc3RlckNvcmUoW0NlbnRyYWwgQ29yZV0pXG4gICAgV2ViQVBJe3tDZW50cmFsIFdlYiBBUEkgU2VydmVyfX0gLS0tIE1hc3RlckNvcmUoW0NlbnRyYWwgQ29yZV0pXG4gICAgTWFzdGVyQ29yZSAtLS0gTG9jYWxDb3JlQShbU2l0ZSBBIENvcmVdKVxuICAgIE1hc3RlckNvcmUgLS0tIExvY2FsQ29yZUIoW1NpdGUgQiBDb3JlXSlcbiAgICBMb2NhbE9QQ1VBU1ZSW1xTaXRlIEIgT1BDIFVBIFNlcnZlci9dIC0tLSBMb2NhbENvcmVCXG4gICAgTG9jYWxXZWJBUEl7e1NpdGUgQiBXZWIgQVBJIFNlcnZlcn19IC0tLSBMb2NhbENvcmVCXG4gICAgTG9jYWxDb3JlQSAtLS0gQ29ubjEoU2l0ZSBBIENvbm5lY3RvciAxKVxuICAgIExvY2FsQ29yZUEgLS0tIENvbm4yKFNpdGUgQSBDb25uZWN0b3IgMilcbiAgICBMb2NhbENvcmVCIC0tLSBDb25uMyhTaXRlIEIgQ29ubmVjdG9yKVxuIiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifSwidXBkYXRlRWRpdG9yIjpmYWxzZX0=).

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
