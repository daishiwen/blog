---
layout: post
category: 模板
tags: [Template - image]
title:  "Responsive Galleries with Foundation"
header: no
image:
   thumb: "gallery-example-1-thumb.jpg"
gallery:
    - image_url: gallery-example-1.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-2.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-3.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-4.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-5.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-6.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-7.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-8.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-1.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-2.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-3.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-4.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-5.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-6.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-7.jpg
      caption: Great images by Unsplash.com
    - image_url: gallery-example-8.jpg
      caption: Great images by Unsplash.com
---

You just need to choose a template like the `page` or `page-fullwidth` template and then just use `{% raw %}{% include gallery %}{% endraw %}`.
<!--more-->

{% include gallery %}


## How to embed a gallery

`{% raw %}{% include gallery %}{% endraw %}` lets you easily embed a gallery into your post. To use the gallery-include...


### Step 1

1. Make two images: a thumbnail and a big image.
2. Name the thumbnail *gallery-image-thumb.jpg* and...
3. ...name the big *gallery-image.jpg*.
4. Place them in the *images*-folder.


### Step 2

Define the big version in frontmatter,  

~~~
gallery:
    - image_url: gallery-image.jpg
~~~

If you like captions, give each image a caption:

~~~
gallery:
    - image_url: gallery-image.jpg
      caption: Starting Page with huge One Logo
~~~


### Step 3

Add the include whereever you want in your content with `{% raw %}{% include gallery %}{% endraw %}`.


## Other Template

{: .t50 }
{% include list-posts category='Template' entries='3' %}
