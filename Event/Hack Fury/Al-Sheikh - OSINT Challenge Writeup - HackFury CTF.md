# Introduction
This challenge, titled "Al-Sheikh", was a medium-difficulty OSINT challenge presented at the HackFury CTF competition. The challenge provided a single image showing an aerial view of a busy highway, a golf course, and a sunset over a hazy skyline.
The objective was to identify three pieces of information from the image alone:

The city where the photo was taken
The country it belongs to
The name of a nearby café
![[place.PNG]]

# **Initial Analysis & Geolocation**

At first glance, the image appeared to show an aerial view of a modern city — a busy multi-lane highway, a lush golf course, and a hazy sunset over a distant skyline. The photo was clearly taken from a high floor of a building, likely a hotel or residential tower.

The first step was to scan the image carefully for any unique or distinguishable element that could be searched. One structure stood out immediately — a distinctive modern architectural landmark visible in the background near the golf course. It had an unusual, artistic silhouette that didn't look like a typical building.

Using **Google Images reverse search**, I uploaded the image and focused on that landmark. The results pointed directly to the **Dubai Creek Golf & Yacht Club** — a well-known venue in Dubai featuring its iconic sail-shaped clubhouse.

This single finding was enough to answer the first two questions:

- **Country:** UAE 🇦🇪
- **City:** Dubai 🏙️
![[first.png]]

![[google.png]]
![[q1.png]]
![[q2.png]]

# **Finding the Nearby Café**

With the location confirmed as the **Dubai Creek Golf & Yacht Club**, the next step was to find a nearby café.

First, I queried Google for the club's coordinates, which returned:

- **Latitude:** 25.2443° N
- **Longitude:** 55.3333° E

I then plugged these coordinates into **Google Earth** and began exploring the surrounding area. Since the challenge asked for a café, I searched using the keyword **"coffee"** within the area — and one result immediately appeared: **Costa Coffee**.

This turned out to be the correct answer to the third question:

- **Nearby Café:** Costa Coffee ☕
![[cor.png]]![[club.png]]![[coffee.png]]
![[q3.png]]
# **Conclusion**
س
This was honestly one of the most enjoyable challenges I worked on. What made it special wasn't its complexity — it was the lesson it taught: **always start simple.**

It's easy to overthink OSINT challenges and jump straight to advanced tools and techniques. But "Al-Sheikh" was a great reminder that sometimes a single reverse image search is all you need to crack open the whole challenge.

By staying methodical and observant, I was able to answer all three questions:

- ✅ **Country:** UAE
- ✅ **City:** Dubai
- ✅ **Nearby Café:** Costa Coffee

A great challenge that sharpened both my geolocation instincts and my OSINT mindset. 🕵️‍♂️![[finaly.png]]