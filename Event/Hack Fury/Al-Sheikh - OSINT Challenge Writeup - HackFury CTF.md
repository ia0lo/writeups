# Introduction
This challenge, titled "Al-Sheikh", was a medium-difficulty OSINT challenge presented at the HackFury CTF competition. The challenge provided a single image showing an aerial view of a busy highway, a golf course, and a sunset over a hazy skyline.
The objective was to identify three pieces of information from the image alone:

The city where the photo was taken
The country it belongs to
The name of a nearby café
<img width="1170" height="2076" alt="place" src="https://github.com/user-attachments/assets/27ee0cb4-41f1-4478-bb32-bd1cd932182e" />
# **Initial Analysis & Geolocation**

At first glance, the image appeared to show an aerial view of a modern city — a busy multi-lane highway, a lush golf course, and a hazy sunset over a distant skyline. The photo was clearly taken from a high floor of a building, likely a hotel or residential tower.

The first step was to scan the image carefully for any unique or distinguishable element that could be searched. One structure stood out immediately — a distinctive modern architectural landmark visible in the background near the golf course. It had an unusual, artistic silhouette that didn't look like a typical building.

Using **Google Images reverse search**, I uploaded the image and focused on that landmark. The results pointed directly to the **Dubai Creek Golf & Yacht Club** — a well-known venue in Dubai featuring its iconic sail-shaped clubhouse.

This single finding was enough to answer the first two questions:

- **Country:** UAE 🇦🇪
- **City:** Dubai 🏙️
<img width="289" height="224" alt="first" src="https://github.com/user-attachments/assets/22cab67c-3ccf-4c22-9d58-b5c6038b595b" />
<img width="1770" height="934" alt="google" src="https://github.com/user-attachments/assets/588a5515-978b-4f0a-a9e0-333c6180f0d9" />
<img width="1498" height="148" alt="q1" src="https://github.com/user-attachments/assets/0666dd1f-9023-4310-aa20-bde491c0503b" />
<img width="1498" height="136" alt="q2" src="https://github.com/user-attachments/assets/983c361c-0e7f-4e45-8953-6d9c0d433fd6" />

# **Finding the Nearby Café**

With the location confirmed as the **Dubai Creek Golf & Yacht Club**, the next step was to find a nearby café.

First, I queried Google for the club's coordinates, which returned:

- **Latitude:** 25.2443° N
- **Longitude:** 55.3333° E

I then plugged these coordinates into **Google Earth** and began exploring the surrounding area. Since the challenge asked for a café, I searched using the keyword **"coffee"** within the area — and one result immediately appeared: **Costa Coffee**.

This turned out to be the correct answer to the third question:

- **Nearby Café:** Costa Coffee ☕
<img width="1145" height="937" alt="cor" src="https://github.com/user-attachments/assets/d1cc1ffe-44ab-4080-b3d7-3c1b13077c33" />
<img width="1726" height="916" alt="club" src="https://github.com/user-attachments/assets/2b9dfc58-3818-42a7-ba43-a815b235f466" />
<img width="1726" height="916" alt="coffee" src="https://github.com/user-attachments/assets/619d575b-6523-4738-9b70-ea5603d75544" />
<img width="1498" height="116" alt="q3" src="https://github.com/user-attachments/assets/cd7c4f3a-1222-425f-96d6-7eac77d6dd59" />

# **Conclusion**
س
This was honestly one of the most enjoyable challenges I worked on. What made it special wasn't its complexity — it was the lesson it taught: **always start simple.**

It's easy to overthink OSINT challenges and jump straight to advanced tools and techniques. But "Al-Sheikh" was a great reminder that sometimes a single reverse image search is all you need to crack open the whole challenge.

By staying methodical and observant, I was able to answer all three questions:

- ✅ **Country:** UAE
- ✅ **City:** Dubai
- ✅ **Nearby Café:** Costa Coffee

A great challenge that sharpened both my geolocation instincts and my OSINT mindset. 🕵️‍♂️
<img width="1498" height="561" alt="finaly" src="https://github.com/user-attachments/assets/0b10bc9c-2edc-4331-bcd5-e6e45ad36ed2" />
