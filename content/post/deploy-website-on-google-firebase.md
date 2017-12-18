---
title: "Deploy and host your website on Google Firebase"
date: 2017-11-26
draft: false
description: "Deploying and hosting your website on Google Firebase service is easy and free of cost."
keywords: "WebHosting,Google,Firebase"
categories: [ "Resources And Guides" ]
image: "/img/firebasehosting/add-new-project-in-google-firebase-thumb.png"
logothumb: "/img/logothumb/firebase.png"
tags: [
    "Web Hosting","Google Firebase"
]
---

After finishing the first draft of my [new website](https://www.pawangaria.com) I wanted to host my website on [firebase](https://firebase.google.com/) because I found Firebaseas hosting server very useful, easy to use and free of cost for limited hosting.

So I decided to write down my knowdledge and experience of hosting website with firebase here and if you are also looking for hosting your website in firebase please follow thesteps below.

### Create account or Login on Firebase Console

To deploy your website on firebase first you need to create an account on [Google Firebase](https://console.firebase.google.com/). For Account, you can use your normal Gmail user or create new Gmail account. After successful login to Firebase console, you can easily create a new project.
![google-firebase-addProject-page](/img/firebasehosting/add-new-project-in-google-firebase.png)

### Setup of Firebase CLI
Firebase hosting and deployments are done by the use of Firebase CLI (command line tool). CLI depends on [node.js](https://nodejs.org/en/) and npn so before installing Firebasewe need to install node.js and npm in your local machine.[Reference](https://changelog.com/posts/install-node-js-with-homebrew-on-os-x)

#### on Mac
```
brew install node
```
#### on Windows
Download from [nodejs download](https://nodejs.org/en/download/) and follow installation instructions.

After installing node and npm you can verify the installation and check the version
```
node -v

npm -v

```
You will see the version installed after running the above commands.

### Install Firebase CLI
Now after node.js and npm installed properly we can install the Firebase CLI using the below command
```
npm install -g firebase-tools
```
### Initialise website setup with firebase
Go to the base directory of your website project in the command line and run the command
```
firebase init
```
and then follow the instructions on the terminal

![google-fireBase-init-website-command](/img/firebasehosting/google-firebase-init-command.png)

After firebase init you can see one firebase.json file generated in your website directory. This JSON file is a  basic configuration file for the Firebase deployment. You can find more details about [firebase.json](https://firebase.google.com/docs/hosting/deploying#section-firebase-json).

### Deploy your website to Firebase
Now your website is ready to depoy on the firebase server.Run the following command from the base directory of your website
```
firebase deploy
```
![google-fireBase-deploy-command](/img/firebasehosting/google-firebase-deploy-command.png)

### Access your website
After successfull deployement of your website you can get your website url from firebase account and generally URL's looks like this

**yourProjectName-ID.firebaseapp.com**

![google-fireBase-hosting-page-and-website-url](/img/firebasehosting/google-firebase-web-hosting-url.png)

Now you can view and open your website in any of the browsers using the above URL.

I took the installation reference from [firebase docs](https://firebase.google.com/docs/hosting/deploying#section-hosting-setup).

### What is Next Step?
Now the next step is to connect your hosted website on Firebase with Real Domain. I will cover this as separate topic in the [next articles](https://www.pawangaria.com/post/mapcustomdomainwithgodaddyandfirebase/).

Thanks for reading!
