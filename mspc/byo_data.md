# How to 'BYO' Data to the Planetary Computer

Unlike all of the other documentation for the MSPC,
documentation for 'bringing' your own data is scant to nonexistent.
This is probably because this is not necessarily the intended use of the MSPC.
That said, it is an entirely possible and valid one, and it can make a lot of sense
because:

1. you may not have any other equivalent cloud infrastructure (where you can drop in and immediately start using the full Python and R geostacks) on hand;
2. you can use MSPC as a testing and code-development environment for your analysis;
3. then you could either decide to use MSPC for your entire full analysis (it's free, and well built!) or decide to stand up your own compute (where your code could be moved/cloned and then should run more or less as is).

This was my logic when I decided to use MSPC for a project, but I had a lot of trouble figuring out a workflow for setting up and accessing my data, none of which is included within the MSPC Data Catalog.

Thus, I document herein the steps I took. As a fair warning, they may not be the optimal steps, and some could be unnecessary or even flat-out wrong. However, they appear to have worked for me, so I write them up in the hopes that will save some other poor soul some time and sweat.

So, here goes!

## I. set up cloud storage

Your data will need to live in [Azure cloud blob storage](https://azure.microsoft.com/en-us/products/storage/blobs) (and specifically in the 'westeurope' region, where the MSPC computing is located). (Note: blob comes from BLOB, or 'Binary Large OBject' – unstructured storage for data in binary format. It is stored in data lakes, and... aaaand that's pretty much the edge of my understanding, so chances are you'll be fine there too! 😅)

Thus, you will first need to set up a storage account and container. Here's a basic walkthrough of the process – or at least, the way I did the process, which was to default to scripting things (for reproducibility's sake) but not to be dogmatic about it. Because of that approach, most of the steps below present either images and/or code snippets.

### 1. Azure account creation
If you don't already have an Azure account then you'll need to create one [here](https://azure.microsoft.com/en-us/get-started/azure-portal) (should be straightforward).
![image](azure_signup.png)

### 2. Install the Azure CLI
Install the Azure Command Line Interface (CLI) to allow you to run commands from your local terminal. (Again, this is ideal from the point of view of
reproducibility, because you can figure out your exact step as a line of code, save it,
and then retrace your steps and/or redo them later on, if necessary...
such as I had to do in order to write up these notes!)
The installation will vary from one machine/OS to another, so the best I can do for is
to point you to [this link](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
and ask you to follow the instructions.

The next handful of steps I followed were basically those laid out [here](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-cli), and they included:

### 3. Log into your Azure account from the CLI

This is very straightforward: You'll just run the command `az login` from your local terminal/console.

[ASIDE: I recognize that this may not be straightforward if you're not used to using a terminal! If that is the case, here are two tips to get you set up:
1. Open whichever program you typically use to search for other programs on your computer and search for it (look for "command prompt" on Windows; look for "Terminal" on Mac or Linux), then click on the program to open it.
2. To run a command in the prompt, you just first type it out (exactly as you see it! including spaces, dashes, underscores, and copying capital/lowercase letters correctly), then hit <Enter>.
3. When you use a terminal/console, if you do work that creates files/folders/etc they will be created inside whatever folder you are running your terminal/console from. This is likely to be either your "Home" or "Desktop" directory, but you can change it by running `cd </YOUR/PATH/HERE/>` (where you would replace `/YOUR/PATH/HERE/` with, well, your desired path. Note that how to format paths is outside the scope of this documentation, but there is plenty of good information on the web and it is a very good skill to have!).]

Okay, so, you've opened your terminal and run `az login`. You should first see the terminal telling you that it's sending you to a browser window to complete the login: ![image](az_login_cmd.png)

Then your default browser should open a tab/window prompting you to log into your Azure account. This will look different depending on your institution/etc, but you would use the same login credentials that you created in step 1 (if you didn't already have an account).

Once you've successfully logged in, your terminal should register that, and then you should be good to go!

### 4. Create a new Azure resource group

### 5. Create a general-purpose Azure storage account

## II. convert data to cloud-native formats

## III. store data on Azure

## IV. index data with a [**STAC**](stac.md)

One last thing to mention here: At this final step, I was very confused for a while (and really, I still am!) about whether
or not to create an API for my STAC. 
I initially assumed I needed to do this because MSPC uses a STAC API to expose
its Data Catalog to users.
However, I eventually found a way to read and search the STAC and then read
data out of it without the use of an API:

```python

```

Thus, the long and short answers to my concern:

- *long*: I still don't know if maybe there would have been benefits to investing the time to create my own STAC API, but I've never made an API before, and it felt daunting when I had other things pressing for my time, and in the end it wasn't actually necessary, so I opted not to.
- *short*: maybe nice; not necessary.

