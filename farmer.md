# Setup a Farmer
To get started with Template Farm you'll need at least one instance of the Farmer installed on your PC (Windows only). The Farmer is a light-weight app that
monitors a folder in your local or network storage (just like Dropbox or Google Drive) and controls AE instances.

Any AE project that will be saved into the Watch Folder that is monitored by the Farmer will show up in Template Farm as a template in a matter of seconds.
Start by downloading the Farmer [here](https://www.templatefarm.io/download). After the download is done, start the installer.
The installer is a straight forward process, just follow the instructions of the installation wizard.

## Setup a Watch Folder
Once the Farmer is installed, run it. If you are running the Farmer for the first time on this your PC, you will be asked to choose a shared folder.
The Watch Folder, is a folder in your local or network storage that will be monitored by the Farmer. Any AE project that is saved into the Watch Folder will show up
online as a template in a matter of seconds. If you never ran the Farmer on the PC you installed it on, it will ask you to choose a Watch Folder automatically. If you need to modify or change the
Watch Folder location, click the **Settings** button and go to the **Watch Folder** tab.

### Picking a Watch Folder location
First, you'll need to pick the location of your Watch Folder. Click on the **Browse** button next the **Path** property and select the folder you want the Farmer to monitor.

> Always pick the root folder in your folders structure. The Farmer monitors all sub folders within the folder you choose.
> Make sure that the Watch Folder has all the read / write permissions.

### Considerations for Local vs Network Location
When choosing a Watch Folder you can choose a Local Folder (a PC local drive) or a Network Share. While a local folder is easier and quicker to setup,
the network share has a lot of advantages to it and sometime might be necessary. This is why you should should consider a Watch Folder on a Network Share:
* If you have multiple artists that are going to save AE projects in the Watch Folder from different workstations.
* If you plan to have more than one Farmer and you plan to cluster them, all Farmers must have the same Watch Folder path defined.
* If you plan to define a network share as your Watch Folder, make sure you have all the permissions an always run the Farmer **As Administrator**.
* When defining a Watch Folder on a network share, consider the speed of your network. The Farmer will need to read and write data into the Watch Folder. A network connection of minimum 1gb is recommended.

### Filtering the files
By default the Farmer will pickup any After Effects project that is saved in the Watch Folder or any of its sub-folders. In some cases you'll want to filter the files that the Farmer picks up and converts to templates.
For example, if After Effects is configured to generate an **auto save** version of your projects every period of time, you might want the Farmer to ignore these files and pick up only the original project file.
This is where the **Filter** setting of the Watch Folder becomes handy. The **Filter** setting is using a selection pattern to select or ignore files that start, end or contain specific strings.

For example, if we want the Farmer to ignore the **auto save** files of After Effects, we will type **auto-save** in the **Filter** filed and check the **Exclude Results** checkbox next to it.
What this essentially tells the Farmer to do is: Select all the files that contain 'auto-save' string anywhere in their name (note the astrix before and after) and ignore them.

### Monitoring Refresh rate
The farmer will periodically scan the Watch Folder to check if new content was added, a new After Effects project was added or an existing projects was changed since the last scan.
During those periodic scans, Farmer will convert the project into a template that the users will eventually see online in Template Farm.

You can change the rate in which the Farmer will do those periodic scans, by changing the
**Refresh Rate** property in the Watch Folder settings. The refresh rate value is in milliseconds.

> Setting this property to less than 2000 (2 seconds) might cause unstable results.

> Once the Watch Folder was defined, you might need to restart the Farmer.

## Setup After Effects
Once the Watch Folder is setup, you'll need to tell the Farmer what After Effects instance it should use to read, write and modify projects. Open the **Settings** window, go to the **Plugins** tab and
select the **AeFarmerPlugin** tab. This is where you will configure the general settings that the Farmer will use to communicate with After Effects. Some of those settings can be overridden by individual projects.

### After Effects root folder
First, you'll need to choose the After Effects root folder. This is where the executable files of After Effects live on your PC. More specifically, we are looking for the location of the **AfterFX.exe** file.
Usually, the After Effects folder will be located in a path that looks something like this: **C:\Program Files\Adobe\Adobe After Effects CC 2018\Support Files**.
Depending on the version of your After Effects this location might slightly differ.
> You can always run a file search for **AfterFX.exe** to find the location of your After Effects installation.

Next, you'll need to configure the initial settings that After Effects should use when previewing and rendering content. There are four presets in total, two for preview and two for rendering.
Each setting is a name of a preset you can define inside of After Effects under **Edit > Templates**.

### Render Settings
This is the name of the Render Settings template that After Effects will use when users will render content from Template Farm. In After Effects, go to **Edit > Templates > Render Settings**.
Use the name of the template as it shows in the **Settings Name** drop down. Normally, this render settings template will be configured to the highest quality since this is the final output setting.

### Output Module
This is the name of the output module template that After Effects will use  when users will render content from Template Farm.
In After Effects, go to **Edit > Templates > Output Modules**. Use the name of the template as it shows in the **Settings Name** drop down.

### Preview Render Settings
This is the name of the Render Settings template that After Effects will use when users will preview content in Template Farm. Here you can add some variation to the render settings template,
to allow additional flexibility and convenience. Some things to consider for the Preview Render Settings:
* Set a lower **Quality** setting to improve speed when the Farmer communicates with Template Farm.
* Set a lower **Resolution** setting to improve speed when the Farmer communicates with Template Farm.
* Allow rendering **Guide Layers** to provide more visual feedback to user while previewing the content.

### Preview Output Module
This is the name of the Output Module template that After Effects will use when users will preview content in Template Farm. When users preview content in Template Farm, the Farmer is expecting After Effects to return a single or multiple frames since previewing a full video each time will take too long.
This is why it is very important that the preview output module template will be set to return images. When editing the output module template settings, choose **JPEG Sequence** from the **Format** drop down.
If you need your preview with an Alpha Channel (transparency), you can pick the **PNG Sequence** format, but be aware that the size of each preview frame will be larger and this might affect the communication
speed between the Farmer and Template Farm.

> This kind of Output Module does not exist in After Effects by default (unlike **Best Settings** for the Render settings). You'll need to create this Output Module template on your own.

### Optimized Preview
Template Farm communicates with the Farmer each time a user navigates, previews or renders out content. In turn, the Farmer is communicating with the local After Effects instance. Each time a preview
or a render is initiated, the Farmer will trigger After Effects and tell it what to do. This process normally runs in the background and you'll see nothing visually happening on the PC that the Farmer is installed on.
The Farmer will launch After Effects as a background process, preform required tasks and shut down the After Effects instance upon completion. If someone is using the PC while this is happening,
they will see nothing even if the use After Effects at that moment. This process will happen each time a new task
will be received from Template Farm. Because the Farmer will launch and shut down After Effects each time, this might result in a slower preview times.

The **Optimized Preview** setting tells the Farmer that it always should use an already open instance of After Effects, this will eliminate the time we need to wait while the After Effects is loading.
If you enable this option, the Farmer will run the process in the foreground and you'll starting seeing After Effects window popup into focus each time a preview or a render task is received by the Farmer.
While this will speed up the preview time, it will also make the PC un-usable, since it will override what anything that is being done in After Effects at the moment.

Some things to consider before enabling **Optimized Preview**:
* A PC dedicated for Template Farm use will ensure the processing power is not split with other tasks.
* A high volume of previews.
* Are you previewing heavy projects that take a long time to load? If so, the combined time between loading After Effects each time and loading a heavy project each time, might result in a long waiting time for each preview.

### Default Output Folder
The default output folder is where the final output will be rendered to. If the default output folder is not set, the final output will be rendered in the same folder where the original After Effects file is stored (ie the Watch Folder). This setting can be overriden on a per-project basis and editing the local settings of the .tfp file. More information is available in the Unique Project Settings section.

## Connecting to Template Farm
Once you've configured the Watch Folder and the After Effect settings of the Farmer, you can connect the Farmer to your account. In order to do that you'll need to generate a unique key
that will be used as a link between the Farmer and your Template Farm account.
To generate a new key, login to your Template Farm account, in the header, under the account drop down, choose **[Settings > Farmers](https://www.templatefarm.io/account/settings/farmers)**.
Each Farmer must have it's own unique key, you can generate as much unique keys as your account allows.

Click the **Add Farmer** button, a new popup will show up. Give the Farmer a name. The name is for your convenience and will not change anything in the Farmer settings itself. You can also choose
to assign this Farmer to a cluster if needed (this setting can be adjusted at any time). Once you click **Done** and new Farmer key will be added to the list of Farmers.
Copy the key of the Farmer and and launch the Farmer on your PC. Once the Farmer starts, it will show a login window in which you'll need to paste the key you just copied and click **Login**.
Once the Farmer connects to Template Farm, it will print **=========== FARMER IS READY TO GO ===========** in the log window.

> If your organization is using proxies to connect to the internet, you'll need to get the proxy settings info from your web admin and input it in the login window.

At this point you are ready to star using Template Farm. Click the **Home** button in the header and you should see your Watch Folder in the navigation screen.
Next you'll need to create some After Effects projects that will be used as templates by Template Farm.

# Unique Project Settings
When an After Effects project is saved in the Watch Folder (or any of the sub folders in the Watch Folder), the Farmer will generate a side-car file with the same name as the After Effects project and a .tfp extension. These files contain the information needed
by Template Farm to generate the online Template. The .tfp file also contains individual project settings that will allow you to setup properties that won't be accessible online and override the general
properties that were setup in the After Effect section of the Farmer. You can edit the .tfp files by simply double clicking them. This will bring up the local settings editor which is installed automatically
when the Farmer is installed.
> If the .ftp files association is not registered you can always open the local settings editor manually by running **LocalSettingsEditor.exe** from the Farmer installation folder
(normally located in C:\Program Files\TwiztedDesign\FARMER)

## Render output location
By default, when users will render new content in Template Farm, the final render file will be rendered in the same folder as the After Effects project. Usually this is not the desired behavior.
Ideally the After Effects projects should be located in a folder to which the artists will have access (the Watch Folder), while the final rendered files should land in a location that is accessible for producers
and other members of the team. This location can be anywhere in your local network, but it will not be exposed online for security reasons (Farmer doesn't expose you local storage).

When you open a .tpf file, you can change the output location for the final render. In the **Settings** tab of the local settings editor, under **Output Settings**, check the **Save to** checkbox.
Click the browse button and select the folder where you would like the final render to be saved. From now on, each render that will be generated from the corresponding After Effects project will be rendered to the
folder you have selected and not next to the After Effects project.

> This setting can work in combination with the **Custom Naming** settings that allow you to generate the name of the final rendered file
dynamically based on different properties. You can define the **Custom Naming** settings in the project in Template Farm.

## Override After Effects settings
During the initial setup of the Farmer we configure the Render Settings and the Output Modules the After Effects will use when rendering and previewing content. These settings are global and will be used
for all projects by default. The local settings editor allows you to override those global settings and render each project with unique render settings and output modules.

In the **Plugins Override** tab, select the **AeFarmerPlugin**. Check the properties you'd like to override and indicate the render settings or the output module you like to use for this specific project.
The local settings will override the global settings that were set in the Farmer. You can override all or just some of the settings combining the local and the global settings together. For example, you can still use
**Best Settings** for the Render Settings that are set by default in the Farmer (globally) and use a different Output Mobile to render the video with a different codec.

# Logos & Thumbnails
When an After Effects project is saved in the Watch Folder, the folder containing the project and the project itself will show up in Template Farm with default thumbnails. You change the thumbnails to give your editors
a better experience and a clearer understanding of what what each folder contains and what each project and template should look like.
You can also change the logo at the top left (in the header) to create a more brand related experience (only for enterprise accounts).

## Folder Logo
To change the default Template Farm logo on the top left of the screen, add a PNG file named **_logo.png** to your folder.
When a user navigates into any folder that contains a PNG file called **_logo.png** the logo will be replaced with the content of the file.

## Folder Thumbnails
In order to change the default thumbnail of a folder, add a JPG file named thumb.jpg to your folder.
Users will see this thumbnail before they navigate into the folder.
> For best presentation and speed keep the thumbnail files an a 16X9 aspect, and around 480x270 pixels in size

## Project Thumbnails
In order to change the default thumbnail of a project, add a JPG file named exactly as your project.
For example: if your After Effects project file name is **Titles.aep**, add a JPG file called **Title.jpg** next to it.
The users will see this thumbnail before they open the project.
> For best presentation and speed keep the thumbnail files in a 16X9 aspect, and around 480x270 pixels in size

## Template Thumbnails
In order to change the default thumbnails of templates within a project, add a JPG file named as your project name followed by an underscore and the template name.
For example: If your project file name is **Titles.aep** and the template name is **LowerThird**, add a JPG file named **Titles_LowerThird.jpg** next to it.
The users will see this thumbnail when they open the project.

> The thumbnails of the templates are also used for provide the initial preview that users will see before the preview content on their own.
You might want to have a bigger resolution for those files sine they are presented as a preview. In order to improve performance and speed, try to lower the JPG files quality to as low as it would be acceptable by you.


