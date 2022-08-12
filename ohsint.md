# [OhSINT](https://tryhackme.com/room/ohsint)
> Are you able to use open source intelligence to solve this challenge?

## Introduce

The challenge provides an image to be downloaded. Opening the image in an image viewer does not reveal anything special.

![WindowsXP](https://user-images.githubusercontent.com/90561566/184367655-fcb261e8-ca27-4ff2-8899-19fe0126e4ce.jpg)

A quick to-do when exploring images is to check for its metadata.

```
Image metadata is text information related to an image file that is usually attached to the file.
It includes details relevant to the image.
This attached information can be EXIF files, IPTC files, 8BIM files or ICC files.
EXIF files hold dozens of details that can be valuable to an OSINT investigator. It can have information such as:
when the picture was taken, where it was taken, the shutter speed, the owner of the image,...
```

## Exploring EXIF data

Simply, the EXIF data can be observed by checking out the Properties of the Image and navigating to the Details Panel.

```
This provides trivial information but does not paint the complete picture.
More can be dug up using ExifTool.
Exiftool is a powerful command-line application for reading,
writing and editing meta information in a wide variety of files.
```

![image](https://user-images.githubusercontent.com/90561566/184367577-eb8797fe-e3dc-4f04-9b4a-b16a565eab80.png)

Let's observe the image with ExifTool.

![image](https://user-images.githubusercontent.com/90561566/184367783-70e34858-bb50-4fbb-a265-1ab8cb08227d.png)

Aha! It is clear that someone holds the copyright on this image, OWoodflint. It hints to be a username.

The next step is to find if there is any information on the internet about "OWoodflint". Investigation continues!

## Google Dorking

Search "OWoodflint" on google or any search engine and try to gather meaningful data.

![image](https://user-images.githubusercontent.com/90561566/184368080-fe1c2b9f-abc7-48b3-8a57-2055a49ec4ae.png)

3 leads have been discovered connected to this username.

The Twitter account reveals the answer to the 1st question of the challenge. The user's avatar is a cat.

![image](https://user-images.githubusercontent.com/90561566/184368148-1f1bf27f-02d4-43a9-a3d0-bd3154667a01.png)

Other information has extracted from exif is GPS location, we can search on the map `54° 17' 41.27" N, 2° 15' 1.33" W`

![image](https://user-images.githubusercontent.com/90561566/184368771-c1294b78-e112-4728-8a9d-f715c61d3035.png)

I don't see any information for the question, but it could be useful information in real practice

![image](https://user-images.githubusercontent.com/90561566/184369137-6d8254ba-bee1-432c-93ca-bf57094919fa.png)

He shared a story about a free wifi on Twitter

## Identifying WAPs

```
BSSID is an unique address given to a Wireless Access Point (WAP) to recognize it.
It is similar to a MAC address on a PC.
Lucky for us, there are websites for collecting information about the different wireless hotspots around the world.
These hotspots can be identified using SSID, GPS coordinates and BSSID.
```

To use [wigle.net](https://wigle.net/), an account should be created.

![image](https://user-images.githubusercontent.com/90561566/184370245-c443c26e-0320-4bdf-b985-3f9036c3ce53.png)

Searching for OWoodflint's BSSID points us to the user's location which is London. Another question solved!

SSID is a customizable name given to a WAP. The SSID of OWoodflint's WiFi as `UnileverWiFi`.

## Further Investigation

It's time to explore the other search results. On his Github repo, we can found his email he left.

![image](https://user-images.githubusercontent.com/90561566/184370722-3af725f9-ef61-4ff8-959c-c88c67051d73.png)

The next task is to find out this user's holiday location. Conveniently, the user has disclosed this in their blog.

![image](https://user-images.githubusercontent.com/90561566/184370814-ed63292d-9a19-46c2-af78-c1768055cb80.png)

I hope they had a great experience in New York.

he final task is to find OWoodflint's password. It is a bit trickier.

After going through the pages, again and again, searching endlessly for the password, it was placed in a basic location.

![image](https://user-images.githubusercontent.com/90561566/184371591-67e9df70-077c-4ec9-a1fa-fab7d95a0307.png)

he hid it with white color right on his blog page.
