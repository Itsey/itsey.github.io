# Add New Image to Site

[meta-title]: #Information;#Level100;

### Perquisites

* Folder of Images.
* Photoshop CC 2019 Installed
* Photoshop Script Loaded

### Steps

[step]:

#### 10 -Copy Images to Import Path.

Copy the new images to d:\Files\Working\JSBAssets\Incomming
[step]:

#### 15 Open Photoshop

[step]:

#### 20 - Execute Photoshop Import Macro

[meta]#Beginner;
Open the Actions menu within the actions menu locate "JSBGalleryOnly" Action - note this is bound to F5.
[step]:

#### 30 - Execute Macro (F5)

Select the image from the load dialog.
Change the ratio on the crop tool from Ratio to WxHxResolution ( Top Left Dropdown)

Reposition the crop tool on the Image.

Tick the crop selection tick.
[step]:

#### 40 - Execute The Linqpad Macro

[meta]:#Reference;#sensitive;

Ensure that Linqpad is connected to: SQL5104.site4now.net
The username is: db_a4b54e_jsb2_admin
The password is in Keepass.
[meta]:#Beginner;
Look for the text in the output like this:
B0XXX Created

[step]:

#### 50 Go To Step 20

Repeat from step 20 until all images are imported.
[step]:

#### 60 FTP Transfer Missing Images

Open WinSCP
On the left hand pane navigate to D:\Files\Working\JSBAssets\MasterPhotos\Images\2024\ where the year is the last part of the path.

On the right hand pane navigate to 
/justsoballoons/sbstatic/imgs/i/2024/

Copy all missing image folders from the left to the right.

Wait for the upload to complete.
[step]:

#### 70 Navigate to JSB Site.

[https://www.justsoballoons.co.uk/jsb/]
At the bottom select Customer Login.
Go to the All Images Page.

[meta]:#Reference;#Sensitive;

Username: Sabina.

Login.

Click See All Images.
Scroll to the bottom of the page.

The new images should show with a New overlay.

![](.\images\jsb-new-image.png)
[meta]:
