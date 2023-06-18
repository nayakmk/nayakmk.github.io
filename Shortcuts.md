
## Prerequisite

https://github.com/jekyll/jekyll-compose


## Create Post

bundle exec jekyll post "Helm: Getting Started" --timestamp-format "%Y-%m-%d %H:%M:%S %z"

https://github.com/jekyll/jekyll-compose

## Draft Post

bundle exec jekyll post "Microservices: Outbox Pattern" --timestamp-format "%Y-%m-%d %H:%M:%S %z" --drafts


## Running Locally

bundle exec jekyll s

jekyll server --watch

### With Drafts

jekyll server --drafts --watch


## Link Post

https://github.com/MichaelCurrin/dev-cheatsheets/blob/master/cheatsheets/jekyll/templating/links.md?plain=1

[Link Text]({% post_url 2023-06-18-reactive-java-understanding-flux-window-and-windowtimeout-operators %})