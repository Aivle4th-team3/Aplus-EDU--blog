# gh-pages용 블로그 레포지토리

> 블로그 사이트: [github.io/Aplus-Edu](https://Aivle4th-team3.github.io/Aplus-EDU)  
> 빌드 저장소: [Aivle4th-team3/Aplus-EDU](https://github.com/Aivle4th-team3/Aplus-EDU/tree/gh-pages)

## 포스트 추가 방법

1. \_posts 폴더 아래에 `YYYY-MM-DD-제목.md` 으로 마크다운을 작성한다.

-   YYYY-MM-DD는 포스트의 날짜가 된다.

2. md 파일에는 상단에 아래 문구를 추가한다.

    ```md
    ---
    layout: post
    title: 제목
    permalink: /posts/url주소/
    categories: [카테고리들]
    ---
    ```

-   포스트는 카테고리로 분류되어 사이트에서 볼 수 있다.
-   카테고리는 문자열 또는 리스트로 넣을 수 있다.
-   리스트로 넣으면 여러 카테고리에 속하게 되며, 순서에 따라 categories 탭에선 계층으로 보이게 된다.

## 로컬 빌드 방법

1. Ruby 설치

    [https://rubyinstaller.org/downloads](https://rubyinstaller.org/downloads)

2. Jekyll 설치

    ```sh
    $ gem install jekyll
    ```

3. Bundler 설치

    ```sh
    $ gem install bundler
    ```

4. Gem 종속성 설치

    ```sh
    $ bundle install
    ```

5. 빌드

    ```sh
    $ bundle exec jekyll build
    ```

6. 서버 실행
    ```sh
    $ bundle exec jekyll serve
    ```

-   참고
    [GitHub](https://docs.github.com/ko/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll) ,
    [Jekyll](https://jekyllrb.com/docs/installation)

## 서버 빌드 방법

[.github/workflows/jekyll.yml](https://github.com/Aivle4th-team3/blog/blob/master/.github/workflows/jekyll.yml)로 깃헙 레포지토리에 workflow가 적용되어 자동 수행된다.

1. 현재 깃헙 레포지토리로 push한다.

-   빌드 없이 푸쉬 하려면 커밋 메시지에 [skip ci]를 적는다. [참고](https://docs.github.com/ko/actions/managing-workflow-runs/skipping-workflow-runs)

2. 워크플로어가 깃헙 레포지토리에서 수동으로 빌드하고 원격 레포지토리([Aivle4th-team3/Aplus-EDU](https://github.com/Aivle4th-team3/Aplus-EDU/tree/gh-pages))의 gh-pages 브랜치에 결과물(\_site)을 푸쉬한다.

3. 원격 레포지토리에서는 gh-pages 브랜치에 푸쉬를 받으면 자동 빌드하여 블로그를 구성한다.

## 기초 템플릿

### Catbook 1.0 ([#a9fa120](https://github.com/starry99/catbook/tree/a9fa120f4492355cd524e037d255e20d47735ac1))

Catbook is a category-centric theme for Jeykyll. This theme is originally inspired from [Book theme](https://github.com/kkninjae/book).

[![LICENSE](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE) ![GENERATOR](https://img.shields.io/badge/made_with-jekyll-blue.svg) ![VERSION](https://img.shields.io/badge/current_version-1.0-green.svg)

### License

[MIT License](https://opensource.org/licenses/MIT)
