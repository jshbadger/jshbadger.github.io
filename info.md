1. Finish about page (include info about academics and experience?)
2. Change the icon at the top of the webpage (tab icon)
3. Fill out more of the projects that have been completed and in progress ones

Quick refresher if you haven't setup the hugo in a while.
`hugo server -D` will create a local hugo server to confirm your configuration is desired. The `-D` flag will include all files labelled as drafts.

If you want to host the temporary hugo server on an ip other than localhost, you can use
```sh
hugo server --bind 192.168.1.20 --baseURL http://192.168.1.20/
```

If you're installing the hugo repo to a new computer, you will need to redownload the submodule things. This can be done with
```sh
git submodule init
git submodule update
```
