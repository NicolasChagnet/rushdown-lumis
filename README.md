# rushdown-lumis

This is an extension for the [rushdown](https://crates.io/crates/rushdown) crate.

Highlight the code blocks of a markdown document with [lumis](https://lumis.sh). lumis highlights code with the grammars of [tree-sitter](https://tree-sitter.github.io/tree-sitter/) and the colorschemes of Neovim, and writes each block as HTML, while the document is rendered.

## Usage

Add the crate via `cargo add rushdown-lumis` and include it within your rushdown parser

```rust
let theme: rushdown_lumis::Theme = "catppuccin_mocha".parse().unwrap();

let parser = rushdown::new_markdown_to_html(
    Default::default(),
    Default::default(),
    rushdown::parser::empty_parser_extension(),
    rushdown_lumis::lumis_html_renderer_extension(rushdown_lumis::Options::new(theme)),
);

let mut html = String::new();
parser(&mut html, "```rust\nlet answer = 42;\n```").unwrap();
assert!(html.contains(r#"<span style="color: #cba6f7;">let</span>"#));
```

No stylesheet is needed: every colour of a block is written inline. A page which wants the small niceties that lumis stylesheets add can ask `lumis themes build-css <theme>` for the classes of the theme.

The default build highlights Rust, Shell, C, C++, Go, JavaScript, TypeScript, HTML, CSS, Python, SQL, JSON, YAML, TOML, Markdown and diff. Four features add more: `web`, `backend`, `config-files` and `all-languages`. A site which needs one language outside the default set can depend on lumis itself with the feature for it, which cargo joins to the ones this crate asks for.
