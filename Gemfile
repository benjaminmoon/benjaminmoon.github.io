source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "faraday", "0.17.3"

# remediation CVE-2020-14001
gem "kramdown", ">= 2.3.0"

# theme plugin
gem "minimal-mistakes-jekyll", ">= 4.21.0"

gem "tzinfo-data"
gem "wdm", "~> 0.1.0" if Gem.win_platform?

# https://github.com/advisories/GHSA-j96r-xvjq-r9pg
gem "activesupport", ">= 6.0.3.1"

gem "nokogiri", ">= 1.11.0.rc4"

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-include-cache"
  gem "jekyll-algolia"
end
