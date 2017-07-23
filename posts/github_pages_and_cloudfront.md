# GitHub Pages with Cloudfront for SSL termination

## Summary

My coworker and I attempted to configure this recently. Due to some oversights
on my part, we ran into some interesting problems. I wanted to keep a note of
how we overcame these issues.

Here's what we hoped to accomplish:

* Use Cloudfront to terminate SSL and provide HTTPS connectivity to the GitHub
  Pages site under our domain name
* Use Cloudfront in order to minimize our footprint of services that we use.
  We could certainly use Cloudflare but I was hoping to minimize our need for
  additional services in our environment

## The issue

We found a great [blog
post](http://strd6.com/2016/02/github-pages-custom-domain-with-ssltls/) from
Daniel Moore which allowed us to quickly and easily front our GitHub Pages site
with Cloudfront. The mistake we made was fairly simple. We were also following
the GitHub documentation which asks for a CNAME file in your GitHub Pages
repository. Without thinking about this, you migth be able to guess what will
happen.

It appears that placing a CNAME file in your GitHub pages repository will result
in an HTTP redirect to your custom domain when you request the github.io URL.

Our Cloudfront origin was set to our github.io URL and, following the blog post
linked above, we had our custom domain with an alias record set in Route53 to
point at our Cloudfront distribution with an origin of our github.io URL. That
GitHub Pages repository contained a CNAME file with our custom domain.

Every request to our custom domain would hit Cloudfront which would request the
github.io URL and GitHub would return a redirect back to our custom domain. This
resulted in endless redirects.

The solution was to remove the CNAME file from the GitHub repository. Cloudfront
would then immediately get a 200 response with the GitHub pages content that we
wanted to return.

In our particular case, we also had to deal with cache invalidations because
Cloudfront had cached the redirect returned by GitHub.

I hope that helps if you run into similar issues!

[Back](/index.html)
