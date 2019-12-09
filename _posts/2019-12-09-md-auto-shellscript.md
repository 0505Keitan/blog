---
layout: post
title:  '記事の雛形を作ってくれるShellScriptを書いた'
date:   2019-12-09 12:35
permalink: 'md-auto-shellscript'
---
記事の通りです。多分めんどくさいのでほとんど使わないかも()

{% highlight shellscript %}
echo -n title:
read title
echo -n link:
read link

filetitle=`date '+%Y-%m-%d-'$link`
time=`date '+%Y-%m-%d %H:%M'`

echo '---' >> .files/$filetitle.md
echo "layout: post" >> .files/$filetitle.md
echo "title:  '$title'" >> .files/$filetitle.md
echo "date:   $time" >> .files/$filetitle.md
echo "permalink: '$link'" >> .files/$filetitle.md
echo "---" >> .files/$filetitle.md

mv .files/$filetitle.md _posts/
{% endhighlight %}