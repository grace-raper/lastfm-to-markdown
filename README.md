# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/James+Ferraro/Far+Side+Virtual"><img src="https://lastfm.freetls.fastly.net/i/u/64s/6000501b749222f1ae0d56f691878ad6.png" title="James Ferraro - Far Side Virtual"></a> <a href="https://www.last.fm/music/CHAI/PINK"><img src="https://lastfm.freetls.fastly.net/i/u/64s/c7061f6efaeb277c1accdb75b5e50ce3.jpg" title="CHAI - PINK"></a> <a href="https://www.last.fm/music/James+Dean+Bradfield/The+Great+Western"><img src="https://lastfm.freetls.fastly.net/i/u/64s/2ff921446fb24ad38d4062ccbafb70de.jpg" title="James Dean Bradfield - The Great Western"></a> <a href="https://www.last.fm/music/James+Blake/Overgrown"><img src="https://lastfm.freetls.fastly.net/i/u/64s/35071298781ffa59c5dcac373186428f.jpg" title="James Blake - Overgrown"></a> <a href="https://www.last.fm/music/Bad+Bunny/X+100PRE"><img src="https://lastfm.freetls.fastly.net/i/u/64s/e220bd192e63a48e6fddda59f3fc7662.jpg" title="Bad Bunny - X 100PRE"></a> <a href="https://www.last.fm/music/James+Ferraro/NYC,+Hell+3:00AM"><img src="https://lastfm.freetls.fastly.net/i/u/64s/b0eed3cd8e8646e384a31192ad978564.jpg" title="James Ferraro - NYC, Hell 3:00AM"></a> <a href="https://www.last.fm/music/Bad+Bunny/Un+Verano+Sin+Ti"><img src="https://lastfm.freetls.fastly.net/i/u/64s/c6dd5eae9d4d12e5aaca39304ea50059.gif" title="Bad Bunny - Un Verano Sin Ti"></a> <a href="https://www.last.fm/music/CHAI/PUNK"><img src="https://lastfm.freetls.fastly.net/i/u/64s/30dc63338a616f4d0c39ba9c86919166.png" title="CHAI - PUNK"></a> </p>

          
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
        uses: melipass/lastfm-to-markdown@v1.3.1
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         INCLUDE_LINK: true # Optional. Defaults is false. If you want to include the link to the album page, set this to true.
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
