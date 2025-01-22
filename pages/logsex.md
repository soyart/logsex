# Logseq SPA on GitHub Pages
	- > Some Logseq plugins might not work at all, but it should not break the documents
	- [logsex](https://github.com/soyart/logsex) provides an initial Logseq graph and CICD assets to integrate with GitHub Pages
	- There many use cases for Logseq publishing, including internal Wiki, and cheat sheets
	- logsex then allows us to focus on writing and pushing changes, and forget about the simple twist of fate
	- To reduce complexity, all content in the graph is considered public for this purpose (Logseq defaults to private content)
	  
	  > Consult Logseq configuration file if you want to make public only a small subset of graph content.
	- ## Why?
		- Although Logseq has publishing features built in, deploying the generated site manually to some URLs can be very painful
		- GitHub Pages is a good infrastructure for simple static sites
- # Write and build Logseq SPA
	- Fork this repository on GitHub
	- Start writing to Logseq, committing changes to Git when done
	- For each push to `master`, the Logseq static SPA will be built and deployed to branch `publish` via GitHub workflow `/.github/workflows/publish.yaml`
- # Publish with GitHub Pages
	- > logsex intentionally ignores GitHub workflow files for GitHub Pages because I'm too lazy and we generally do this once per repository anyway.
	- Enable GitHub Pages for your repository on the WebUI, with `Deploy from a branch` strategy, which should be the default
	- In GitHub Pages settings, configure the root of the website to the root of branch `publish`
	- After the SPA assets are made available in `publish`, GitHub Pages should be able to put it up on the designated URL
- # GitHub Pages with HTTPS
	- If you're using your own custom domain and want to enforce HTTPS, then there's a few extra steps.
	- You can start by simply clicking on "Enable HTTPS" or something of sort
	- Then you can use the UI to validate the HTTPS settings for you (this will most likely fail)
	- Go to your domain's DNS settings and configure it according to instructions from GitHub (this usually involves adding some DNS records)
	- ### [DNS Settings Guide from GitHub](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https#verifying-the-dns-configuration) (Jan 2025)
		- In some cases, a HTTPS certificate will not be able to be generated  due to the DNS configuration of your custom domain. This can be caused  by extra DNS records, or records not pointing to the IP addresses for  GitHub Pages.
		- To ensure a HTTPS certificate generates correctly, we recommend the following configurations. Any additional `A`, `AAAA`, `ALIAS`, `ANAME` records with the `@` host, or `CNAME` records pointing to your `www` subdomain or other custom subdomain that you would like to use with  GitHub Pages may prevent the HTTPS certificate from generating.
		- | Scenario | DNS record type | DNS record name | DNS record value(s) |
		  | ---- | ---- | ---- |
		  | Apex domain (`example.com`) | `A` | `@` | `185.199.108.153` `185.199.109.153` `185.199.110.153` `185.199.111.153` |
		  | Apex domain (`example.com`) | `AAAA` | `@` | `2606:50c0:8000::153` `2606:50c0:8001::153` `2606:50c0:8002::153` `2606:50c0:8003::153` |
		  | Apex domain (`example.com`) | `ALIAS` or `ANAME` | `@` | `USERNAME.github.io` or `ORGANIZATION.github.io` |
		  | Subdomain (`ww​w.example.com`, `blog.example.com`) | `CNAME` | `SUBDOMAIN.example.com.` | `USERNAME.github.io` or `ORGANIZATION.github.io` |