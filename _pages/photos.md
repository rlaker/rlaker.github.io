---
title: "Photography"
permalink: "/photos/"
author_profile: false
layout: splash
header:
    overlay_image: "/images/photos/header.jpg"
galleryfilm:
    - url: "/images/photos/test-1.jpg"
    - image_path: "//images.weserv.nl/?url={{ site.url | replace: 'http://','' | replace: 'https://','' }}{{ "/images/photos/test-1.jpg" | relative_url }}&w=300&h=300&output=jpg&q=50&t=square"
    - alt: "Test 1"
    - title: "Test 1 title"
    - url: "/images/photos/test-2.jpg"
    - image_path: "/images/photos/test-2.jpg"
    - alt: "Test 2"
    - title: "Test 2 title"
    - url: "/images/photos/test-3.jpg"
    - image_path: "/images/photos/test-3.jpg"
    - alt: "Test 3"
    - title: "Test 3 title"
---

# Photography

## Film Photography

{% include gallery id="galleryfilm" class="full" caption="Test caption" %}
