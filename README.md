![Angular](https://img.shields.io/badge/angular-%23DD0031.svg?style=for-the-badge&logo=angular&logoColor=white) CookBook
=============

[_Angular CookBook_](https://multivers-dev.github.io/angular-cookbook/en) is an Open Source Angular Knowledge Project built for and by the community.

You can edit existing pages via top right pen icon button  and propose Pull Request without having to open vscode.
For new article, just create a folder and a markdown file you can also follow instructions below to project locally 



# Table of Contents
- [How to Install ?](#how-to-install-)
    - [Prerequisites](#prerequisites)
        - [Rust](#rust)
    - [dependencies](#dependencies)
- [Builds](#builds)
- [Serve](#serve)
- [How to Contribute ?](#how-to-contribute)

# How to Install ?

## Prerequisites
>### ![Rust](https://img.shields.io/badge/rust-%23000000.svg?style=for-the-badge&logo=rust&logoColor=white)
>### ![Cargo](https://img.shields.io/badge/cargo-%23000000.svg?style=for-the-badge&logo=rust&logoColor=white)


Install mdBook and the renderers used by this book
```sh
cargo install --locked --version 0.5.4 mdbook
cargo install --locked --version 0.5.1 mdbook-angular
cargo install --locked --version 0.4.0 mdbook-i18n-helpers
cargo install --locked --version 0.3.1 mdbook-svgbob
```


### i18n
> ## ![i18n](https://img.shields.io/badge/i18n-%23000000.svg?style=for-the-badge&logo=rust&logoColor=white)
>* Run 
>  ```sh
>    MDBOOK_OUTPUT='{"xgettext": {"pot-file": "messages.pot"}}' \ mdbook build -d po
>  ```
>  or 
>   ```sh
>   MDBOOK_OUTPUT='{"xgettext": {"pot-file": "messages.pot"}}' mdbook build -d po
>   ```
>
>
>* Update existing po for fr 
>  ```sh
>    msgmerge --update po/fr.po po/messages.pot
>  ```
>* Update existing po for en 
>  ```sh
>    msgmerge --update po/en.po po/messages.pot
>  ```

# Builds

>* Run `build` in fr 
>
>```sh
>  MDBOOK_BOOK__LANGUAGE=fr mdbook build --dest-dir book/fr
>```
>
>* Run `build` in en 
>
>```sh
>  MDBOOK_BOOK__LANGUAGE=en mdbook build --dest-dir book/en
>```
<br>

# Serve

>* Run `serve` in fr 
>
> ```sh
>MDBOOK_BOOK__LANGUAGE=fr mdbook serve -d book/fr -p 3000
>```
>
>* Run `serve` in en 
>
>```sh
>MDBOOK_BOOK__LANGUAGE=en mdbook serve -d book/en -p 3001
>```

## [How to Contribute?](./CONTRIBUTING.md)
