# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><img src="https://lastfm.freetls.fastly.net/i/u/64s/1edfd465e2c6d85950625add8f9f0ceb.png" title="Genesis - The Lamb Lies Down on Broadway"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/61e5e6db31b440bcb52d2b6d6cf8dd9a.png" title="X JAPAN - The Last Live"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/d4e731bf7b4d558ab985d335b55349a9.jpg" title="Gacharic Spin - 確実変動 -KAKUHEN-"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/7ee97a7c0e884ca6cc0da389481fafe8.png" title="Genesis - A Trick Of The Tail"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/bdf358a48437f28e7c870089ff911296.png" title="Genesis - Nursery Cryme"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/15ef1c034e1f6a71d0d69d3b6f5c534f.png" title="Lady Gaga - ARTPOP"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/1d20439b8467234f5e083884792cc5c6.png" title="Genesis - Selling England by the Pound"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/3ffd54b049a13d8d1b9ecc9a125d9aa2.jpg" title="Lady Gaga - Born This Way"> </p>

          
## 👩🏽‍💻 What you'll need
* A README.md file.
* Last.fm API key
  * Fill [this form](https://www.last.fm/api/account/create) to instantly get one. Requires a last.fm account.
* Set up a GitHub Secret called ```LASTFM_API_KEY``` with the value given by last.fm.
* Also set up a ```LASTFM_USER``` GitHub Secret with the user you'll get the weekly charts for.
* Add a ```<!-- lastfm -->``` tag in your README.md file, with two blank lines below it. The album covers will be placed here.

## Instructions
To use this release, add a ```lastfm.yml``` workflow file to the ```.github/workflows``` folder in your repository with the following code:
```diff
name: lastfm-to-markdown

on:
  schedule:
    - cron: '2 0 * * *'
  workflow_dispatch:

jobs:
  lastfm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: lastfm to markdown
        uses: melipass/lastfm-to-markdown@v1.3
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         IMAGE_COUNT: 6 # Optional. Defaults to 10. Feel free to remove this line if you want.
      - name: commit changes
        continue-on-error: true
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add -A
          git commit -m "Updated last.fm's weekly chart" -a

      - name: push changes
        continue-on-error: true
        uses: ad-m/github-push-action@v0.6.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}\
          branch: main
```
The cron job is scheduled to run once a day because Last.fm's API updates weekly chart data daily at 00:00, it's useless to make more than 1 request per day because you'll get the same information back every time. You can manually run the workflow in case Last.fm's API was down at the time, going to the Actions tab in your repository.

## 🚧 To do
* Allow users to choose the image size for the album covers.
* Feel free to open an issue or send a pull request for anything you believe would be useful.
