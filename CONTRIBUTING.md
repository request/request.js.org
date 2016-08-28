
Hey :wave:

1 - If you haven't done so install gitbook locally:

```bash
npm install -g gitbook-cli
# install the latest version (stored in ~/.gitbook)
gitbook fetch
```

2 - If the book have any plugins in it (optional):

```bash
# install plugins
# the plugins should be listed in books.json
# without the `gitbook-plugin-` prefix!
gitbook install
```

3 - Each time you modify something:

```bash
# cd into your book's dir and generate it (_book folder)
gitbook build
# serve your book at http://localhost:4000
gitbook serve
```

4 - Check if everything looks good and all links are working.

5 - Submit PR with your changes and wait for 24 hours. If there is no response or any other objection merge the PR.
