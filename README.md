# GAE
Google App Engine (easier than GCE)

![](https://raw.githubusercontent.com/QueenieCplusplus/GAE/main/gae.jpg)

a very easier Serverless way (than GCE & GKE) to create, maintain, scale up for the changing network traffic & stoage needs and so on, such as LB, microservices, noSQL DB, memcache, revision and rollback/rollout.

since the GCE is an IaaS solution, GCP provides devops the SaaS solution got the same result from GCE + GKE, helps developers to create env for their app (website) using GAE + Cloud Run.

serverless see https://github.com/QueenieCplusplus/CloudRun/blob/main/README.md

# yaml file 

all config info for GAE in app

https://cloud.google.com/appengine/docs/flexible/nodejs/configuring-your-app-with-app-yaml

# support Hosts 

    * Nodejs

    * PHP

    * Go

    * Java

# Core Steps:

(1) create a Nodejs App

(2) deploy app in GAE instance

(3) without Downtime, revsion (rollback/rollout) the App

-----

# Nodejs App

from step 1

> to clone, install, and start the App server.

* 1.1 open cloud shell

* 1.2, clone the app from github

       git clone https://github.com/QueenieCplusplus/Simple_Nodejs_App.git

* 1.3, install the app in shell

       cd Simple_Nodejs_App
       
       cd flexible
       
       npm install

* 1.4, start the npm server

       npm start

* 1.5, stip the npm server

       ctrl + c
       
# GAE API

from step 2

> to activate (enable) the API Service in Cloud console.

* 2.1, in cloud console, navigate to API & Services > Dashboard.

![](https://raw.githubusercontent.com/QueenieCplusplus/GAE/main/activate%20api.png)

* tips & attentions:

App Engine uses a file called app.yaml to describe an application's deployment configuration. If this file is not present, App Engine will try to guess the deployment configuration. However, it is a good idea to provide this file.

Open app.yaml to see what it contains. You can use vim, nano,or emacs to view the file.

* 2.2, in cloud sell, open the app.yaml file to see the code line as following.

         runtime: nodejs
         env: flex
         
* 2.3, to deploy app into GAE.

         gcloud app deploy

Region Selection

* 2.4, to select the nearest region in prompt for app to deploy, then type "y" to say yes.

      Because this is the first time you are deploying App Engine, the tool will prompt you to select a location to deploy the app.

* tips & attentions:

It takes time sunce the App Engine environment automatically provisions a (GCE) Compute Engine virtual machine for you behind the scenes, and then installs and starts the application.

Browe URL

* 2.5, after deployment, we got an URL which is a new url in browser, this proj-id is as same as proj-id in "Connection Details" Tab in c;oud console.

       http://<proj-id>.appspot.com 

# Modify Web App

from step 3:

> install new lib, add some codeline, and re-deploy app without Downtime.

* 3.1, in cloud shell, install new lib.

      npm install uuid@^3.1 --save
      
* 3.2, add some codeline to app.js

      vi app.js
      
      i
      
      [app.js]
      
      const uuid = require('uuid/v4');
      
      ...
      
      .send(ˋHello, ${uuid()}ˋ)
      
      :wq
      
* app.js

      'use strict';

      // [START gae_flex_quickstart]
      const express = require('express');
      const uuid = require('uuid/v4');

      const app = express();

      app.get('/', (req, res) => {
        res
          .status(200)
          .send(`Hello, ${uuid()}!`)
          .end();
      });

      // Start the server
      const PORT = process.env.PORT || 8080;
      app.listen(PORT, () => {
        console.log(`App listening on port ${PORT}`);
        console.log('Press Ctrl+C to quit.');
      });
      // [END gae_flex_quickstart]
      
 * 3.3, we can test app locally in cloud shell and console.
 
       npm start
       
 * 3.4, and see the result in the browser, after clicking on "preview on port 8080" tab in console.

# Revsion Web

* 3.5, run app after typing the cmd line in shell.

       gcloud app deploy
       
 * 3.6, see new web page which is deployed using the url 
 
       http://<proj-id>.appspot.com 
 
 * 3.7, or use cloud shell to see the deployment result.
 
       gcloud app browse
