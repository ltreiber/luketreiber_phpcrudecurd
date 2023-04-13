phpcrudecrud_deployment.md
Luke Treiber

This is a guide to assist in the deployment of the PHP application. The guide will walk you through creating and setting up the environment to allow for the PHP crude CRUD application to run.
Purpose

This application was designed to teach a user how to set up and implement a basic PHP Application. The Application runs on a webserver and allows a user to have full CRUD (Create, Read, Update, and Delete) operations from a browser connection.
Overview

The guide will begin by explaining how to create and install an Ubuntu Virtual Machine. The virtual machine will act as both our Webserver (Apache2) and Database. We will need to set up a connection to the database. From this, we will be able to deploy our PHP application which can be accessed via a connection.
Virtual Machine Configuration

While the Virtual Machine configuration will depend on how many users you expect to connect, the application is quite lightweight. Thus, requirements for our virtual machine are quite low.

    CPU: 1 Core
    RAM: 2 GB
    Disk: 10GB

Setting up VM

This guide is assuming that Virtualbox is being used and has already been installed. Here is a link to the Virtualbox download page: Virtualbox Downloads

    Let's launch Virtualbox and click the blue star button in the top-row.
    Next, let's name our virtual machine: phpcrudecrud_VM
    Download Ubuntu 20.04 ISO
    Select this download as the ISO image in Virtualbox
    Click the dropdown unattended install and add your login information you want to use to log in to your Virtualbox.
    Under the Hardware section, the default settings are okay. But, verify they are with the requirements listed above.
    Under the Disk section, feel free to reduce the space to 10GB or keep it at the default.
    Hit Enter on your keyboard and wait for the VM to boot!

Installing Ubuntu 20.04 Server

    When the Virtual Machine is launched, you should select your preferred language.

    Next, continue without updating the installer.

    Many of these configurations can remain as default by using the arrow keys to select DONE. Done with highlighted as green when it is selected.

    Create your profile
        Add your name and make the server's name <yourname>phpcrudecrud
        Create a username to login
        Create and confirm your login password

    NOTE: Please ensure that you have recorded the username and password. Otherwise, you cannot log in.

    Enable the installation of OpenSSH server

    Wait for the Installation to complete
    This will be indicated in the top left and highlighted in orange.

    Shutdown the Virtual Machine.

Setting up network (HIGHLY RECOMMENDED)

This set allows us to use SSH to access the machines. You can skip this if you only want to use the virtual box.

    While the Virtual Machine is turned off, let's change its 'hardware', so we can use SSH.
    Under tools, create a host-only adapter
    The adapter should be configured automatically and have DHCP server enabled
    Now, select our virtual machine back on the homepage and hit settings. This should display a pop-up window with the name of the VM.
    On the left side, there is a network menu.
    Under this menu, select and enable
