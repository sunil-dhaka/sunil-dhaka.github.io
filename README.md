# sunil-dhaka.github.io
Personal Website! 🤠

#### `Resources to help, when you setup your page 😄:`

1. [`How to setup git-page with your namecheap domain`](https://gist.github.com/notTag/4a60598d018124c9ac4a7b1f3e2bac9a)
2. `How to make your domain SSL certified`:
    - [What is CSR?](https://www.namecheap.com/support/knowledgebase/article.aspx/337/67/what-is-a-certificate-signing-request-csr/) 👍
    - When generating CSR code: If using `apache openssl` follow [this](https://www.namecheap.com/support/knowledgebase/article.aspx/9446/14/generating-csr-on-apache-opensslmodsslnginx-heroku/), and to know what others web servers are there visit [this](https://www.namecheap.com/support/knowledgebase/article.aspx/467/67/how-to-generate-csr-certificate-signing-request-code/) 
    - Command used to generate `csr` key:
    ```sh
    openssl req -new -newkey rsa:2048 -nodes -keyout `domain-name`.key -out `domain-name`.csr
    ```
    - [Complete DVC for SSL certificate guide](https://www.namecheap.com/support/knowledgebase/article.aspx/9637/68/how-can-i-complete-the-domain-control-validation-dcv-for-my-ssl-certificate/)
    - A good article that shows steps to get SSL certificate, is available on [freecodecamp](https://www.freecodecamp.org/news/how-to-add-an-ssl-certificate-and-custom-namecheap-domain-to-a-gitlab-pages-site-323f8f3ce642/). Some things that seem ambigious may be cleared using above links.
    - Can use [mxtools](https://mxtoolbox.com) to check whether your domain is in DNS record or not.
    - Funny thing is mine is still not certified. 😆

## Development

Run the local server from repo dir:

```sh
bundle install
bundle exec jekyll serve
```
